# Company Entity

This document defines the schema for company entities in the Workforce Automation App, including verification documents and permissions.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the company",
    "required": true,
    "example": "cmp_12345678"
  },
  "name": {
    "type": "string",
    "description": "Company name",
    "required": true,
    "example": "Installations Énergie Plus"
  },
  "siret": {
    "type": "string",
    "description": "SIRET number (French company identification)",
    "required": true,
    "format": "siret",
    "example": "12345678901234"
  },
  "address": {
    "type": "string",
    "description": "Company street address",
    "required": true,
    "example": "123 Avenue de la République"
  },
  "postal_code": {
    "type": "string",
    "description": "Company postal code",
    "required": true,
    "example": "75011"
  },
  "city": {
    "type": "string",
    "description": "Company city",
    "required": true,
    "example": "Paris"
  },
  "country": {
    "type": "string",
    "description": "Company country",
    "required": true,
    "default": "FR",
    "example": "FR"
  },
  "contact_name": {
    "type": "string",
    "description": "Name of the primary contact person",
    "required": true,
    "example": "Marie Dubois"
  },
  "contact_email": {
    "type": "string",
    "description": "Email of the primary contact person",
    "required": true,
    "format": "email",
    "example": "marie.dubois@energie-plus.fr"
  },
  "contact_phone": {
    "type": "string",
    "description": "Phone number of the primary contact person",
    "required": true,
    "format": "phone",
    "example": "+33123456789"
  },
  "installer_application_code": {
    "type": "string",
    "description": "Unique code for installer application registration",
    "required": true,
    "example": "INS-ENG-2025-0123"
  },
  "installer_application_code_creation_date": {
    "type": "date",
    "description": "Date when the installer application code was created",
    "required": true,
    "example": "2025-03-15T10:30:00Z"
  },
  "operation_authorized": {
    "type": "array",
    "description": "List of operation codes authorized for this company",
    "required": true,
    "items": {
      "type": "string",
      "example": "BAR-TH-173"
    },
    "example": ["BAR-TH-173", "BAR-TH-171", "BAR-EQ-115"]
  },
  "verification_documents": {
    "type": "object",
    "description": "Documents provided for company verification",
    "required": true,
    "properties": {
      "kbis": {
        "type": "object",
        "properties": {
          "file_path": {
            "type": "string",
            "example": "documents/companies/cmp_12345678/kbis_20250315.pdf"
          },
          "verification_date": {
            "type": "date",
            "example": "2025-03-15T14:30:00Z"
          },
          "verified_by": {
            "type": "string",
            "example": "usr_87654321"
          },
          "status": {
            "type": "string",
            "enum": ["pending", "verified", "rejected"],
            "example": "verified"
          }
        }
      },
      "rge": {
        "type": "object",
        "properties": {
          "file_path": {
            "type": "string",
            "example": "documents/companies/cmp_12345678/rge_20250315.pdf"
          },
          "verification_date": {
            "type": "date",
            "example": "2025-03-15T14:35:00Z"
          },
          "verified_by": {
            "type": "string",
            "example": "usr_87654321"
          },
          "status": {
            "type": "string",
            "enum": ["pending", "verified", "rejected"],
            "example": "verified"
          },
          "expiration_date": {
            "type": "date",
            "example": "2027-03-15T00:00:00Z"
          }
        }
      }
    }
  },
  "status": {
    "type": "string",
    "description": "Current status of the company account",
    "required": true,
    "enum": ["pending", "active", "suspended", "inactive"],
    "example": "active"
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the company account was created",
    "required": true,
    "example": "2025-03-15T10:30:00Z"
  },
  "modification_date": {
    "type": "date",
    "description": "Date when the company account was last modified",
    "required": true,
    "example": "2025-03-20T14:45:00Z"
  },
  "notes": {
    "type": "string",
    "description": "Administrative notes about the company",
    "required": false,
    "example": "Specialized in water heater installations. Verified documentation on 2025-03-15."
  }
}
```

## Status Definitions

### Pending
Company has been created but verification is not complete.

**Transitions**:
- → Active (when verification is complete)
- → Inactive (if verification fails or is not completed within a certain timeframe)

### Active
Company is verified and can operate in the system.

**Transitions**:
- → Suspended (temporary restriction)
- → Inactive (permanent deactivation)

### Suspended
Company operations are temporarily restricted.

**Transitions**:
- → Active (when suspension is lifted)
- → Inactive (when permanently deactivated)

### Inactive
Company account is deactivated and cannot operate in the system.

**Transitions**:
- → Active (if reactivated by back office)

## Verification Process

1. **Document Submission**:
   - KBIS (company registration document)
   - RGE certification (for energy efficiency operations)
   - Additional certifications as required for specific operations

2. **Verification Steps**:
   - Back office staff reviews submitted documents
   - Validates authenticity and expiration dates
   - Confirms company details match registration information
   - Assigns authorized operations based on certifications

3. **Code Generation**:
   - Upon successful verification, a unique installer application code is generated
   - Code is communicated to the company for user registration
   - Code creation date is recorded for tracking purposes

## Operation Authorization

1. **Authorization Process**:
   - Operations are authorized based on company certifications
   - Each operation corresponds to a CEE (Energy Efficiency Certificate) code
   - Authorization determines which operations can be selected during interventions

2. **Authorization Management**:
   - Back office can add or remove authorized operations
   - Changes to authorization are tracked with modification dates
   - Expiring certifications trigger notifications for renewal

## Relationships

1. **Users**: A company can have multiple users (one-to-many relationship).
2. **Interventions**: A company can have multiple interventions through its users (indirect relationship).

## Indexing

1. **Primary Key**: `id`
2. **Search Indexes**: `name`, `siret`, `postal_code`, `installer_application_code`
3. **Compound Indexes**: `(status, creation_date)`, `(operation_authorized, status)`

## Audit and Compliance

1. **Audit Trail**:
   - All changes to company data are logged
   - Log includes previous values, new values, timestamp, and actor

2. **Document Retention**:
   - Verification documents are retained according to legal requirements
   - Document access is restricted to authorized personnel

## API Endpoints

1. **GET /companies**: List companies (with filtering and pagination)
2. **GET /companies/{id}**: Get company details
3. **POST /companies**: Create new company
4. **PUT /companies/{id}**: Update company
5. **PATCH /companies/{id}/status**: Update company status
6. **PATCH /companies/{id}/operations**: Update authorized operations
7. **DELETE /companies/{id}**: Deactivate company (sets status to inactive)

## Implementation Notes

1. **Storage**: Company data is stored in the primary database.
2. **Document Storage**: Verification documents are stored in a secure file storage system with appropriate access controls.
3. **Code Generation**: Installer application codes follow a defined format and include validation mechanisms to prevent forgery.
