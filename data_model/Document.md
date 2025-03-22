# Document Entity

This document defines the schema for generated documents in the Workforce Automation App, including fields, types, statuses, and delivery methods.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the document",
    "required": true,
    "example": "doc_12345678"
  },
  "intervention_id": {
    "type": "string",
    "description": "Reference to the intervention this document belongs to",
    "required": true,
    "example": "int_87654321"
  },
  "type": {
    "type": "string",
    "description": "Type of document",
    "required": true,
    "enum": [
      "quote",
      "invoice",
      "attestation_honneur",
      "attestation_fin_travaux",
      "contrat_symphonics"
    ],
    "example": "invoice"
  },
  "name": {
    "type": "string",
    "description": "Human-readable name of the document",
    "required": true,
    "example": "Facture - Thomas Martin - 25/03/2025"
  },
  "file_path": {
    "type": "string",
    "description": "Path to the document file",
    "required": true,
    "example": "documents/interventions/int_87654321/invoice_20250325.pdf"
  },
  "template_id": {
    "type": "string",
    "description": "Reference to the template used to generate this document",
    "required": false,
    "example": "tpl_12345678"
  },
  "merge_fields": {
    "type": "object",
    "description": "Data used to populate the document template",
    "required": false,
    "example": {
      "beneficiary_name": "Thomas Martin",
      "beneficiary_address": "45 Rue des Fleurs, 75015 Paris",
      "intervention_date": "2025-03-25",
      "total_amount": "150.00 €",
      "operation_description": "Installation d'un régulateur de ballon d'eau chaude"
    }
  },
  "status": {
    "type": "string",
    "description": "Current status of the document",
    "required": true,
    "enum": [
      "draft",
      "generated",
      "sent",
      "viewed",
      "signed",
      "rejected",
      "expired",
      "archived"
    ],
    "example": "sent"
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the document was created",
    "required": true,
    "example": "2025-03-25T10:30:00Z"
  },
  "sent_date": {
    "type": "date",
    "description": "Date when the document was sent for signature",
    "required": false,
    "example": "2025-03-25T10:35:00Z"
  },
  "viewed_date": {
    "type": "date",
    "description": "Date when the document was first viewed by the recipient",
    "required": false,
    "example": "2025-03-25T11:15:00Z"
  },
  "signed_date": {
    "type": "date",
    "description": "Date when the document was signed",
    "required": false,
    "example": "2025-03-25T11:30:00Z"
  },
  "expiration_date": {
    "type": "date",
    "description": "Date when the signature request expires",
    "required": false,
    "example": "2025-04-25T10:35:00Z"
  },
  "delivery_method": {
    "type": "string",
    "description": "Method used to deliver the document",
    "required": true,
    "enum": ["email", "sms", "in_app", "api"],
    "example": "email"
  },
  "delivery_details": {
    "type": "object",
    "description": "Details about the delivery",
    "required": false,
    "properties": {
      "recipient": {
        "type": "string",
        "example": "thomas.martin@example.com"
      },
      "subject": {
        "type": "string",
        "example": "Votre facture pour l'installation du régulateur de ballon d'eau chaude"
      },
      "message": {
        "type": "string",
        "example": "Bonjour Thomas Martin, veuillez trouver ci-joint votre facture pour l'installation effectuée le 25/03/2025. Merci de la signer électroniquement."
      }
    }
  },
  "signatures": {
    "type": "array",
    "description": "List of signatures applied to this document",
    "required": false,
    "items": {
      "type": "string",
      "description": "Reference to a signature",
      "example": "sig_12345678"
    }
  },
  "file_size": {
    "type": "number",
    "description": "Size of the document file in bytes",
    "required": false,
    "example": 245678
  },
  "file_type": {
    "type": "string",
    "description": "MIME type of the document file",
    "required": true,
    "example": "application/pdf"
  },
  "hash": {
    "type": "string",
    "description": "Hash of the document file for integrity verification",
    "required": false,
    "example": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6"
  },
  "metadata": {
    "type": "object",
    "description": "Additional metadata about the document",
    "required": false,
    "example": {
      "page_count": 2,
      "version": "1.0",
      "generated_by": "usr_87654321"
    }
  }
}
```

## Document Types

### Quote
Price proposal document sent before work begins.

**Purpose**: To provide the beneficiary with a detailed cost estimate and obtain approval before proceeding with the installation.

**Required Fields**:
- Beneficiary information
- Detailed description of operations and products
- Unit and total prices
- Terms and conditions
- Validity period

### Invoice
Billing document generated after work completion.

**Purpose**: To bill the beneficiary for the completed work and provide a record for accounting and tax purposes.

**Required Fields**:
- Beneficiary information
- Company information and tax identification
- Detailed description of operations and products
- Unit and total prices
- Tax information
- Payment terms
- Legal notices

### Attestation d'Honneur (AH)
Sworn statement by the beneficiary required for CEE operations.

**Purpose**: To certify that the energy efficiency work has been completed according to regulations and that the beneficiary meets eligibility criteria.

**Required Fields**:
- Beneficiary information
- Property information
- Detailed description of operations
- Installer information
- Obligated party information
- Regulatory declarations
- Signature fields

### Attestation de Fin de Travaux (AFT)
Work completion certificate.

**Purpose**: To document the successful completion of the installation and the beneficiary's satisfaction.

**Required Fields**:
- Beneficiary information
- Intervention details
- Completion date
- Installer information
- Confirmation of proper functioning
- Signature fields

### Contrat Symphonics
Service contract for the Symphonics platform.

**Purpose**: To establish the terms of service for the Symphonics platform and obtain the beneficiary's consent.

**Required Fields**:
- Beneficiary information
- Service description
- Terms and conditions
- Privacy policy
- Subscription details
- Signature fields

## Status Definitions

### draft
Document has been created but not yet finalized.

**Transitions**:
- → generated (when document is finalized)

### generated
Document has been generated and is ready to be sent.

**Transitions**:
- → sent (when document is sent for signature)
- → archived (if document is no longer needed)

### sent
Document has been sent to the beneficiary for signature.

**Transitions**:
- → viewed (when document is opened by the beneficiary)
- → expired (if signature deadline passes)
- → archived (if document is recalled)

### viewed
Document has been viewed by the beneficiary.

**Transitions**:
- → signed (when document is signed)
- → rejected (if beneficiary refuses to sign)
- → expired (if signature deadline passes)

### signed
Document has been signed by all required parties.

**Transitions**:
- → archived (after retention period)

### rejected
Document has been rejected by the beneficiary.

**Transitions**:
- → archived (if no further action is taken)
- → draft (if document needs to be revised)

### expired
Signature request has expired without completion.

**Transitions**:
- → archived (if no further action is taken)
- → sent (if signature request is renewed)

### archived
Document has been archived for long-term storage.

**Transitions**:
- None (terminal state)

## Document Generation Process

1. **Template Selection**:
   - System selects appropriate template based on document type and operation
   - Templates include placeholders for dynamic content

2. **Data Merging**:
   - Intervention data is merged into the template
   - Calculations are performed for pricing and tax information
   - Legal notices are included based on document type and jurisdiction

3. **PDF Generation**:
   - Merged template is converted to PDF format
   - Document is stored in the file system with appropriate path
   - Metadata and hash are generated for integrity verification

4. **Delivery Preparation**:
   - Delivery method is determined based on beneficiary preferences
   - Delivery details are prepared (recipient, subject, message)
   - Document status is updated to "generated"

## Electronic Signature Process

1. **Signature Request**:
   - Document is sent to the beneficiary via the selected delivery method
   - Status is updated to "sent"
   - Expiration date is set based on configuration

2. **Beneficiary Actions**:
   - Beneficiary receives notification and opens the document (status: "viewed")
   - Beneficiary reviews the document content
   - Beneficiary either signs or rejects the document

3. **Signature Completion**:
   - Upon signature, the document status is updated to "signed"
   - Signature metadata is recorded (timestamp, IP address, validation method)
   - Signed document is stored with signature information
   - Intervention status is updated based on document type

## Relationships

1. **Intervention**: Each document belongs to exactly one intervention (many-to-one relationship).
2. **Signatures**: Each document can have multiple signatures (one-to-many relationship).

## Indexing

1. **Primary Key**: `id`
2. **Foreign Keys**: `intervention_id`, `template_id`
3. **Search Indexes**: `type`, `status`, `creation_date`
4. **Compound Indexes**: `(intervention_id, type)`, `(status, creation_date)`

## API Endpoints

1. **GET /documents**: List documents (with filtering and pagination)
2. **GET /documents/{id}**: Get document details
3. **GET /documents/{id}/file**: Download document file
4. **POST /documents**: Create new document
5. **PATCH /documents/{id}/status**: Update document status
6. **POST /documents/{id}/send**: Send document for signature
7. **GET /documents/templates**: List available document templates

## Implementation Notes

1. **Storage**: Document files are stored in a secure file system with appropriate access controls.
2. **Template Management**: Document templates are managed through an administrative interface.
3. **Signature Integration**: Electronic signature functionality is integrated with a third-party service.
4. **Retention Policy**: Documents are retained according to legal requirements (10 years for financial documents).
5. **Security**: Document access is restricted based on user roles and permissions.
