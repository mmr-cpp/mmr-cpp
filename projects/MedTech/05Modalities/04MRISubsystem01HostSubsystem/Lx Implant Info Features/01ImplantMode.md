# IEC 4th Edition MR Implant — TIR Communication
## Requirement:
**As a**: MR Host Engineer
**I want to**: Implement communication with all host systems
**So that**: Implant data can be shared across all system components
Goal: Transmit implant device/info data (UDI, Model, Serial#, Type, Manufacturer) from the Java WFM into DICOM output via TIR's lx2dicom library, keyed by exam number.

Now, explain changes of Implant Ui creation (where implant off/on there ) and all below points: //interview point of views //scenario based
Implant mode UI creation -> java swing, java
storing implant data from UI to Backend
TIR communication (user addimplant row, Implant data store, print (jvm) -> TIR (C++)
TIR to Lx2DICOM : "c extern" used to communicating between c & c++

Lx2DICOM to image creation process to real image (DICOM)

Techs Used: 
Java->c++->c
Serialization (write), Deserialization, IPC (Socket (TCP/IP))/GEOCOMM.

Implementation Required for Implant mode off/on:

<img width="545" height="380" alt="image" src="https://github.com/user-attachments/assets/1b66b729-72ce-4506-80bc-d5b5edb006ae" />

<img width="940" height="739" alt="image" src="https://github.com/user-attachments/assets/e4d60f2c-a41b-415e-a35e-ec314f9c44eb" />

<img width="940" height="414" alt="image" src="https://github.com/user-attachments/assets/31111c19-53d9-48b4-8d99-ab4165505000" />

<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/d3aad8d0-acc0-4594-be58-74bfabff8002" />

<img width="940" height="533" alt="image" src="https://github.com/user-attachments/assets/e23327cc-ad2d-4642-92ae-2f1d2c7927fa" />

# HLD:
<img width="940" height="753" alt="image" src="https://github.com/user-attachments/assets/69468cf8-6c33-4f66-8624-c71bf8bfb299" />


# Sequence Diagram: complete picture showing exactly how data transfers from Java to C++:

<img width="940" height="516" alt="image" src="https://github.com/user-attachments/assets/2f1c39d6-eb8c-4d8a-b9b0-25bb80c3bee9" />

The communication to SeriesDataTIR is asynchronous (fire-and-forget).
In the onMessage handler (ScannerService.cc:1510):
GEOComm::send("SeriesDataTIR", pkt);
GEOComm::send() is a one-way message dispatch — it sends the ImplantInfoList packet to SeriesDataTIR without waiting for a reply or acknowledgment. 
There is no return value checked and no reply object involved, 

unlike the onRequest/reply pattern used elsewhere in the same class (e.g., onRequest(CVsRequest*, CVsRequest*)) which is synchronous (request/reply pair).


Several design reasons favor the asynchronous GEOComm::send() here:
1.	onMessage is itself async — ScannerService::onMessage() is a one-way GEOComm message handler (registered via GEOCOMM_onMessage),
   	meaning the Java WFM already fired-and-forgot when sending ImplantInfoList to ScannerService. There is no reply channel back to the original sender,
  	so using a synchronous call to TIR would block for no benefit.
2.	Decoupling / non-blocking scan path — ScannerService runs in the scan task context. Blocking on a synchronous RPC to SeriesDataTIR (which may involve disk I/O
  	or database writes) would stall the scanner service thread, potentially impacting scan control responsiveness.
3.	TIR is a storage sink — SeriesDataTIR stores implant data for later series association.
  	There is no data that needs to come back to the scan side in real-time; the result of the store is not needed to proceed with scanning.
4.	Error handling is deferred by design — If TIR storage fails, that is handled on the TIR side (e.g., logging, retry).
  	The scan side has no corrective action it could take synchronously anyway.
  	
    When synchronous would be appropriate is if ScannerService needed a confirmation from TIR before allowing a scan to proceed (e.g., "implant data stored — safe to scan").
  	That would require a request/reply (GEOComm::request()) pattern, similar to how onRequest(CVsRequest*, CVsRequest*) works.
  	That design choice would need to be a deliberate architectural decision, likely involving changes upstream in the WFM flow as well.

    <img width="940" height="764" alt="image" src="https://github.com/user-attachments/assets/4331bdb2-1da2-4fcf-8216-5b2a69c260c7" />

   ## ****************** TIR Comm (UI to Backend) ***********************

   <img width="940" height="761" alt="image" src="https://github.com/user-attachments/assets/6c6e8f76-dac8-4168-9ee1-4bd7dd999fb1" />

   <img width="940" height="724" alt="image" src="https://github.com/user-attachments/assets/302da445-eaaf-4459-8a52-70d735c809a9" /> 

   <img width="940" height="671" alt="image" src="https://github.com/user-attachments/assets/4b47d56c-c7b7-493b-80ac-ceadbcd4ae01" />

   <img width="940" height="701" alt="image" src="https://github.com/user-attachments/assets/ced6a666-6586-472d-bb16-5de9306cdf95" />

   <img width="940" height="722" alt="image" src="https://github.com/user-attachments/assets/6337ffe9-9e96-416f-8ce1-d2b39f98b894" />

   <img width="940" height="709" alt="image" src="https://github.com/user-attachments/assets/a29d090f-f7ac-444f-8d7c-71384d92d648" />

   <img width="940" height="696" alt="image" src="https://github.com/user-attachments/assets/603e8a63-55cb-4918-bb90-588be1cb6faa" />

   <img width="940" height="759" alt="image" src="https://github.com/user-attachments/assets/1b16b546-0efc-45c3-8d05-5849ef03a531" />

   <img width="940" height="736" alt="image" src="https://github.com/user-attachments/assets/62f6d2d9-9ae3-4a8b-9701-058a7edc50ef" />

   <img width="757" height="491" alt="image" src="https://github.com/user-attachments/assets/8f354bdb-ff2f-4f43-bde1-526731196c88" />

   <img width="940" height="574" alt="image" src="https://github.com/user-attachments/assets/be6b523b-2050-45d4-a659-f41925931b86" />

   <img width="940" height="576" alt="image" src="https://github.com/user-attachments/assets/b1774eaa-30dc-4cc7-8d4a-5aee2b78cb89" />

   <img width="940" height="748" alt="image" src="https://github.com/user-attachments/assets/ee86c273-d11a-4458-a2bc-3781f2c3c1b4" />

   <img width="940" height="630" alt="image" src="https://github.com/user-attachments/assets/8ef0a05c-1fba-4ffc-9452-2267f26a3482" />

   <img width="940" height="675" alt="image" src="https://github.com/user-attachments/assets/b3553611-22d7-4664-9bdc-ca79f4dd4a3a" />

   TIR image pipeline
  │
  ▼
lx2dicom(mrimg, dcimg, size, err)       ← lx2dicom.h extern C interface
  │  allocates ge2dcmPvt context
  │  reads exam->ex_no = 71
  ▼
setImageHeader(pvt)                      ← iterates ArrayOfDicomTags[]
  │  hits {0x0043,0x00E2, SQ, child_table, setImplantDetails}
  ▼
setImplantDetails(ptr, SQ_BEGIN)         ← state machine starts
  │  getImplantCount(71)                 ← extern C → ImplantInfoPktStore.cc
  │    m_implantInfoCollection[71].size()
  │    returns 2
  │  → CONT_PROCESSING
  ▼
  [for each implant 0..1]
    setImplantDetails(ptr, E4..E8 leaves)
      │  getImplantField(71, idx, 0..4, buf, 2048)
      │    m_implantInfoCollection[71][idx][fieldIdx]
      │  asciiTextFormat() → validate
      │  setAttributeValue()
      │    → MC_Set_Value_From_String(MsgID, tag, value)
      │                          ↑
      │                    MergeCom DICOM lib writes tag to buffer
    setImplantDetails(ptr, SQ_ITEM_END)
      │  implant_cur_idx++
      │  → CONT or END_PROCESSING
  [end loop]
  │
  ▼
MergeCom flushes buffer → .dcm file on disk

<img width="940" height="577" alt="image" src="https://github.com/user-attachments/assets/e7297142-cf62-4065-a940-ec327ddb8655" />

<img width="940" height="275" alt="image" src="https://github.com/user-attachments/assets/3860e28c-2f35-4c16-bd0c-78ad4fcdd752" />

## Tag/Element should be written in DICOM Image Header (imaged->select current exam and series->Tools)
   Know Process, how tag is writing on DICOM Image Header?
   /c/uws/ms021387/mdmodabbir.raza_rhino_IEC4ThEdLxMRImplant_5159/host/foundation-libs/lx2dicom/gemr2dcm.c // C files 
   a) Tag Table Entry //Parent tags are written based on the group or requirement
      #define g43_id "GEMS_PARM_01"
      /* MROC Sequence: parents MROC scalars (10D7-10DF) + implant sub-sequence (00E2) */
    {{0x0043, 0x10F0}, {0, 0, 0}, SQ, merge_ref_mroc_seq, setMrocSequence},
    0x0043 //group
    0x10F0 //tag/element 
    VR : SQ (sequence)
    merge_ref_mroc_seq //Child tag table for the MROC Sequence (0043,10F0).
  * Tags 10D7-10DF are scalar MROC parameters (dummy values via
  * setMrocDetails).
    setMrocSequence //callback

b) Leaf tags of MROC sequence (a) and sub sequence and it’s child:

   <img width="940" height="359" alt="image" src="https://github.com/user-attachments/assets/d19b68f4-c33c-4967-9460-8dea09e94e42" />
   Note: (0018, 1320) piB1rms // it’s private tag can’t write in tagAddition.xml

### Following Binaries need to be patch during integration testing on any target/VM.
1.	scn
2.	cim
3.	mrWfm.jar
4.	implant-lite-1.0.0.jar
5.	liblx2dicom.so.1
6.	TIR
7.	gemriutil.jar
8.	/export/home/wfm/host/scripts/wfm_env.sh - add CLASSPATH=${CLASSPATH}:${WFM_MR_TOP}/implant-lite-1.0.0.jar
9.	/export/home/signa/w/config/implant.cfg  implant_mode="ON" - not required remove this . we depend on option key to show the option to user
10.	tagAddition.xml - need to update MROC attributes to see these all attributes in header

<img width="940" height="612" alt="image" src="https://github.com/user-attachments/assets/60cc9535-71e0-493d-bfef-8e13b8ec7ba8" />

Q2) Static library and Dynamic library:
Static Library (.a — Archive)
Definition: Code is copied into the executable at link time. The library is no longer needed after build.

<img width="940" height="545" alt="image" src="https://github.com/user-attachments/assets/6a6c00aa-4f69-4c18-ad7d-c75058dd1673" />

Dynamic Library (.so — Shared Object)

<img width="940" height="432" alt="image" src="https://github.com/user-attachments/assets/24f6b19e-6975-4653-a216-4a2ea3c9eb9d" />

 
Key Differences Table
Aspect	            Static (.a)	                   Dynamic (.so)
Link time	          Build time	                   Runtime
Binary size	        Larger (code copied in)	       Smaller (just a reference)
Runtime dependency	None	                         .so must exist on system
Memory usage	      Each process has its own copy	 Shared across all processes
Update/patch	      Must recompile entire binary	 Replace .so — no rebuild needed
Load speed	        Faster (already in binary)	   Slightly slower (dynamic loading)
Portability	        Fully portable	               Platform dependent

<img width="732" height="285" alt="image" src="https://github.com/user-attachments/assets/98ca9645-464c-425d-9eb0-47b99a434044" />

<img width="478" height="368" alt="image" src="https://github.com/user-attachments/assets/9b22adc1-6988-4a0b-9202-2ba03fe37b10" />

libmsa.a — Static Library
•	The .a extension means archive (static library)
•	Object files from RxMgrDownload.m and other sources are copied directly into the scn executable at link time
•	No separate file needed at runtime — code is baked into scn
•	Larger binary size, but self-contained
libScanUIF.so.1 — Dynamic/Shared Library
•	The .so extension means shared object (dynamic library)
•	Code stays in a separate .so file on disk
•	The scn executable loads it at runtime (not link time)
•	Multiple programs can share the same .so in memory
•	Smaller binaries, but the .so must be present on the system at runtime

<img width="940" height="1028" alt="image" src="https://github.com/user-attachments/assets/71c914d6-a90c-450e-8fec-41c3dc89e31e" />
 
## Cx Tech stack
Building blocks: Spring (services), Java/C++, HTML5 + AngularJS/Angular, REST APIs.
Tools: Perforce / Git, Jenkins, SonarQube, IntelliJ, Karma + Jasmine + JUnit, Docker, Node.js, Grunt, Protractor.

<img width="940" height="289" alt="image" src="https://github.com/user-attachments/assets/9420124d-371a-412c-93ef-a90d6941e2ee" />

<img width="940" height="1411" alt="image" src="https://github.com/user-attachments/assets/b10c5a76-f008-4df0-bf7e-68499edc8f03" />

## Key point for interviews: "Makefile is the machine-facing file; Imakefile is the human-facing file. Imakefile is to Makefile what a template is to a rendered document."
1. Imakefile.common (imake template)
What: A platform-agnostic, human-written template that describes what to build — source files, include flags, library names. Uses macro variables ($(TOP), $(VPATH), etc.).
Why: Avoids duplicating build logic for every platform (host, sim, vre32, vre64, etc.). You define sources once, and the tool generates platform-specific Makefiles from it.
When: You edit this when you add/remove source files or change compile flags. You never run it directly — it's input to the imake tool.
Flow:
Imakefile.common  --[imake + cpp]--> Makefile (host)
                                 --> Makefile (sim_linux)
                                 --> Makefile (vre64)  ...
2. Makefile (generated output)
What: The actual file executed by the make command. Contains fully resolved, platform-specific compile rules, object lists, and link commands.

3. CMakeLists.txt (modern CMake)
What: A modern, cross-platform build definition that generates Makefiles, Ninja files, or Visual Studio projects depending on the target.
Why: Solves portability problems that imake/make struggle with — handles dependency discovery, compiler detection, and IDE integration natively.
When: Used in newer projects or when migrating away from imake. You write CMakeLists.txt, run cmake, and it produces the final Makefile / Ninja build file for your platform.

<img width="702" height="395" alt="image" src="https://github.com/user-attachments/assets/e8e16e9c-fa46-49b7-af68-0aa62ba09391" />

"If I add a file to Imakefile.common, does the build automatically pick it up?"
Answer: No — not until you re-run imake to regenerate the Makefiles. This is why in legacy repos with stale Makefiles, you must update both. Ideally, imake regeneration is part of the CI/build step so they stay in sync automatically.


## Interview Questions: on which module uh have worked, what’s ur last contribution?
1) what’s challenging bugs uh have fixed? 
2) what’s process have uh followed for this?
3) Tools and Technologies, why this ? 

Ans: I have Implemented Implant Features, where taking value or info from UI, when I switch to ImpalntMode on, Then addimplantrow->agree consent and Accept.
     We are sending info from UI to backend as below flow:
     SarDBDialogue.java->MRsession.java->SeriesDataEndPoint.cc (TIR)->lx2dicom.h (store)->ImplantInfoPktStore.cc->gemr2dcm.cc and then writing in to DICOM std.

for below tags like B1RMS, maxslewrate and spatial gradient…
added changes in specific file like:
RxMgrDownload.m, SrxIPScreen.m  
why are we adding and what this files do?
Ans:
SarDBDialog.java
step1)     private final   ArrayList<ImplantInfo> implantInfoArrayList = new ArrayList<>();              // List to store implant information objects. 

<img width="712" height="239" alt="image" src="https://github.com/user-attachments/assets/0898b1d6-ecdd-469c-940d-5e75e8f6c8ce" />

Q1: How does implantInfoList.get(i) work?
implantInfoList is a standard Java ArrayList<ImplantInfo>. ArrayList.get(i) returns the element at index i by direct array access (O(1)). The list was populated before calling doSetImplantMode() — specifically in SarDBDialog.java via rebuildImplantInfoList(), which builds implantInfoArrayList by reading each row of the implant table UI, then passes it as the implantSnapshot.
Q1.1) How Implant pannel called?      //called from createJpanel inside SarDbDialoge.java

<img width="604" height="176" alt="image" src="https://github.com/user-attachments/assets/17f0f77d-0330-4815-9021-2caaa078fea0" />

Q2) How have uh created UI part of Implant Mode Settings Panel?
<img width="940" height="668" alt="image" src="https://github.com/user-attachments/assets/7ed66009-c586-415f-aee5-649ec29cb151" />

Q3) From where we call addimplantRow()? How it’s syncing data, if uh make any changes?
<img width="940" height="731" alt="image" src="https://github.com/user-attachments/assets/677e59ff-4c42-47d6-809e-bc77bf979728" />
<img width="848" height="1045" alt="image" src="https://github.com/user-attachments/assets/691991ee-adfd-4eb8-839b-0424078bad83" />
<img width="940" height="879" alt="image" src="https://github.com/user-attachments/assets/1fff2235-e1c9-4167-a91c-417686b79864" />

Q4) How are we storing Implant Info data in to List?
<img width="940" height="704" alt="image" src="https://github.com/user-attachments/assets/91207c7c-634b-4ddb-9c95-2c37ace4a8d5" />

//Remove all implant Row
<img width="651" height="917" alt="image" src="https://github.com/user-attachments/assets/d10e0f76-1c0f-48ed-8789-65556bc28398" />
//Restore ImplantRows under
 private void updatePrefObject()
    {
   // Restore implant table rows
        restoreImplantRows();-> rebuildImplantInfoList
Q5) How are we sending to MRsession.java?  SarDBdialoge.java->MRSession.java
   /*** Process Accept button event.
     * @param event The action event
     */ private void acceptActionPerformed(ActionEvent event)
    {
<img width="926" height="395" alt="image" src="https://github.com/user-attachments/assets/84401030-f49f-4430-9527-68c5b5f8d1a5" />

Used this import gemriutil.util.GeSerializable; //class 
<img width="726" height="567" alt="image" src="https://github.com/user-attachments/assets/22f71122-c455-49ef-afc6-7e9fbadc63c9" />

Q1) If storeImplantInfoPkt () method with 3 args called from SeriesDataEndPoint.cc then How it Interlinked with lx2dicom.h, implantInfoPktStore.cc and gemr2dcm.c?
<img width="645" height="654" alt="image" src="https://github.com/user-attachments/assets/7db78a88-a3be-4632-a9a3-e5017095f32f" />

<img width="940" height="621" alt="image" src="https://github.com/user-attachments/assets/aa83928a-d064-4333-b304-b834aec59ec6" />

<img width="847" height="806" alt="image" src="https://github.com/user-attachments/assets/a53afac8-0b60-4912-aa0a-58425b91c7d4" />

<img width="940" height="759" alt="image" src="https://github.com/user-attachments/assets/c0403ff9-6065-4702-be64-a3dc7b509fec" />

<img width="900" height="835" alt="image" src="https://github.com/user-attachments/assets/4151b059-91cb-4933-a56d-aa8a375fbde7" />

 gemr2dcm.c is the consumer of the stored data.
So the interlink is based on two things:
1.	Function declaration/implementation linkage
o	lx2dicom.h declares storeImplantInfoPkt(...)
o	ImplantInfoPktStore.cc implements it
o	SeriesDataEndPoint.cc calls it
2.	Runtime data linkage
o	examNo is used as the key
o	SeriesDataEndPoint.cc stores data under examNo
o	gemr2dcm.c retrieves data using the same examNo when writing DICOM tags.
<img width="870" height="839" alt="image" src="https://github.com/user-attachments/assets/ccdfdf67-c469-40be-ac2e-bd10c7beb4b6" />

2Q) For some tags like img->piB1rms, we are making changes in imageDB.h, RxMgrDownload.m, SrxIPScreen.m and Ifcc
but for implantInfo related tags like: Device UDI, Device Model we are simply passing data from SarDBDialog.java->MRSession.java->SeriesDataEndPoint.cc->gemr2dcm.cc

<img width="940" height="766" alt="image" src="https://github.com/user-attachments/assets/3e4a53b6-35dd-4a39-b117-cea6b73f1c30" />

<img width="940" height="637" alt="image" src="https://github.com/user-attachments/assets/8965ff59-d64f-4bbb-b50e-be27713e6dc4" />

<img width="839" height="832" alt="image" src="https://github.com/user-attachments/assets/05ee44cc-fcd5-44de-8593-2208caef38b3" />

<img width="940" height="780" alt="image" src="https://github.com/user-attachments/assets/abe33b03-5906-4a6f-a3e7-92df7b9f682d" />

<img width="940" height="464" alt="image" src="https://github.com/user-attachments/assets/26829cd4-a47b-48d2-a23b-b8eadf96e55e" />

<img width="940" height="599" alt="image" src="https://github.com/user-attachments/assets/a14f36f4-0f5c-49c9-bfba-44d5c2a64b88" />

So they are treated like implant/MROC metadata coming from session/SAR workflow, not like img->maxSlewRate.

Conclusion:
<img width="663" height="801" alt="image" src="https://github.com/user-attachments/assets/a0213be8-fb9f-4f54-a47f-93a371d1e835" />

//Padding:
<img width="738" height="550" alt="image" src="https://github.com/user-attachments/assets/8c388284-e279-45a7-a088-b219133f6f3a" />

RxMgrDownload.m
<img width="940" height="882" alt="image" src="https://github.com/user-attachments/assets/7dd6bea2-91b4-42f5-8e46-d80596c9cef8" />
 allocBuffer->rxInfo.seriesInfo.zLoc = zLoc;
    if(pmid != nil)
    {getCvValue(pmid, CV_pib1rms, &piB1rms);
    } else
    {  piB1rms = 0.0;
    }
allocBuffer->rxInfo.scanInfo.piB1rms = piB1rms;

<img width="940" height="418" alt="image" src="https://github.com/user-attachments/assets/70a67094-e3b6-4b64-9908-2eb1ce32f5bd" />
<img width="638" height="717" alt="image" src="https://github.com/user-attachments/assets/bc3af86f-f95f-4132-9853-e827b37c699d" />
<img width="834" height="623" alt="image" src="https://github.com/user-attachments/assets/91785700-1329-44b3-83bd-f459711d2056" />

case 0x181320:  
            snprintf(valptr, MY_STR_LEN, "%g", img->piB1rms);
            break;


******************************Interview Point of View********************************
Q1) Can you explain the end-to-end system design and processing flow for an Implant Mode pipeline that includes UI creation, backend data storage, and communication from a JVM-based service to a high-performance C++ TIR engine, followed by integration with an Lx2DICOM imaging module using extern "C"?
Please cover how implant parameters like piB1rms and maxSlewRate are handled, and explain the full data flow from UI to final DICOM image generation.
Additionally, describe the C++ concepts, STL containers, DSA usage, threading model (sync vs async), IPC mechanisms (where and how used), and the system design aspects including HLD, LLD, SOLID principles, and design patterns involved.”
Below is the end-to-end Implant Mode / MROC pipeline design from UI to DICOM.

<img width="579" height="925" alt="image" src="https://github.com/user-attachments/assets/b9b8ec6e-5ebf-44a5-8d41-97bb67281005" />
<img width="507" height="539" alt="image" src="https://github.com/user-attachments/assets/378baeac-968f-48c0-9dc1-fdb33962fe1e" />
<img width="732" height="814" alt="image" src="https://github.com/user-attachments/assets/4a029875-f1cd-4077-bc3e-33b6cbe9f375" />
<img width="764" height="694" alt="image" src="https://github.com/user-attachments/assets/15729e34-420d-416a-8375-a41db8ca9666" />
<img width="745" height="773" alt="image" src="https://github.com/user-attachments/assets/c2042d39-da29-4815-8372-8a8188eeb534" />
<img width="940" height="821" alt="image" src="https://github.com/user-attachments/assets/5c53e11e-f58b-4647-9095-881297418b9e" />
<img width="867" height="789" alt="image" src="https://github.com/user-attachments/assets/eb25e8d5-66d1-4648-8d54-9e842c1d061c" />
<img width="940" height="819" alt="image" src="https://github.com/user-attachments/assets/fe7cbecd-fcfa-47ea-a6ef-b8af95b94596" />
<img width="789" height="884" alt="image" src="https://github.com/user-attachments/assets/23720e72-75cb-409b-ab3d-d9e2ef090adc" />
<img width="940" height="865" alt="image" src="https://github.com/user-attachments/assets/0926fdf3-ee9d-4c49-a90e-7aabea79035f" />

<img width="940" height="627" alt="image" src="https://github.com/user-attachments/assets/9ed36225-5055-456f-910a-f2f1d805284b" />
<img width="789" height="747" alt="image" src="https://github.com/user-attachments/assets/39244353-9afb-42c6-9278-183f9cb43405" />
<img width="940" height="804" alt="image" src="https://github.com/user-attachments/assets/52ca744b-ff68-4606-99ab-127312d3905d" />
<img width="725" height="906" alt="image" src="https://github.com/user-attachments/assets/3bbf7550-b905-4479-aa8c-492d81f22e97" />
<img width="940" height="814" alt="image" src="https://github.com/user-attachments/assets/c23a4393-ea8f-4eed-ab3b-b37cc4e9bb8d" />
<img width="514" height="342" alt="image" src="https://github.com/user-attachments/assets/9b5d96ba-93a9-4ffd-880a-984713f308c4" />
<img width="681" height="653" alt="image" src="https://github.com/user-attachments/assets/6274156a-651a-4259-b850-6d9f199fbef8" />
<img width="585" height="904" alt="image" src="https://github.com/user-attachments/assets/601c0345-5e95-45db-a763-2df04294ccd3" />
<img width="628" height="694" alt="image" src="https://github.com/user-attachments/assets/9991db4f-5aeb-4ae5-b729-2e14e0cb1a4b" />
<img width="914" height="876" alt="image" src="https://github.com/user-attachments/assets/808eff2d-71fe-428f-9fb0-3df2bec13d55" />
<img width="940" height="652" alt="image" src="https://github.com/user-attachments/assets/0ec070ee-defa-4bf0-b176-1ffcf3a8f78a" />
<img width="678" height="910" alt="image" src="https://github.com/user-attachments/assets/4a3cdd83-d7b3-45a8-98fe-714d9fafac27" />

<img width="770" height="744" alt="image" src="https://github.com/user-attachments/assets/920ca0ba-9b93-4191-8ece-e6b30de19353" />
<img width="940" height="503" alt="image" src="https://github.com/user-attachments/assets/c5ae744f-e93d-483f-ac82-9918c8193f42" />

<img width="747" height="648" alt="image" src="https://github.com/user-attachments/assets/5d68d479-16db-4275-829b-ba96b8bbe8d3" />























































































