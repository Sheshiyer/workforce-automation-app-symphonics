# Photo Entity

This document defines the schema for photos captured during interventions in the Workforce Automation App, including geolocation, timestamps, and OCR processing.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the photo",
    "required": true,
    "example": "pht_12345678"
  },
  "intervention_id": {
    "type": "string",
    "description": "Reference to the intervention this photo belongs to",
    "required": true,
    "example": "int_87654321"
  },
  "operation_id": {
    "type": "string",
    "description": "Reference to the operation this photo is associated with",
    "required": false,
    "example": "op_87654321"
  },
  "type": {
    "type": "string",
    "description": "Type of photo",
    "required": true,
    "enum": ["before", "after", "application", "fiscal_documents", "meter", "other"],
    "example": "before"
  },
  "subtype": {
    "type": "string",
    "description": "Subtype of photo for more specific categorization",
    "required": false,
    "example": "water_heater_front"
  },
  "file_path": {
    "type": "string",
    "description": "Path to the photo file",
    "required": true,
    "example": "photos/interventions/int_87654321/before_20250325_123045.jpg"
  },
  "thumbnail_path": {
    "type": "string",
    "description": "Path to the thumbnail version of the photo",
    "required": false,
    "example": "photos/interventions/int_87654321/thumbnails/before_20250325_123045.jpg"
  },
  "file_name": {
    "type": "string",
    "description": "Original file name of the photo",
    "required": true,
    "example": "before_20250325_123045.jpg"
  },
  "file_size": {
    "type": "number",
    "description": "Size of the photo file in bytes",
    "required": true,
    "example": 2456789
  },
  "file_type": {
    "type": "string",
    "description": "MIME type of the photo file",
    "required": true,
    "example": "image/jpeg"
  },
  "width": {
    "type": "number",
    "description": "Width of the photo in pixels",
    "required": true,
    "example": 3024
  },
  "height": {
    "type": "number",
    "description": "Height of the photo in pixels",
    "required": true,
    "example": 4032
  },
  "coordinates": {
    "type": "object",
    "description": "Geolocation coordinates where the photo was taken",
    "required": true,
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
      },
      "altitude": {
        "type": "number",
        "description": "Altitude in meters (if available)",
        "example": 35
      }
    }
  },
  "capture_time": {
    "type": "date",
    "description": "Timestamp when the photo was captured",
    "required": true,
    "example": "2025-03-25T12:30:45Z"
  },
  "upload_time": {
    "type": "date",
    "description": "Timestamp when the photo was uploaded to the system",
    "required": true,
    "example": "2025-03-25T12:31:15Z"
  },
  "device_info": {
    "type": "object",
    "description": "Information about the device used to capture the photo",
    "required": false,
    "properties": {
      "make": {
        "type": "string",
        "example": "Apple"
      },
      "model": {
        "type": "string",
        "example": "iPhone 16 Pro"
      },
      "os_version": {
        "type": "string",
        "example": "iOS 18.0"
      },
      "app_version": {
        "type": "string",
        "example": "1.2.0"
      }
    }
  },
  "exif_data": {
    "type": "object",
    "description": "EXIF metadata extracted from the photo",
    "required": false,
    "example": {
      "make": "Apple",
      "model": "iPhone 16 Pro",
      "exposure_time": "1/125",
      "f_number": "2.8",
      "iso": 100,
      "focal_length": "4.2mm",
      "flash": "No flash"
    }
  },
  "ocr_processed": {
    "type": "boolean",
    "description": "Whether OCR processing has been performed on this photo",
    "required": true,
    "example": true
  },
  "ocr_text": {
    "type": "string",
    "description": "Text extracted from the photo using OCR",
    "required": false,
    "example": "PDL: 12345678901234\nCompteur: 12345"
  },
  "ocr_confidence": {
    "type": "number",
    "description": "Confidence score of the OCR processing (0-100)",
    "required": false,
    "minimum": 0,
    "maximum": 100,
    "example": 85
  },
  "ocr_fields": {
    "type": "object",
    "description": "Structured data extracted from the photo using OCR",
    "required": false,
    "example": {
      "pdl": {
        "value": "12345678901234",
        "confidence": 92,
        "bounding_box": [100, 200, 300, 220]
      },
      "meter_reading": {
        "value": "12345",
        "confidence": 88,
        "bounding_box": [150, 250, 250, 270]
      }
    }
  },
  "ai_verification": {
    "type": "object",
    "description": "AI verification results for the photo",
    "required": false,
    "properties": {
      "is_authentic": {
        "type": "boolean",
        "description": "Whether the photo is authentic (not AI-generated)",
        "example": true
      },
      "confidence": {
        "type": "number",
        "description": "Confidence score of the authenticity verification (0-100)",
        "example": 95
      },
      "verification_time": {
        "type": "date",
        "example": "2025-03-25T12:32:00Z"
      }
    }
  },
  "tags": {
    "type": "array",
    "description": "Tags associated with the photo",
    "required": false,
    "items": {
      "type": "string",
      "example": "water_heater"
    },
    "example": ["water_heater", "regulator", "installation"]
  },
  "notes": {
    "type": "string",
    "description": "Additional notes about the photo",
    "required": false,
    "example": "Shows the water heater before installation of the regulator."
  },
  "status": {
    "type": "string",
    "description": "Status of the photo",
    "required": true,
    "enum": ["pending", "verified", "rejected", "archived"],
    "example": "verified"
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the photo record was created",
    "required": true,
    "example": "2025-03-25T12:31:15Z"
  },
  "modification_date": {
    "type": "date",
    "description": "Date when the photo record was last modified",
    "required": true,
    "example": "2025-03-25T12:35:00Z"
  }
}
```

## Photo Types

### Before
Photos taken before the intervention to document the initial state.

**Purpose**: To establish a baseline for comparison and document the pre-intervention condition of the equipment or site.

**Requirements**:
- Must be taken before any work begins
- Must clearly show the equipment or area to be modified
- Must include geolocation data
- Must be timestamped

### After
Photos taken after the intervention to document the completed work.

**Purpose**: To document the successful completion of the intervention and the proper installation of equipment.

**Requirements**:
- Must be taken after work is completed
- Must show the same perspective as the corresponding "before" photo
- Must clearly show the installed equipment or modifications
- Must include geolocation data
- Must be timestamped

### Application
Screenshots or photos of application interfaces related to the intervention.

**Purpose**: To document the proper configuration and operation of control applications for the installed equipment.

**Requirements**:
- Must show the application interface with relevant settings
- Must show the connection to the installed equipment
- Must include user account information where applicable

### Fiscal Documents
Photos of tax documents or income statements for precarity qualification.

**Purpose**: To document the beneficiary's eligibility for enhanced subsidies based on income level.

**Requirements**:
- Must clearly show the relevant income information
- Must include the beneficiary's name and tax identification
- Must be from the current or previous tax year
- Must be handled with appropriate privacy controls

### Meter
Photos of electricity or gas meters showing PDL or PCE numbers.

**Purpose**: To document the correct identification of the energy supply point.

**Requirements**:
- Must clearly show the meter with its identification number
- Must be legible for OCR processing
- Must include geolocation data
- Must be timestamped

## Geolocation Requirements

All photos (except application screenshots) must include precise geolocation data:

1. **Precision**: Coordinates must be captured with maximum precision allowed by the device
2. **Validation**: Geolocation must be within a reasonable distance of the intervention address
3. **Completeness**: Both latitude and longitude are required; altitude is optional
4. **Accuracy**: Accuracy information must be included when available

If geolocation data is missing or invalid, the photo capture should be blocked with an appropriate error message.

## OCR Processing

Certain photo types (meter, fiscal documents) undergo OCR processing to extract relevant information:

1. **Text Extraction**: Full text content is extracted from the image
2. **Field Recognition**: Specific fields (PDL, PCE, income amounts) are identified and extracted
3. **Confidence Scoring**: Each extracted field includes a confidence score
4. **Validation**: Extracted data is validated against expected formats and ranges
5. **Manual Review**: Low-confidence extractions are flagged for manual review

## AI Verification

To ensure photo authenticity, AI verification may be applied:

1. **Authenticity Check**: Verifies that the photo was captured by a real camera (not AI-generated)
2. **Manipulation Detection**: Identifies potential digital manipulation or editing
3. **Confidence Scoring**: Provides a confidence score for the verification result
4. **Flagging**: Suspicious photos are flagged for manual review

## Status Definitions

### pending
Photo has been uploaded but not yet verified.

**Transitions**:
- → verified (when validated by the system or a reviewer)
- → rejected (if found to be invalid or inappropriate)

### verified
Photo has been verified and accepted.

**Transitions**:
- → rejected (if later found to be invalid)
- → archived (after retention period)

### rejected
Photo has been rejected due to quality issues, missing data, or other problems.

**Transitions**:
- → archived (after retention period)

### archived
Photo has been archived for long-term storage.

**Transitions**:
- None (terminal state)

## Relationships

1. **Intervention**: Each photo belongs to exactly one intervention (many-to-one relationship).
2. **Operation**: Each photo may be associated with a specific operation (many-to-one relationship).

## Indexing

1. **Primary Key**: `id`
2. **Foreign Keys**: `intervention_id`, `operation_id`
3. **Search Indexes**: `type`, `capture_time`, `status`
4. **Geospatial Indexes**: `coordinates`
5. **Compound Indexes**: `(intervention_id, type)`, `(operation_id, type)`

## API Endpoints

1. **GET /photos**: List photos (with filtering and pagination)
2. **GET /photos/{id}**: Get photo details
3. **GET /photos/{id}/file**: Download photo file
4. **GET /photos/{id}/thumbnail**: Download photo thumbnail
5. **POST /photos**: Upload new photo
6. **PATCH /photos/{id}/status**: Update photo status
7. **POST /photos/{id}/ocr**: Trigger OCR processing for a photo
8. **POST /photos/{id}/verify**: Trigger AI verification for a photo

## Implementation Notes

1. **Storage**: Photo files are stored in a secure file system with appropriate access controls.
2. **Thumbnails**: Thumbnails are generated for efficient display in the mobile application.
3. **Compression**: Photos may be compressed for storage efficiency while preserving essential details.
4. **Offline Support**: Photos can be captured offline and uploaded when connectivity is restored.
5. **Batch Processing**: OCR and AI verification may be performed in batch for efficiency.
