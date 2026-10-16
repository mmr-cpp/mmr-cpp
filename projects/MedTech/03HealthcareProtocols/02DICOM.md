# DICOM PS3.1 (2026d) – Developer Reference Notes

## What is DICOM?
DICOM (Digital Imaging and Communications in Medicine) is the international standard for exchanging, storing, managing, and transmitting medical imaging information and related healthcare data.

## Main Goals
- Interoperability between multi-vendor imaging systems
- Standardized network communication
- Standardized file formats for media exchange
- Consistent image and metadata representation
- Conformance-based integration

## Scope
DICOM defines:
- Network communication protocols
- Command syntax and semantics
- Media storage services
- File format and directory structures
- Conformance requirements

DICOM does NOT define:
- Internal implementation details
- Complete system functionality
- Formal certification/testing processes

## Communication Model
Typical flow:
1. Modality (CT/MR/US/X-Ray) acquires image
2. DICOM object is created
3. Metadata attached using DICOM Data Elements
4. Object transmitted via DICOM network services
5. PACS/VNA stores data
6. Workstations retrieve and display studies

## Core Concepts
### Information Objects (IODs)
Represent real-world entities such as:
- Patient
- Study
- Series
- Image
- Structured Report

### Unique Identifiers (UIDs)
Every important object receives a globally unique identifier.

Examples:
- Study Instance UID
- Series Instance UID
- SOP Instance UID

### Service Classes
Operations performed on DICOM objects:
- Storage (C-STORE)
- Query/Retrieve (C-FIND, C-MOVE, C-GET)
- Verification (C-ECHO)
- Worklist Management
- Print Management

## DICOM Network Stack
Application Layer: DICOM Services
Presentation Layer: Transfer Syntax Negotiation
Session Layer: Association Management
Transport Layer: TCP/IP

## Association Lifecycle
1. Association Request (A-ASSOCIATE-RQ)
2. Presentation Context Negotiation
3. Service Transactions
4. Release Association

## Transfer Syntax
Defines how data is encoded.
Examples:
- Implicit VR Little Endian
- Explicit VR Little Endian
- JPEG
- JPEG 2000
- RLE

## DICOM File Structure
- 128-byte preamble
- DICM marker
- File Meta Information
- Dataset

## Patient/Study Hierarchy
Patient
 └── Study
      └── Series
           └── SOP Instance (Image/Object)

## Conformance Statements
Vendors must publish a Conformance Statement describing:
- Supported SOP Classes
- Transfer Syntaxes
- Roles (SCU/SCP)
- Limitations and options

## Important Standard Parts
- PS3.1 Introduction & Overview
- PS3.2 Conformance
- PS3.3 Information Object Definitions
- PS3.4 Service Class Specifications
- PS3.5 Data Structure & Semantics
- PS3.6 Data Dictionary
- PS3.7 Message Exchange
- PS3.8 Network Communication
- PS3.10 Media Storage & File Format
- PS3.15 Security Profiles
- PS3.18 Web Services (DICOMweb)
- PS3.22 Real-Time Communication

## Development Focus Areas
### Image Storage
Understand:
- SOP Classes
- Transfer Syntaxes
- C-STORE

### Query & Retrieval
Understand:
- C-FIND
- C-MOVE
- C-GET

### Metadata Handling
Understand:
- Tags
- VR (Value Representation)
- VM (Value Multiplicity)

### DICOMweb
REST-based APIs:
- WADO-RS
- QIDO-RS
- STOW-RS

### Security
- TLS
- Authentication
- Access control
- Audit support

## Recommended Reading Order
1. PS3.1 Overview
2. PS3.5 Data Structure & Semantics
3. PS3.6 Data Dictionary
4. PS3.7 Message Exchange
5. PS3.8 Network Communication
6. PS3.4 Service Classes
7. PS3.18 DICOMweb

## Practical Development Checklist
- Parse DICOM datasets
- Handle UID generation
- Support Association negotiation
- Implement Storage SCU/SCP
- Implement Query/Retrieve services
- Validate Transfer Syntax support
- Generate Conformance Statement
- Secure communications using TLS

## Official Reference
DICOM PS3.1 2026d Introduction and Overview
http://dicom.nema.org/ -> downloads
