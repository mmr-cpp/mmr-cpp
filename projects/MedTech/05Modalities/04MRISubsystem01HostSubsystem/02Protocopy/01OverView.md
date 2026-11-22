# Component Overview 
ProtoCopy (Protocol Copy Tool, binary ProtEx) is an MR Scan software component launched from the Data Apps menu in Image Management.

It lets a user create a new site protocol from a previously scanned exam, without re-entering scan parameters manually, and publishes the result into the site protocol library.

<img width="494" height="275" alt="image" src="https://github.com/user-attachments/assets/a8b32758-3e65-4582-87d0-6b0eff4a2c5e" />


## Launch and Input Selection
The user first selects an item in the Image Browser — this can be an entire exam, a single series, or a single image within a series — and then opens ProtoCopy from Data Apps. 
ProtoCopy loads the corresponding exam/series/image into its own image viewer window regardless of which granularity was selected.

## Protocol Creation Modes
ProtoCopy supports two save modes, controlled by the Save Exam checkbox:

<img width="379" height="245" alt="image" src="https://github.com/user-attachments/assets/f918f878-4ad1-47e4-8399-f91278604678" />

In other words, the Save Exam checkbox — not the original Image Browser selection — determines 
whether the output protocol is a single-task or an all-series (multi-task) protocol.

## Protocol Metadata and Save Workflow
Before saving, the user:

1. Selects the required anatomy category.

2. Enters a protocol name.

3. Clicks Save As Protocol (optionally after checking Save Exam).

On success, ProtoCopy displays a "Protocol Save Successful" message in the ProtoCopy window, and the protocol is written into the local site protocol library.

## Version Upgrade
As part of the save pipeline, ProtoCopy runs protocolup, which automatically upgrades any protocol authored under an older MR_VERSION to the current system's release (e.g., upgrading to MR30.0#2) so the saved protocol is compatible with the local scanner's software baseline.

## Cross-Hardware Support
Because the source exam may have been scanned on a GE MR system with a different hardware configuration (field strength, gradient/coil setup) than the system running ProtoCopy, a hardware-mismatch advisory message may be shown; this is informational only (post–SPR HCSDM00138028) and does not block the save.

# References:
<img width="452" height="359" alt="image" src="https://github.com/user-attachments/assets/74cda9bc-3aaf-4dcc-af74-18092a694f4f" />

# Requirements:
Requirement1:  The protocol copy feature shall copy the scan parameters from the highlighted image in the Browser to a new protocol in the site library.

Requirement2: The protocol copy feature shall only copy the protocol when the field strength and gradient slew rates match between the image and the scanner performing the copy.

SPR (New Feature): HCSDM00138028 WF-enh. Protocopy should support different system configurations

Requirement3: The protocol copy feature shall have the ability to create a multi-series protocol by selecting an exam in the image browser before requesting the protocol copy.

# Source/Binaries:

<img width="383" height="272" alt="image" src="https://github.com/user-attachments/assets/bd51adfd-daa1-47aa-ad9c-955ed6665799" />

# Functionalities 

<img width="413" height="470" alt="image" src="https://github.com/user-attachments/assets/a1218556-8c72-4be7-a785-6b9abc6970bc" /> 
<img width="412" height="312" alt="image" src="https://github.com/user-attachments/assets/83438278-db76-4610-96c6-45778d2186e2" />

# Dependency
<img width="358" height="445" alt="image" src="https://github.com/user-attachments/assets/69c95153-bc96-4c93-b369-14ac4f6bb2b6" />
<img width="356" height="175" alt="image" src="https://github.com/user-attachments/assets/52a931ae-f3b1-474e-a262-af587e553e06" />

# System use cases:
<img width="353" height="443" alt="image" src="https://github.com/user-attachments/assets/46aaabfe-0ed8-45e8-ad95-f691018819ad" />
<img width="356" height="152" alt="image" src="https://github.com/user-attachments/assets/6cb2dc78-7e75-4edb-b86a-78130969868a" />

# High-Level Flow Diagram
<img width="1024" height="496" alt="image" src="https://github.com/user-attachments/assets/158ddbd3-708a-4fed-aa32-66d4afec3b32" />

# FlowChart_Protocopy_WorkFlow_MR_Protocols:
<img width="7665" height="1076" alt="01_FlowChart_Protocopy_wrkflow_MR_Protocols" src="https://github.com/user-attachments/assets/5859047f-84fb-4ede-aa30-97e3fd4a6391" />


# Class Diagrams

```mermaid
classDiagram
    direction LR

    class ProtExMain {
        <<module>>
        +main(argc, argv) int
    }

    class UxCProtExShell {
        <<UI>>
        +create_ProtExShell() Widget
        +activateCB_prtclbtn()
        +activateCB_quitBtn()
        +activateCB_reloadBtn()
        +prtclnametext Widget
        +btnAllSeries Widget
        +status_bar Widget
    }

    class ProtExCallbacks {
        <<module>>
        -ImageDB ProtExAiM
        -ProtCategory int
        -ProtName String
        -ErrMsgBuf String
        +initViewport()
        +updateUI(imagePath)
        +displayImage(imagePath)
        +ProtocolNameCallback()
        +ProtocolCategorySelected(num)
        +ProtExSaveCallback()
        +buildProtocolName(path) int
        +isDupSerNo(table, serno) bool
        +isDynaPlanSepSer(path, data) bool
        +isMolliSer(path, data) bool
        +upgradeProtocol(protname, destDir) int
        +insertProtocolIntoDb(destDir) int
        +ProtExCloseCallback()
        -displayStatusMsg(msg)
    }

    class ProtExData {
        +serno int
        +selSerPath String
        +data ProtExImageAttributes
        +sortCriterion(p1, p2) bool
    }

    class ProtExAiM {
        -ImageDB Aselection
        -_ready bool
        -MxImage bool
        -curSerNo int
        -curImgNo int
        -seriesTable PathTable
        -imageTable PathTable
        +init()
        +refresh()
        +buildSeriesTable(path, type)
        +buildImageTable(path, type)
        +getSelectedImagePath() String
        +getSelectedSeriesNumber() int
        +getSeriesCount() int
        +getImageType() bool
        +setImageType(isMx)
        +gotoNextSeries(direction) String
        +gotoNextImage() String
        +gotoNextExam(direction) String
    }

    class ProtExImageAttributes {
        -validProtInfo bool
        -MxImage bool
        -FieldStrength String
        -SRMode String
        -GCoilType String
        -GradientAmp String
        -compatibleConfig bool
        -postProc bool
        -modality String
        -imageSet String
        +setFieldStrength(value)
        +setSystemConfig(value)
        +setSoftwareRev(value)
        +setPostProc(imageType)
        +getConfigInfo() String
        +isCompatibleConfig() bool
        +isValidProtocol() bool
        +isPostProc() bool
        +isMxImage() bool
        +getSRMode() int
        +getGCoilType() int
        +getFieldStrength() int
        +getModality() String
        +getImageSet() String
    }

    class ImageAttributeUtils {
        <<module>>
        +getProtExImageAttributes(imagePath) ProtExImageAttributes
        +isPETModality(modality) bool
        +isPETSeries(imagePath) bool
        +isPETModalityAllowed() bool
        +getDMFTagValue(imagePath, tag) DMFValue
    }

    class Img2Prot {
        <<module>>
        +img2prot(img, prot, isMx, serCnt, serNo, isPET) int
        +createLxProtocol() int
        +createMxProtocol() int
        +createPETProtocol() int
        -writeProtocolHeaderSection()
        -writeSeriesGlobalSection()
        -writeSeriesSetSection()
    }

    class SDCSelectionAPI {
        <<external>>
        +ASdCSelectionOpen()
        +ASdCPrimaryImage()
        +AinitCommunications()
    }

    class TerraDMF {
        <<external>>
        +DMFDicomFileItemTemplate
        +DMFItem.getValue(tag)
        +TerraBlob
    }

    class ScannerConfig {
        <<external>>
        +getcfg(MRconfig.cfg, fieldStrength)
        +getcfg(GradientConfig.cfg, SRMode)
        +getProductProperty(key)
    }

    class ProtocolLibraryTools {
        <<external>>
        +protexists
        +protocolup
        +indexProtocol
        +protocol.tmpl
    }

    ProtExMain --> UxCProtExShell : creates
    ProtExMain --> ProtExCallbacks : initViewport
    ProtExMain --> ScannerConfig : reads modalities
    UxCProtExShell --> ProtExCallbacks : invokes callbacks
    ProtExCallbacks *-- ProtExAiM : owns ImageDB
    ProtExCallbacks "1" o-- "0..*" ProtExData : Save Exam list
    ProtExData --> ProtExImageAttributes : holds
    ProtExCallbacks ..> ImageAttributeUtils : uses
    ImageAttributeUtils ..> ProtExImageAttributes : creates
    ProtExCallbacks ..> Img2Prot : converts
    ProtExCallbacks ..> ProtocolLibraryTools : check, upgrade, index
    ProtExAiM ..> SDCSelectionAPI : reads Browser selection
    ImageAttributeUtils ..> TerraDMF : reads DICOM tags
    ProtExImageAttributes ..> ScannerConfig : compares hardware
    Img2Prot ..> TerraDMF : reads 0025,101B
    Img2Prot ..> ImageAttributeUtils : header values
    Img2Prot ..> ProtocolLibraryTools : reads template
```

 ProtExMain.cc: starts the Xt application, creates the UI, initializes communications and the viewport, then runs the event loop.

 ProtExShell.cc: creates widgets and registers Xt callbacks. The Save button registration calls Wrap_activateCB_prtclbtn.

 ProtExCallbacks.cc: receives UI actions and controls validation, the Save-one-image/Save-Exam branch, conversion, upgrade, and indexing. it drives the save workflow (single image for R1, full exam loop for R3). For R2, it displays the advisory. A mismatch does not stop either save branch.

 ProtExAiM.cc: reads the current Image Management selection and provides image, series, and exam navigation. Simply, it resolves the selection and builds series or image tables.

 ProtExImageAttributes.cc: reads DICOM metadata and checks whether protocol data is present and whether a series is eligible. it supplies hardware/compatibility metadata used only for an advisory warning (R2, post-SPR) → Img2ProtEngine (img2prot.cc) extracts and writes the Tcl protocol file → protocolup/indexProtocol publish it into the Site Protocol Library.

 img2prot.cc: extracts the embedded protocol payload and rebuilds the protocol file.
 
## Requirement mapping for Class Diagram: 
 <img width="341" height="491" alt="image" src="https://github.com/user-attachments/assets/45b99779-25be-46b5-a64e-a0717f9f3837" />


# Sequence Diagrams:
## sceanario#1 -R1: Copy scan parameters from the highlighted image to a new site protocol
```mermaid
sequenceDiagram
    autonumber
    actor User
    participant IM as ImageManagement
    participant Shell as ProtExShell
    participant AiM as ProtExAiM
    participant Cb as ProtExCallbacks
    participant I2P as img2prot
    participant Dicom as DicomImage
    participant PU as protocolup
    participant IP as indexProtocol
    participant DB as SiteProtocolDB

    User->>IM: Highlight image in Browser
    User->>IM: Click ProtoCopy in Data Apps
    IM->>Shell: Launch ProtEx with selection
    Shell->>AiM: init reads current selection
    AiM-->>Shell: Selected image path
    User->>Shell: Enter protocol name and category
    User->>Shell: Click Save As Protocol with Save Exam off
    Shell->>Cb: ProtExSaveCallback
    Cb->>Cb: isValidProtNm and buildProtocolName
    alt Name invalid or duplicate
        Cb-->>User: Show error and stop
    else Name accepted
        Cb->>AiM: getSelectedImagePath
        AiM-->>Cb: image path
        Cb->>I2P: img2prot for selected image
        I2P->>Dicom: Read protocol blob tag 0025 101B
        Dicom-->>I2P: Compressed protocol data
        I2P->>I2P: Decompress and build protocol file
        I2P-->>Cb: Protocol file in tmp
        Cb->>PU: upgradeProtocol
        PU-->>Cb: Upgrade result
        alt Upgrade failed
            Cb-->>User: Show ProtSavFail
        else Upgrade succeeded
            Cb->>IP: insertProtocolIntoDb
            IP->>DB: Add new single series protocol
            Cb-->>User: Show ProtSavSuc
        end
    end
```
## sceanario#2   -R2: Hardware compatibility check / Copy only when field strength and gradient slew rate match

Note : Refer the SPR for the changes relates to this requirement: HCSDM00138028 WF-enh. Protocopy should support different system configurations

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant IM as ImageManagement
    participant Shell as ProtExShell
    participant AiM as ProtExAiM
    participant Cb as ProtExCallbacks
    participant Attr as ProtExImageAttributes
    participant Cfg as SystemConfigFiles
    participant I2P as img2prot
    participant PU as protocolup
    participant IP as indexProtocol
    participant DB as SiteProtocolDB

    User->>IM: Highlight image from a system with different field strength or gradient config
    User->>IM: Click ProtoCopy in Data Apps
    IM->>Shell: Launch ProtEx with selection
    Shell->>AiM: init reads current selection
    AiM-->>Shell: Selected image path
    Shell->>Cb: initViewport displays image
    Cb->>Cb: displayImage calls updateUI with ImagePath
    Cb->>Attr: getProtExImageAttributes ImagePath
    Attr->>Attr: setFieldStrength from tag 0018 0087
    Attr->>Attr: setSoftwareRev from tag 0018 1020
    Attr->>Attr: setSystemConfig from tag 0043 1082 reads GCoilType SRMode GradientAmp RFampType
    Attr-->>Cb: ProtExImageAttributes object data
    Cb->>Attr: getConfigInfo builds label and compares config
    Attr->>Cfg: getcfg GradientConfig.cfg GCoilType and SRMode
    Attr->>Cfg: getcfg MRconfig.cfg gradientAmp RFampType fieldStrength
    Cfg-->>Attr: scanner current hardware values
    Attr->>Attr: compare image values against scanner values sets compatibleConfig flag
    Attr-->>Cb: formatted config info string
    Cb->>Attr: isValidProtocol and isCompatibleConfig
    Attr-->>Cb: compatibleConfig true or false
    alt compatibleConfig false hardware mismatch detected
        Cb->>Cb: displayStatusMsg ProtHWChange warning only
        Note over Cb,User: Warning is informational per SPR HCSDM00138028 does not block Save
    else compatibleConfig true
        Cb->>Cb: displayStatusMsg blank no warning needed
    end
    User->>Cb: Enter protocol name and category
    User->>Cb: Click Save As Protocol regardless of earlier warning
    Cb->>Cb: ProtExSaveCallback invoked
    Note over Cb: No isCompatibleConfig check exists here hardware mismatch does not block save
    Cb->>Cb: isValidProtNm and buildProtocolName
    Cb->>AiM: getSelectedImagePath and getImageType
    AiM-->>Cb: selImgPath and MxImage
    Cb->>I2P: img2prot for selected image
    I2P->>I2P: read protocol blob decompress and write protocol file
    I2P-->>Cb: return 0 success
    Cb->>PU: upgradeProtocol
    PU-->>Cb: Upgrade result true since protocolup no longer restricts hardware
    Cb->>IP: insertProtocolIntoDb
    IP->>DB: Add protocol even though source hardware differs from scanner
    Cb-->>User: Show ProtSavSuc protocol saved despite different system configuration
```

# Req2: A user can import/highlight an exam from a system with a different field strength or gradient configuration and successfully run ProtoCopy — 
ProtExSaveCallback() → img2prot() → upgradeProtocol() → insertProtocolIntoDb() all complete and the protocol lands in the site library (ProtSavSuc). 
The only trace of the original hardware-matching requirement is the non-blocking ProtHWChange status-bar message shown earlier by updateUI(), 
advising the user that some values may need manual optimization — exactly matching the SPR's intent to "support different system configurations" 
instead of refusing the copy.

ProtExImageAttributes (ProtExImageAttributes.cc) still computes a hardware-compatibility flag:
setFieldStrength()
setSystemConfig()
getConfigInfo()
isCompatibleConfig() 

ProtExSaveCallback() — the actual save/copy logic — never calls isCompatibleConfig() or getConfigInfo() at all.

Per the SPR description, protocolup (the external shell command invoked by upgradeProtocol()) also no longer restricts on hardware differences — 
this was removed on the backend side outside this source tree, consistent with "Removed restriction on hardware changes in ProtoCopy 
since protocolUp back-end no longer has any."

### Note: isCompatibleConfig() is not required to save a protocol. It only reports whether the source image's hardware configuration matches the local system.

In updateUI(), ProtoCopy uses that result to decide whether to display a status message. It does not stop the save.

When the user clicks Save As Protocol, ProtExSaveCallback() follows a separate path:

Validates the protocol name and checks for duplicates. Extracts protocol data using img2prot(). Calls upgradeProtocol(), which runs protocolup. 
Calls insertProtocolIntoDb() and displays the success message.

## sceanario#3  - Create a multi-series protocol from a selected exam

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant IM as ImageManagement
    participant Shell as ProtExShell
    participant AiM as ProtExAiM
    participant Cb as ProtExCallbacks
    participant I2P as img2prot
    participant Dicom as DicomImage
    participant PU as protocolup
    participant IP as indexProtocol
    participant DB as SiteProtocolDB

    User->>IM: Highlight exam in Browser
    User->>IM: Click ProtoCopy in Data Apps
    IM->>Shell: Launch ProtEx with selection
    Shell->>AiM: init reads current selection
    AiM-->>Shell: Selected image path and exam context
    User->>Shell: Enter protocol name and category
    User->>Shell: Enable Save Exam toggle
    User->>Shell: Click Save As Protocol
    Shell->>Cb: ProtExSaveCallback
    Cb->>Cb: isValidProtNm and buildProtocolName
    alt Name invalid or duplicate
        Cb-->>User: Show error and stop
    else Name accepted
        Cb->>AiM: getSelectedImageNumber and getSelectedSeriesNumber
        AiM-->>Cb: currentImageNo and currentSeriesNo saved for restore
        Cb->>AiM: getSeriesCount
        AiM-->>Cb: serCount total series in exam
        loop For each series i from 0 to serCount
            Cb->>AiM: gotoNextSeries first or next
            AiM-->>Cb: selSerPath for series i
            Cb->>Cb: getProtExImageAttributes reads series data
            Cb->>Cb: check isValidProtocol isPostProc MRAC duplicate serno
            alt Series eligible
                Cb->>Cb: isDynaPlanSepSer or isMolliSer when serno high
                Cb->>Cb: push series into aTable list
            else Series skipped
                Cb->>Cb: discard this series
            end
        end
        alt aTable empty no eligible series
            Cb-->>User: Show ProtNoSav and stop
        else aTable has eligible series
            Cb->>Cb: sort aTable by series number
            loop For each entry in sorted aTable
                Cb->>I2P: img2prot for this series with running serNo
                I2P->>Dicom: Read protocol blob tag 0025 101B
                Dicom-->>I2P: Compressed protocol data
                I2P->>I2P: Decompress and append series block to protocol file
                I2P-->>Cb: return status for this series
                alt img2prot failed for this series
                    Cb-->>User: Show ProtNoSav and stop
                end
            end
            Cb->>PU: upgradeProtocol
            PU-->>Cb: Upgrade result
            alt Upgrade failed
                Cb-->>User: Show ProtNoSav
            else Upgrade succeeded
                Cb->>IP: insertProtocolIntoDb
                IP->>DB: Add new multi series protocol
                Cb-->>User: Show ProtSavSuc
            end
            Cb->>AiM: gotoNextSeries and gotoImageNumber restore original selection
            AiM-->>Cb: selection restored to currentSeriesNo and currentImageNo
        end
    end
```
# Key difference R1 vs R2: 
R1 calls img2prot() once with serCnt=1, serNo=1 for the single highlighted image. 
R3 calls img2prot() once per eligible series in the exam, with serCnt=aTable.size() and serNo incrementing, 
so the same output file accumulates one block per series — producing a single multi-series protocol.

# WorkFlow
Single-series protocol
Select an exam, series, or image in Image Management and launch ProtoCopy. Enter a protocol name and anatomy, then select Save As Protocol. 
ProtoCopy creates a single-task protocol from the image displayed in its viewer.

Hardware compatibility
ProtoCopy checks the source image's hardware configuration against the local system and may display an advisory for a mismatch. 
The mismatch does not itself block saving; other validation and processing checks still apply.

Exam protocol
Select an exam, series, or image and launch ProtoCopy. Enter a name and anatomy, select Save Exam, then Save As Protocol. 
ProtoCopy creates a multi-task protocol from the exam's eligible series.

For both save modes, ProtoCopy runs protocolup and invokes indexing into the site protocol library. 
The success message alone does not confirm indexing succeeded because indexing failures are only logged.

# Appendix : Assumptions and open Questions 

 <img width="290" height="430" alt="image" src="https://github.com/user-attachments/assets/823fc979-d118-4a66-9f93-ae94c8ae293b" />

 # other details for R2 (different system):
 <img width="640" height="311" alt="image" src="https://github.com/user-attachments/assets/730ca919-f85f-41be-8a6c-ed922b5fb803" />
 <img width="640" height="293" alt="image" src="https://github.com/user-attachments/assets/b44997fe-8638-411d-9f4b-0de97010754b" />
 <img width="640" height="383" alt="image" src="https://github.com/user-attachments/assets/5dd61465-38ee-4775-a718-fb889c5940b4" />
 <img width="640" height="392" alt="image" src="https://github.com/user-attachments/assets/af4196dd-5c28-4ee5-9590-41e2362e1337" />

 <img width="640" height="375" alt="image" src="https://github.com/user-attachments/assets/3c989352-67a0-48d7-a0fd-234e3d5a2d83" />

 # Online Call chain:
 Data Apps → main() → ProtExAiM::init() → initViewport() → updateUI()
  → getProtExImageAttributes() → getConfigInfo()          [R2 warning]
User clicks Save → ProtExSaveCallback()
  → isValidProtNm() → buildProtocolName() → protexists
  → Save Exam ON:  gotoNextSeries() loop → img2prot() per series      [R3]
  → Save Exam OFF: getSelectedImagePath() → img2prot() once           [R1]
  → upgradeProtocol() → protocolup
  → insertProtocolIntoDb() → indexProtocol → Site Protocol library

  # ******* My Questions ***********
In ImageManagement we select scanned image/Dicom Image from protocopy to create new protocol.
scenario:

By selecting Exam from ImageManagement-> save exam (on/enable/checking)->save as new protocol
By selecting Exam from ImageManagement-> Save Exam (off)-> save as new protocol
By selecting series from ImageManagement for existing Exam -> create new protocol
By selecting image from ImageManagement for existing Exam-> create new protocol
which are correct scenarios among 4?

Ans: All four can be valid, provided ProtoCopy resolves the Browser selection to an available image containing protocol data. 
The Save Exam checkbox, not just the Browser selection level, determines what gets saved.

<img width="483" height="235" alt="image" src="https://github.com/user-attachments/assets/04b33fef-1c7e-4339-9638-15bf70f370c5" />

# Key external interfaces involved
DICOM image payload: protocol blob in tag (0025,101B)

Product configuration checks: getcfg, system properties

Duplicate validation: /usr/g/bin/protexists

Protocol template: PROTOCOL_TEMPLATE_FILE or /usr/g/bin/protocol.tmpl

Compression helpers: gzip / gunzip

Protocol staging and upgrade: protocolup

Indexing into site library: /export/home/wfm/host/scripts/indexProtocol

Filesystem temp staging: tmp, /tmp/.fm, /tmp/protcpy


 


 














