# Intervention Entity

This document defines the schema for interventions in the Workforce Automation App, including statuses, timestamps, linked operations, and documents.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the intervention",
    "required": true,
    "example": "int_12345678"
  },
  "user_id": {
    "type": "string",
    "description": "Reference to the user managing the intervention",
    "required": true,
    "example": "usr_87654321"
  },
  "company_id": {
    "type": "string",
    "description": "Reference to the company performing the intervention",
    "required": true,
    "example": "cmp_87654321"
  },
  "beneficiary_first_name": {
    "type": "string",
    "description": "First name of the beneficiary (customer)",
    "required": true,
    "example": "Thomas"
  },
  "beneficiary_last_name": {
    "type": "string",
    "description": "Last name of the beneficiary (customer)",
    "required": true,
    "example": "Martin"
  },
  "beneficiary_email": {
    "type": "string",
    "description": "Email address of the beneficiary",
    "required": true,
    "format": "email",
    "example": "thomas.martin@example.com"
  },
  "beneficiary_phone": {
    "type": "string",
    "description": "Phone number of the beneficiary",
    "required": true,
    "format": "phone",
    "example": "+33612345678"
  },
  "address": {
    "type": "string",
    "description": "Street address of the intervention location",
    "required": true,
    "example": "45 Rue des Fleurs"
  },
  "postal_code": {
    "type": "string",
    "description": "Postal code of the intervention location",
    "required": true,
    "example": "75015"
  },
  "city": {
    "type": "string",
    "description": "City of the intervention location",
    "required": true,
    "example": "Paris"
  },
  "country": {
    "type": "string",
    "description": "Country of the intervention location",
    "required": true,
    "default": "FR",
    "example": "FR"
  },
  "coordinates": {
    "type": "object",
    "description": "Geolocation coordinates of the intervention location",
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
  "pdl": {
    "type": "string",
    "description": "PDL (Point de Livraison) number for electricity",
    "required": false,
    "format": "pdl",
    "example": "12345678901234"
  },
  "pce": {
    "type": "string",
    "description": "PCE (Point de Comptage et d'Estimation) number for gas",
    "required": false,
    "format": "pce",
    "example": "12345678901234"
  },
  "housing_type": {
    "type": "string",
    "description": "Type of housing",
    "required": true,
    "enum": ["apartment", "house", "building", "other"],
    "example": "apartment"
  },
  "construction_year": {
    "type": "number",
    "description": "Year of construction of the building",
    "required": false,
    "example": 1985
  },
  "heating_type": {
    "type": "string",
    "description": "Type of heating system",
    "required": false,
    "enum": ["electric", "gas", "oil", "wood", "other"],
    "example": "electric"
  },
  "status": {
    "type": "string",
    "description": "Current status of the intervention",
    "required": true,
    "enum": [
      "A_traiter",
      "En_cours",
      "quote_sent",
      "quote_signed",
      "invoice_sent",
      "installation_done",
      "published",
      "canceled",
      "maintenance_required"
    ],
    "example": "En_cours"
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the intervention was created",
    "required": true,
    "example": "2025-03-15T10:30:00Z"
  },
  "modification_date": {
    "type": "date",
    "description": "Date when the intervention was last modified",
    "required": true,
    "example": "2025-03-20T14:45:00Z"
  },
  "scheduled_date": {
    "type": "date",
    "description": "Date when the intervention is scheduled",
    "required": false,
    "example": "2025-03-25T09:00:00Z"
  },
  "completion_date": {
    "type": "date",
    "description": "Date when the intervention was completed",
    "required": false,
    "example": "2025-03-25T11:30:00Z"
  },
  "operations": {
    "type": "array",
    "description": "List of operations included in this intervention",
    "required": true,
    "items": {
      "type": "string",
      "description": "Reference to an operation",
      "example": "op_12345678"
    }
  },
  "documents": {
    "type": "array",
    "description": "List of documents generated for this intervention",
    "required": false,
    "items": {
      "type": "string",
      "description": "Reference to a document",
      "example": "doc_12345678"
    }
  },
  "photos": {
    "type": "array",
    "description": "List of photos captured during this intervention",
    "required": false,
    "items": {
      "type": "string",
      "description": "Reference to a photo",
      "example": "pht_12345678"
    }
  },
  "notes": {
    "type": "string",
    "description": "Additional notes about the intervention",
    "required": false,
    "example": "Customer requested installation before noon. Access code for building: 1234."
  },
  "source": {
    "type": "string",
    "description": "Source of the intervention (user-created or admin-assigned)",
    "required": true,
    "enum": ["user", "admin", "api"],
    "example": "user"
  },
  "symphonics_sync_status": {
    "type": "object",
    "description": "Status of synchronization with Symphonics platform",
    "required": false,
    "properties": {
      "status": {
        "type": "string",
        "enum": ["pending", "success", "error"],
        "example": "success"
      },
      "last_sync_date": {
        "type": "date",
        "example": "2025-03-25T12:00:00Z"
      },
      "error_message": {
        "type": "string",
        "example": null
      },
      "symphonics_id": {
        "type": "string",
        "example": "sym_87654321"
      }
    }
  }
}
```

## Status Definitions

### A_traiter (To Process)
Intervention has been created but not yet started.

**Transitions**:
- → En_cours (when opened by an installer)
- → canceled (if canceled before starting)

### En_cours (In Progress)
Intervention is being processed by an installer.

**Transitions**:
- → quote_sent (when a quote is sent for signature)
- → invoice_sent (when skipping quote and sending invoice directly)
- → canceled (if canceled during processing)

### quote_sent
Quote has been sent to the beneficiary for signature.

**Transitions**:
- → quote_signed (when the quote is signed)
- → canceled (if canceled after quote is sent)

### quote_signed
Quote has been signed by the beneficiary.

**Transitions**:
- → invoice_sent (when invoice is sent after quote signature)
- → canceled (if canceled after quote is signed)

### invoice_sent
Invoice and associated documents have been sent for signature.

**Transitions**:
- → installation_done (when documents are signed)
- → canceled (if canceled after invoice is sent)

### installation_done
Installation is complete and all documents are signed.

**Transitions**:
- → published (when data is published to Symphonics)
- → maintenance_required (if issues are identified)

### published
Data has been successfully published to the Symphonics platform.

**Transitions**:
- → maintenance_required (if issues are identified)

### canceled
Intervention has been canceled.

**Transitions**:
- None (terminal state)

### maintenance_required
Intervention requires maintenance or follow-up.

**Transitions**:
- → En_cours (when maintenance is started)
- → installation_done (when maintenance is completed)

## Validation Rules

1. **PDL/PCE Validation**:
   - PDL must be validated against Symphonics API
   - PDL format must conform to standard patterns
   - At least one of PDL or PCE must be provided

2. **Address Validation**:
   - Address fields must be complete
   - Postal code must conform to country format
   - Geolocation coordinates should be provided when available

3. **Beneficiary Validation**:
   - Email must be a valid format
   - Phone must be a valid format
   - Name fields cannot be empty

4. **Status Transitions**:
   - Status changes must follow the defined state machine
   - Certain status transitions require specific conditions to be met

## Relationships

1. **User**: Each intervention is managed by exactly one user (many-to-one relationship).
2. **Company**: Each intervention is performed by exactly one company (many-to-one relationship).
3. **Operations**: Each intervention includes multiple operations (one-to-many relationship).
4. **Documents**: Each intervention generates multiple documents (one-to-many relationship).
5. **Photos**: Each intervention captures multiple photos (one-to-many relationship).

## Indexing

1. **Primary Key**: `id`
2. **Foreign Keys**: `user_id`, `company_id`
3. **Search Indexes**: `beneficiary_last_name`, `postal_code`, `pdl`, `pce`
4. **Geospatial Indexes**: `coordinates`
5. **Status Indexes**: `status`, `creation_date`
6. **Compound Indexes**: `(user_id, status)`, `(company_id, status)`, `(scheduled_date, status)`

## API Endpoints

1. **GET /interventions**: List interventions (with filtering and pagination)
2. **GET /interventions/{id}**: Get intervention details
3. **POST /interventions**: Create new intervention
4. **PUT /interventions/{id}**: Update intervention
5. **PATCH /interventions/{id}/status**: Update intervention status
6. **DELETE /interventions/{id}**: Cancel intervention (sets status to canceled)
7. **GET /interventions/{id}/operations**: Get operations for an intervention
8. **GET /interventions/{id}/documents**: Get documents for an intervention
9. **GET /interventions/{id}/photos**: Get photos for an intervention

## Implementation Notes

1. **Storage**: Intervention data is stored in the primary database.
2. **Offline Support**: Intervention data is cached for offline access with synchronization upon connectivity restoration.
3. **Performance Considerations**: Intervention queries are optimized for mobile use with appropriate pagination and filtering.
4. **Geolocation**: Coordinates are captured with maximum precision allowed by the device.
5. **Status Tracking**: All status changes are logged with timestamps and actor information.
