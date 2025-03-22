# Intervention Module Requirements

This document details the requirements for the intervention workflows in the Workforce Automation App, including step-by-step processes for intervention creation, completion, and document generation.

## Intervention Management

### INT001: Create Intervention (MVP)

**Description**: Process for creating a new intervention.

**Requirements**:
1. Users must be able to create a new intervention from:
   - The "+" button on the homepage
   - The "dossiers à traiter" (jobs to process) list
2. If the intervention was pre-loaded by administration, form fields should be pre-populated.
3. If created by the user, the form should open empty.
4. Upon opening the form, the intervention status should automatically change from "A_traiter" (to process) to "En_cours" (in progress).
5. The interface must be optimized for mobile use.

### INT002: Intervention Data Entry (MVP)

**Description**: Overall structure for intervention data entry.

**Requirements**:
1. The intervention process must be sequenced into 3 distinct steps:
   - Step 1: Beneficiary information (coordinates, housing, address)
   - Step 2: Intervention information (products, pricing, quotes)
   - Step 3: Photo documentation (before/after, application photos, meters)
2. A progress indicator (metro line style) must show advancement through the steps.
3. Each page must include navigation controls (next/previous) to move between steps.
4. The interface must be optimized for mobile use with appropriate input controls.

### INT003: Form Page 1 - Customer Information (MVP)

**Description**: Data entry for beneficiary and housing information.

**Requirements**:
1. The beneficiary section must include:
   - Input fields for contact information with appropriate format validation for email and phone
2. The housing section must include:
   - For MVP: Free text entry fields for address (address, postal code, city)
   - Address validation to ensure completeness
3. After address entry, the system must call the Symphonics API (https://api.symphonics.fr/validation/docs#/default/postPdlAddressLookup) to identify the PDL based on address and name.
4. Before proceeding to the next page, the PDL must be validated using the Symphonics API (https://api.symphonics.fr/validation/docs#/default/getPdl).
5. If PDL validation fails, an error message must be displayed but navigation between steps should remain possible.

### INT004: Address Geolocation (Post-MVP)

**Description**: Enhanced address entry using geolocation.

**Requirements**:
1. The system must provide a widget to populate the address based on the phone's geolocation.
2. If geolocation is not available, address suggestions must be provided by a third-party service as the user types.
3. The interface must clearly indicate when geolocation is being used.
4. Users must be able to override or adjust geolocation-provided addresses.

### INT005: Form Page 2 - Operations (MVP)

**Description**: Selection of operations and products for the intervention.

**Requirements**:
1. For MVP, the operations section must:
   - Provide a single value for operations and products ("Opération Ballon")
2. The interface must be optimized for mobile use with appropriate input controls.

### INT006: Contextual Operations Filtering (Post-MVP)

**Description**: Enhanced operations selection with filtering.

**Requirements**:
1. The list of operations must be restricted to those authorized for the installer's company (`operation_authorized`).
2. The list of obligated parties must be restricted to those defined for the selected operation.
3. The filtering must be dynamic and respond to user selections.
4. The interface must clearly indicate when filtering is applied.

### INT007: Price Simulation (Post-MVP)

**Description**: In-UI price simulation for quotes.

**Requirements**:
1. The system must calculate pricing based on intervention characteristics (unit price, number of rooms, etc.).
2. The calculation must follow the rules specified in the "Devis" (Quote) section of the specification.
3. The simulation must update dynamically as users change inputs.
4. The interface must clearly display the calculated price and component breakdown.

### INT008: Quote Sending (Post-MVP)

**Description**: Generation and sending of quotes for electronic signature.

**Requirements**:
1. Installers must be able to review the displayed proposal and send it for electronic signature by clicking "Envoyer Devis".
2. Each operation must be linked to a document template that may include multiple sections and merge fields.
3. The system must update the intervention status to:
   - "quote_sent" until the quote is signed
   - "quote_signed" when the quote is signed
4. The interface must clearly indicate that quote sending is optional for certain interventions (e.g., water heater regulator installation, maintenance).

### INT009: Form Page 3 - Photos (MVP)

**Description**: Photo documentation of the intervention.

**Requirements**:
1. The "Photos before" section must:
   - Validate that the installer takes as many photos as equipment declared in the Operations section
2. The "Photos after" section must:
   - Validate that the installer takes as many photos as equipment declared in the Operations section
3. The "Application Photos" section must:
   - Allow the installer to add one or more photos identifying the login of the equipment control application
4. The interface must be optimized for mobile use with appropriate camera controls.

### INT010: Conditional Photo Blocks (Post-MVP)

**Description**: Dynamic photo requirements based on operation type.

**Requirements**:
1. The "Photos tax documents" section must appear conditionally to justify the beneficiary's precarious qualification through income documentation.
2. The "Meter" section must:
   - If PDL is provided, require a photo of the electricity bill or meter showing the PDL number
   - If PCE is provided, require a photo of the gas bill or meter showing the meter number
3. The appearance of each photo block must be conditioned by information in the Operation's `Related_photo` field.
4. The interface must clearly indicate which photo blocks are required based on the selected operations.

### INT011: Finalize Intervention (MVP)

**Description**: Process for completing an intervention.

**Requirements**:
1. When clicking "Terminer l'intervention" at the bottom of page 3, the system must validate:
   - Presence and correct number of required photos
   - Presence of PDL
   - Syntax of PDL and PCE
   - Existence of a user account with the email provided via the Symphonics API (https://api.symphonics.fr/validation/docs#/default/getUser)
2. If validation fails, an error message must appear directing the installer to correct the issue.
3. Upon successful validation:
   - For MVP: Generate a single document template with invoice, customer attestation, and Symphonics contract
   - Send the document for electronic signature
   - Update intervention status to "invoice_sent"
4. When the document is signed by the beneficiary, update the intervention status to "installation done".

### INT012: Document Generation (Post-MVP)

**Description**: Enhanced document generation based on operation type.

**Requirements**:
1. The list of generated documents must be determined by the `related_document` field for the specified operations.
2. The system must support multiple document types and templates.
3. Document generation must incorporate all relevant data from the intervention.
4. The interface must indicate document generation progress and success/failure.

### INT013: Photo Geotagging (MVP)

**Description**: Geolocation tagging of photos.

**Requirements**:
1. All photos taken through the application must be:
   - Geolocated with latitude and longitude at maximum precision allowed by the phone
   - Timestamped
2. If geolocation or timestamping fails, an error message must appear and block photo capture.
3. Metadata must be securely stored with the photo.

### INT014: OCR Processing (Post-MVP)

**Description**: Optical character recognition for photo content.

**Requirements**:
1. The system must validate photo readability through OCR for meter and application photos.
2. OCR must detect PDL and PCE numbers from meter photos and auto-populate the corresponding fields.
3. OCR must validate photo authenticity to prevent AI-generated images.
4. The interface must indicate OCR processing status and results.

### INT015: Electronic Signature (Post-MVP)

**Description**: Electronic signature process for documents.

**Requirements**:
1. The system must implement email-based electronic signature:
   - The client receives an email with the document and signing invitation
   - The client reviews the document, validates information, and signs in the opened page
2. SMS confirmation is not required for signature.
3. The signature event must be captured to update the intervention status.
4. The interface must provide clear instructions for the signature process.

## General Requirements

1. All interfaces must be optimized for mobile use.
2. The system must provide appropriate error handling and user feedback.
3. Data validation must occur at appropriate points to ensure data integrity.
4. The system must maintain audit logs of all intervention activities.
5. Performance must be optimized for operation on 4G networks.
