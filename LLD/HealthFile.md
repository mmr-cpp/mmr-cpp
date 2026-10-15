```mermaid
sequenceDiagram
    autonumber

    actor User
    participant Main as ProtExMain.cc
    participant UI as ProtExShell
    participant Sel as ProtExAiM
    participant CB as ProtExCallbacks.cc
    participant Attr as ProtExImageAttributes
    participant Conv as img2prot.cc
    participant DMF as Terra DMF DICOM API
    participant SDC as SDC Selection API
    participant OS as OS Commands & Files
    participant Site as Site Protocol Library

    Note over User,Site: Prerequisite: SaveRx and Scan have stored protocol data in DICOM tag 0025,101B

    User->>Main: Launch ProtoCopy from Image Management

    Main->>OS: getProductProperty(MWL.StartExam.AllowedModalities)
    Main->>UI: create_ProtExShell()
    Main->>UI: VisualInterface_Manage()
    Main->>SDC: AinitCommunications()

    Main->>Sel: ProtExAiM initialization
    Sel->>SDC: ASdCSelectionOpen()
    Sel->>SDC: ASdCPrimaryImage()
    Sel->>Sel: buildSeriesTable()
    Sel->>Sel: buildImageTable()

    Main->>CB: initViewport()
    CB->>Sel: getSelectedImagePath()

    CB->>CB: updateUI()
    CB->>CB: displayImage()

    CB->>Attr: getProtExImageAttributes()

    Attr->>DMF: createNewItem()
    Attr->>DMF: getValue(model)
    Attr->>DMF: getValue(field strength)
    Attr->>DMF: getValue(modality)
    Attr->>DMF: getValue(image type)
    Attr->>DMF: getValue(tag 0025,101B)

    DMF-->>Attr: Metadata + Protocol Presence

    CB->>Attr: getProductInfo()
    CB->>Attr: getConfigInfo()

    Attr->>OS: getcfg MRconfig.cfg
    Attr->>OS: getcfg GradientConfig.cfg

    CB->>UI: displayStatusMsg()

    User->>UI: Enter Protocol Name
    UI->>CB: ProtocolNameCallback()

    User->>UI: Select Category
    UI->
