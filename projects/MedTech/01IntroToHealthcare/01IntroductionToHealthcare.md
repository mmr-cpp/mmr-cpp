# Healthcare Domain 

## Hospital Workflow.

👤 **Patient Arrives**
⬇️

🏥 **HIS (Hospital Information System)**
➡️ Patient Registration
➡️ Order Entry / Exam Request
⬇️

📋 **RIS (Radiology Information System)**
➡️ Order Scheduling
➡️ Worklist Preparation
⬇️

🖥️ **Modality Worklist**
➡️ Scheduled Exam Available to Modality
⬇️

📡 **Modality**
➡️ Image Acquisition (X-Ray / CT / MRI / Ultrasound)
⬇️

🔍 **Review Workstation**
➡️ Image Review
➡️ Quality Check
➡️ Clinical Assessment
⬇️

✅ **Exam Completed**
➡️ Results Available for Clinical Use

**All are seperate Systems Microservices/Monolith**

# HIS - Start with this ...
- The end users of a Hospital Information System (HIS) are diverse, categorized primarily into clinical staff, administrative personnel, management, and patients, who interact with the platform for daily operations, record keeping, and care management.
- Designed to manage Hospital's operations such as:
    - Medical
    - Administrative
    - Financial
    - Legal issues.
- Handles Activities Related to Admit, Discharge and Transfer (ADT) of a patient.

# RIS
- The primary end users of a Radiology Information System (RIS) are healthcare professionals who manage and execute medical imaging workflows. This group includes radiologists, radiology technologists, referring physicians, hospital administrators, and billing or front-desk staff.
- Networked software system for managing medical Imagery and associated data.
- Basic Functions:
    - Patient Management and Tracking.
    - Scheduling
    - Result Reporting
    - Billing.
      
# Acquisition Modalities 
- CT, MRI, Ultrasound, X-Ray, etc.
- Includes:
    - Calibration
    - Image post processing 
    - Image Visualization (GRX)
    - Clinical Applications
    - Archiving images in Media.
    - Connectivity to RIS, PACS and DICOM Printers.
 
## Review Workstations 
- Imaging Workstation is used by the radiologist to diagnose the scanned images.
- Usually, workstations support multiple modalities.
- Includes:
    - Clinical Applications.
    - Advanced Image Visualisation.
    - Persistence of Images in Media.
    - DICOM Connectivity to PACS and DICOM Printer.
 
# PACS
- Capable of storing, retrieving, transmitting and displaying digital images and relevant patient data from various imaging sources.
- Storage and transfer based on DICOM Standard.

# EMR
- 📄 **EMR (Electronic Medical Record)**
- 💻 Digital version of the traditional paper-based medical record for an individual. Provided from healthcare's side like GE/Siemens.
- 👤 Stores patient information, including:
  - Patient Demographics
  - Diagnoses
  - Clinical Notes
  - Medications
  - Laboratory Results
  - Immunization Records
  - Allergy Information
- ✅ Reduces and helps prevent medical errors.
- ⚡ Improves efficiency in healthcare operations.
- 🤝 Enhances coordination of patient care.
- 🏥 Represents a patient's medical record within a single healthcare facility, such as:
  - Doctor's Office
  - Clinic
  - Hospital
    
# Clinical Applications
- 🔬 Enhances the diagnosis and interpretation of acquired medical data.
- Integrated within modality applications, or separate microservices.
- 💻 Advanced software post-processing techniques enable healthcare professionals to examine medical data in more effective and non-conventional ways.
- 🖼️ Supports advanced visualization and analysis capabilities, including:
  - 📊 3D image volume reconstruction
  - 🌐 4D image visualization and analysis
  - 🔍 Advanced image viewing and manipulation
- ⚡ Improves clinical decision-making by providing deeper insights into diagnostic data.
- 🩺 Assists clinicians in identifying abnormalities that may not be easily visible in standard image views.

## 🏥 Standards – DICOM
- 📷 **DICOM (Digital Imaging and Communications in Medicine)**
- 🌐 Industry standard that enables medical imaging equipment and healthcare systems to communicate and exchange imaging data.
- 🤝 Developed to ensure interoperability between imaging devices from different vendors.
- 📁 Defines both:
  - Image file format for medical images
  - Network communication protocol for transmitting medical data
- 🩺 Widely used for storing, sharing, viewing, and managing medical images across healthcare environments.

### 🏛️ Organizations Behind DICOM
- 🏥 **ACR (American College of Radiology)**
- ⚡ **NEMA (National Electrical Manufacturers Association)**

### 🛠️ Common DICOM Frameworks & Libraries
- 💻 **DCMTK** (C++)
- 💻 **MergeCOM** (C++)
- 💻 **ClearCanvas** (C#)
- 💻 **DCM4CHE** (Java)

## 🏥 Standards – HL7
- 🌐 **HL7 (Health Level Seven)** is an ANSI-accredited Standards Developing Organization (SDO) operating in the healthcare industry.
- 🔄 Provides standards for healthcare data exchange, enabling interoperability between healthcare information systems.
- 📡 Focuses on the **Application Layer (Layer 7)** of the OSI networking model.
- 📋 Supports the exchange of both:
  - Clinical data
  - Administrative data
- 🤝 Enables communication between systems such as:
  - EMR/EHR
  - HIS
  - RIS
  - Laboratory Information Systems (LIS)
  - Billing and Insurance Systems
- 🏛️ HL7 is a non-profit organization composed of:
  - Healthcare providers
  - Payers
  - Vendors
  - Government organizations
  - Volunteers
- 🔑 Primary goal:
  - Syntactic Interoperability (data format consistency)
  - Semantic Interoperability (shared meaning of data)

## 🛠️ Common HL7 Frameworks & Libraries
- ☕ **HAPI** (Java)
- 🔷 **NHAPI** (.NET)
- ☕ **dcm4che** (Java)

## 🔄 Typical HL7 Workflow
👤 Patient Registration (HIS / EMR)  
⬇️
📋 Order Creation  
⬇️
📨 HL7 Message Generated (ADT / ORM / ORU)  
⬇️
🔄 Message Exchange Between Systems  
⬇️
🏥 RIS / LIS / EMR Receives Data  
⬇️
✅ Patient Information Updated Across Healthcare Systems

### 📨 Common HL7 Message Types
- 👤 **ADT** – Admission, Discharge, Transfer
- 📝 **ORM** – Order Message
- 📊 **ORU** – Observation Result
- 💰 **DFT** – Detailed Financial Transaction
- 📅 **SIU** – Scheduling Information

## 🏥 Standards – HL7 FHIR
- 🚀 **FHIR (Fast Healthcare Interoperability Resources)** is a healthcare interoperability standard developed by **HL7**.
- 🔄 Designed to enable the seamless exchange of healthcare information between different healthcare systems and applications.
- 🌐 Combines the best features of HL7 standards with modern web technologies such as:
  - RESTful APIs
  - JSON
  - XML
  - HTTP

### ✨ Key Highlights
- 📏 **Standardization**
  - Provides a consistent approach for representing and exchanging healthcare data.
- 🔧 **Flexibility**
  - Supports a wide range of healthcare workflows and implementation scenarios.
- ⚡ **Efficiency**
  - Simplifies data exchange using lightweight web-based technologies.
- 📊 **Data Focus**
  - Organizes healthcare information into reusable resources such as Patient, Observation, Encounter, and Medication.
- 🤝 **Interoperability**
  - Enables different healthcare systems to communicate and share information effectively.

### 🧩 Common FHIR Resources
- 👤 Patient
- 🏥 Organization
- 👨‍⚕️ Practitioner
- 📅 Appointment
- 🩺 Encounter
- 💊 Medication
- 🧪 Observation
- 📋 Diagnostic Report

### 🛠️ FHIR Implementation Considerations
- 🌐 FHIR Servers
- 💻 FHIR Clients
- 📐 Data Modeling
- 🔒 Security
- 🔐 Privacy
- ✅ Authentication & Authorization

### 🔄 Typical FHIR Workflow
👤 Patient Registered  
⬇️
🏥 EMR / EHR Creates Patient Resource  
⬇️
🌐 FHIR API Request  
⬇️
📤 Data Exchange Between Systems  
⬇️
📥 Receiving System Processes Resource  
⬇️
✅ Healthcare Information Updated Across Applications
