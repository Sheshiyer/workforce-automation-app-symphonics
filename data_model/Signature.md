# Signature Entity

This document defines the schema for electronic signatures in the Workforce Automation App, including validation and storage requirements.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the signature",
    "required": true,
    "example": "sig_12345678"
  },
  "document_id": {
    "type": "string",
    "description": "Reference to the document this signature belongs to",
    "required": true,
    "example": "doc_87654321"
  },
  "intervention_id": {
    "type": "string",
    "description": "Reference to the intervention this signature is associated with",
    "required": true,
    "example": "int_87654321"
  },
  "signatory_type": {
    "type": "string",
    "description": "Type of signatory",
    "required": true,
    "enum": ["beneficiary", "installer", "company_representative", "third_party"],
    "example": "beneficiary"
  },
  "signatory_name": {
    "type": "string",
    "description": "Full name of the signatory",
    "required": true,
    "example": "Thomas Martin"
  },
  "signatory_email": {
    "type": "string",
    "description": "Email address of the signatory",
    "required": true,
    "format": "email",
    "example": "thomas.martin@example.com"
  },
  "signatory_phone": {
    "type": "string",
    "description": "Phone number of the signatory",
    "required": false,
    "format": "phone",
    "example": "+33612345678"
  },
  "signature_time": {
    "type": "date",
    "description": "Timestamp when the signature was applied",
    "required": true,
    "example": "2025-03-25T11:30:00Z"
  },
  "ip_address": {
    "type": "string",
    "description": "IP address from which the signature was applied",
    "required": true,
    "example": "192.168.1.1"
  },
  "user_agent": {
    "type": "string",
    "description": "User agent of the device used for signing",
    "required": false,
    "example": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.0 Mobile/15E148 Safari/604.1"
  },
  "geolocation": {
    "type": "object",
    "description": "Geolocation data at the time of signing (if available)",
    "required": false,
    "properties": {
      "latitude": {
        "type": "number",
        "example": 48.856614
      },
      "longitude": {
        "type": "number",
        "example": 2.352222
      },
      "accuracy": {
        "type": "number",
        "description": "Accuracy of the coordinates in meters",
        "example": 10
      }
    }
  },
  "validation_method": {
    "type": "string",
    "description": "Method used to validate the signatory's identity",
    "required": true,
    "enum": ["email", "sms", "none"],
    "example": "email"
  },
  "validation_details": {
    "type": "object",
    "description": "Details about the validation process",
    "required": false,
    "properties": {
      "code_sent_time": {
        "type": "date",
        "example": "2025-03-25T11:25:00Z"
      },
      "code_verified_time": {
        "type": "date",
        "example": "2025-03-25T11:28:00Z"
      },
      "attempts": {
        "type": "number",
        "example": 1
      }
    }
  },
  "signature_image": {
    "type": "string",
    "description": "Base64-encoded image of the signature (if captured)",
    "required": false,
    "example": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
  },
  "certificate_id": {
    "type": "string",
    "description": "Reference to the digital certificate used for signing (if applicable)",
    "required": false,
    "example": "cert_12345678"
  },
  "status": {
    "type": "string",
    "description": "Status of the signature",
    "required": true,
    "enum": ["valid", "invalid", "revoked"],
    "example": "valid"
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the signature record was created",
    "required": true,
    "example": "2025-03-25T11:30:00Z"
  },
  "modification_date": {
    "type": "date",
    "description": "Date when the signature record was last modified",
    "required": true,
    "example": "2025-03-25T11:30:00Z"
  },
  "audit_trail": {
    "type": "array",
    "description": "Audit trail of the signature process",
    "required": false,
    "items": {
      "type": "object",
      "properties": {
        "event": {
          "type": "string",
          "example": "signature_request_sent"
        },
        "timestamp": {
          "type": "date",
          "example": "2025-03-25T10:35:00Z"
        },
        "details": {
          "type": "object",
          "example": {
            "delivery_method": "email",
            "recipient": "thomas.martin@example.com"
          }
        }
      }
    }
  }
}
```

## Electronic Signature Process

### 1. Signature Request

When a document requires a signature, the system:
- Creates a signature request with status "pending"
- Sends a notification to the signatory via email
- Generates a unique, secure link for the signatory to access the document

### 2. Identity Validation

Before allowing the signatory to sign the document, the system validates their identity through one of the following methods:

#### Email Validation
- System sends a one-time code to the signatory's email address
- Signatory enters the code to prove access to the email account
- System records the validation details (time sent, time verified, attempts)

#### SMS Validation (Future Enhancement)
- System sends a one-time code to the signatory's phone number
- Signatory enters the code to prove access to the phone
- System records the validation details (time sent, time verified, attempts)

#### No Validation
- For certain low-risk documents, validation may be skipped
- This option is only available for specific document types and scenarios

### 3. Signature Application

Once identity is validated, the signatory can sign the document:
- System captures timestamp, IP address, and user agent
- Geolocation data is captured if available and permitted
- Signatory may draw or type their signature
- System records all signature metadata

### 4. Signature Verification

After the signature is applied:
- System creates a cryptographic hash of the signed document
- Digital certificate may be applied (for qualified electronic signatures)
- Signature status is set to "valid"
- Document status is updated to "signed"

## Legal Compliance

The electronic signature implementation complies with the following regulations:

### eIDAS Regulation (EU)
- Ensures that electronic signatures have the same legal standing as handwritten signatures
- Supports different levels of electronic signatures (simple, advanced, qualified)
- Ensures cross-border recognition within the EU

### GDPR Compliance
- Collects only necessary personal data for signature validation
- Provides clear information about data processing
- Implements appropriate security measures
- Retains signature data according to legal requirements

## Signature Types

### Simple Electronic Signature
- Basic level of signature with email validation
- Suitable for low-risk documents
- Used for quotes and basic attestations

### Advanced Electronic Signature (Future Enhancement)
- Higher level of security with stronger identity verification
- Uniquely linked to the signatory
- Capable of identifying any subsequent changes to the document
- Used for invoices and contracts

### Qualified Electronic Signature (Future Enhancement)
- Highest level of security with qualified certificates
- Equivalent to handwritten signatures in all EU member states
- Used for high-value contracts and legal documents

## Relationships

1. **Document**: Each signature belongs to exactly one document (many-to-one relationship).
2. **Intervention**: Each signature is associated with exactly one intervention (many-to-one relationship).

## Indexing

1. **Primary Key**: `id`
2. **Foreign Keys**: `document_id`, `intervention_id`
3. **Search Indexes**: `signatory_email`, `signature_time`, `status`
4. **Compound Indexes**: `(document_id, signatory_type)`, `(intervention_id, signature_time)`

## API Endpoints

1. **GET /signatures**: List signatures (with filtering and pagination)
2. **GET /signatures/{id}**: Get signature details
3. **GET /signatures/{id}/audit-trail**: Get signature audit trail
4. **POST /signatures/validate**: Validate a signature
5. **PATCH /signatures/{id}/status**: Update signature status (for revocation)

## Implementation Notes

1. **Storage**: Signature data is stored in the primary database with appropriate encryption.
2. **Audit Trail**: All signature events are logged in an immutable audit trail.
3. **Security**: Signature validation uses industry-standard cryptographic techniques.
4. **Integration**: The signature system may be integrated with a third-party electronic signature provider.
5. **Compliance**: The implementation is regularly reviewed to ensure compliance with evolving regulations.
