# Operation Entity

This document defines the schema for operations in the Workforce Automation App, including linked products and compliance requirements.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the operation",
    "required": true,
    "example": "op_12345678"
  },
  "intervention_id": {
    "type": "string",
    "description": "Reference to the intervention this operation belongs to",
    "required": true,
    "example": "int_87654321"
  },
  "code": {
    "type": "string",
    "description": "CEE operation code",
    "required": true,
    "example": "BAR-TH-173"
  },
  "name": {
    "type": "string",
    "description": "Human-readable name of the operation",
    "required": true,
    "example": "Régulateur de ballon d'eau chaude"
  },
  "description": {
    "type": "string",
    "description": "Detailed description of the operation",
    "required": false,
    "example": "Installation d'un régulateur sur un ballon d'eau chaude électrique existant permettant un pilotage de la mise en température en fonction des besoins."
  },
  "obliged_parties": {
    "type": "array",
    "description": "List of obligated parties for CEE operations",
    "required": false,
    "items": {
      "type": "string",
      "example": "EDF"
    },
    "example": ["EDF", "Engie", "Total"]
  },
  "selected_obliged_party": {
    "type": "string",
    "description": "Selected obligated party for this operation",
    "required": false,
    "example": "EDF"
  },
  "related_documents": {
    "type": "array",
    "description": "List of document types required for this operation",
    "required": true,
    "items": {
      "type": "string",
      "enum": ["quote", "invoice", "attestation_honneur", "attestation_fin_travaux", "contrat_symphonics"],
      "example": "attestation_honneur"
    },
    "example": ["invoice", "attestation_honneur", "contrat_symphonics"]
  },
  "related_photos": {
    "type": "array",
    "description": "List of photo types required for this operation",
    "required": true,
    "items": {
      "type": "string",
      "enum": ["before", "after", "application", "fiscal_documents", "meter"],
      "example": "before"
    },
    "example": ["before", "after", "application", "meter"]
  },
  "products": {
    "type": "array",
    "description": "List of products associated with this operation",
    "required": true,
    "items": {
      "type": "string",
      "description": "Reference to a product",
      "example": "prd_12345678"
    }
  },
  "quantity": {
    "type": "number",
    "description": "Quantity of operations performed",
    "required": true,
    "minimum": 1,
    "example": 1
  },
  "unit_price": {
    "type": "number",
    "description": "Unit price for this operation (excluding products)",
    "required": false,
    "example": 50.00
  },
  "total_price": {
    "type": "number",
    "description": "Total price for this operation (including products)",
    "required": false,
    "example": 150.00
  },
  "cee_value": {
    "type": "number",
    "description": "Energy savings value in kWh cumac",
    "required": false,
    "example": 45000
  },
  "eligibility_criteria": {
    "type": "object",
    "description": "Criteria that must be met for CEE eligibility",
    "required": false,
    "properties": {
      "building_type": {
        "type": "array",
        "items": {
          "type": "string",
          "enum": ["residential", "commercial", "industrial", "public"],
          "example": "residential"
        }
      },
      "construction_year": {
        "type": "object",
        "properties": {
          "min": {
            "type": "number",
            "example": 0
          },
          "max": {
            "type": "number",
            "example": 2025
          }
        }
      },
      "heating_type": {
        "type": "array",
        "items": {
          "type": "string",
          "enum": ["electric", "gas", "oil", "wood", "other"],
          "example": "electric"
        }
      },
      "precarity_eligible": {
        "type": "boolean",
        "example": true
      }
    }
  },
  "active": {
    "type": "boolean",
    "description": "Whether this operation is active and available for selection",
    "required": true,
    "example": true
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the operation was created",
    "required": true,
    "example": "2025-03-15T10:30:00Z"
  },
  "modification_date": {
    "type": "date",
    "description": "Date when the operation was last modified",
    "required": true,
    "example": "2025-03-20T14:45:00Z"
  }
}
```

## CEE Operation Codes

CEE (Certificats d'Économies d'Énergie) operation codes define standardized energy efficiency operations recognized by the French government. Each code corresponds to a specific type of intervention with defined eligibility criteria and energy savings calculations.

### Common CEE Codes

1. **BAR-TH-173**: Installation of a water heater regulator
2. **BAR-TH-171**: Installation of a programmable thermostat
3. **BAR-EQ-115**: Installation of energy-efficient lighting systems
4. **BAR-TH-104**: Installation of a heat pump for space heating
5. **BAR-TH-106**: Installation of a heat pump for domestic hot water

### Code Structure

CEE codes follow a standardized structure:
- Sector prefix (e.g., BAR for residential buildings)
- Technology category (e.g., TH for thermal, EQ for equipment)
- Numeric identifier

## Document Requirements

Each operation type requires specific documentation to comply with CEE regulations and business processes:

1. **Quote**: Price proposal before work begins (optional for some operations)
2. **Invoice**: Detailed billing document after work completion
3. **Attestation d'Honneur (AH)**: Sworn statement by the beneficiary
4. **Attestation de Fin de Travaux (AFT)**: Work completion certificate
5. **Contrat Symphonics**: Service contract for the Symphonics platform

## Photo Requirements

Each operation type requires specific photographic evidence:

1. **Before Photos**: Documentation of the situation before intervention
2. **After Photos**: Documentation of the completed installation
3. **Application Photos**: Screenshots of control applications
4. **Fiscal Documents**: Photos of income tax notices (for precarity qualification)
5. **Meter Photos**: Photos of electricity or gas meters showing PDL/PCE numbers

## Relationships

1. **Intervention**: Each operation belongs to exactly one intervention (many-to-one relationship).
2. **Products**: Each operation can include multiple products (one-to-many relationship).

## Validation Rules

1. **Code Validation**:
   - Operation code must be a valid CEE code
   - Code must be in the list of authorized operations for the company

2. **Eligibility Validation**:
   - Intervention details must meet the eligibility criteria for the operation
   - Required documentation must be complete

3. **Pricing Validation**:
   - Total price must be consistent with unit price and quantity
   - Product prices must be included in the total price calculation

## Indexing

1. **Primary Key**: `id`
2. **Foreign Keys**: `intervention_id`
3. **Search Indexes**: `code`, `name`
4. **Compound Indexes**: `(intervention_id, code)`, `(code, active)`

## API Endpoints

1. **GET /operations**: List operations (with filtering and pagination)
2. **GET /operations/{id}**: Get operation details
3. **POST /operations**: Create new operation
4. **PUT /operations/{id}**: Update operation
5. **GET /operations/codes**: Get available operation codes
6. **GET /operations/{id}/products**: Get products for an operation

## Implementation Notes

1. **Storage**: Operation data is stored in the primary database.
2. **Catalog Management**: Operation codes and their requirements are managed through an administrative interface.
3. **Validation Logic**: Complex eligibility rules are implemented in the application logic rather than the data model.
4. **Documentation**: Operation codes and requirements are documented for installers to reference.
