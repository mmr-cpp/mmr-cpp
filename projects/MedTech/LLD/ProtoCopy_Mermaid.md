sequenceDiagram
    autonumber

    actor User
    participant Main as ProtExMain
    participant UI as ProtExShell
    participant Sel as ProtExAiM
    participant CB as ProtExCallbacks
    participant Attr as ProtExImageAttributes
    participant Conv as img2prot
    participant DMF as TerraDMF
    participant SDC as SDC
    participant OS as OS
    participant Site as SiteProtocols

    Note over User,Site: Prerequisite - SaveRx and Scan stored protocol data in DICOM tag 0025,101B

    User->>Main: Launch ProtoCopy

    Main->>OS: Read modality configuration
    Main->>UI: Create UI
    Main->>SDC: Initialize communications

    Main->>Sel: Initialize selection manager
    Sel->>SDC: Open selection
    Sel->>SDC: Get primary image
    Sel->>Sel: Build series table
    Sel->>Sel: Build image table

    Main->>CB: Initialize viewport
    CB->>Sel: Get selected image path

    CB->>CB: Update UI
    CB->>CB: Display image

    CB->>Attr: Read image attributes

    Attr->>DMF: Create item
    Attr->>DMF: Read metadata
    Attr->>DMF: Read tag 0025,101B

    DMF-->>Attr: Return metadata and protocol data

    CB->>Attr: Read product info
    CB->>Attr: Read configuration info

    Attr->>OS: Read MR configuration
    Attr->>OS: Read gradient configuration

    CB->>UI: Display status

    User->>UI: Enter protocol name
    UI->>CB: Name callback

    User->>UI: Select category
    UI->>CB: Category callback

    User->>UI: Click Save As Protocol
    UI->>CB: Save callback

    CB->>CB: Validate protocol name
    CB->>CB: Build protocol name
    CB->>CB: Determine anatomy

    CB->>OS: Check if protocol exists
    OS-->>CB: Exists status

    alt Invalid name or duplicate

        CB->>UI: Display error

    else Name accepted

        alt Save Exam

            loop For each series

                CB->>Sel: Next series
                Sel-->>CB: Series path

                CB->>Attr: Read series attributes

                CB->>CB: Validate protocol
                CB->>CB: Check post processing
                CB->>CB: Check image set
                CB->>CB: PET validation checks
                CB->>CB: Duplicate series checks
                CB->>CB: Add eligible series

            end

            CB->>CB: Sort by series number

        else Save Selected Image

            CB->>Sel: Get selected image path
            CB->>Sel: Get image type

            CB->>Attr: Check PET series
            CB->>Attr: Check modality allowed

        end

        loop Convert each selected item

            CB->>Conv: Convert image to protocol

            Conv->>OS: Read protocol template
            Conv->>DMF: Read tag 0025,101B

            DMF-->>Conv: Return protocol blob

            Conv->>OS: Create temporary file
            Conv->>OS: Decompress protocol blob

            opt FMRI tag present

                Conv->>OS: Create FMRI directory
                Conv->>OS: Create FMRI blob file

            end

            alt PET image

                Conv->>Conv: Create PET protocol

            else Mx image

                Conv->>Conv: Create Mx protocol

            else Lx image

                Conv->>Conv: Create Lx protocol

            end

            Conv->>Conv: Write protocol header
            Conv->>Conv: Write series set section

            Conv-->>CB: Conversion status

        end

        alt Conversion failed or nothing eligible

            CB->>UI: Display save error

        else Protocol file created

            CB->>CB: Upgrade protocol

            CB->>OS: Run protocolup
            OS-->>CB: Upgrade status

            alt Upgrade failed

                CB->>UI: Display failure

            else Upgrade succeeded

                CB->>CB: Insert protocol into database

                CB->>OS: Run indexProtocol
                OS->>Site: Add protocol to site library

                CB->>OS: Remove temporary files

                CB->>UI: Display success

            end

        end

    end

    User->>Site: Verify protocol in category

    User->>UI: Quit

    UI->>CB: Close callback

    CB->>OS: Remove temporary FMRI files
    CB->>OS: Exit
