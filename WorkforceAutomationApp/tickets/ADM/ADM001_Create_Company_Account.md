# ADM001: Create Company Account

## Overview

This ticket covers the implementation of the company account creation functionality for back office administrators. This is a core MVP feature that enables the creation and management of installer companies in the system.

## User Story

**As a** back office administrator,  
**I want to** create and manage company accounts with verification details,  
**So that** installer companies can be properly registered and verified before their users can access the system.

## Acceptance Criteria

1. Back office administrators can create new company accounts with the following information:
   - Company name
   - SIRET number (with validation)
   - Address (street, postal code, city)
   - Contact information (name, email, phone)
   - Verification documents (KBIS, RGE certification)
2. The system generates a unique installer application code for each company
3. The system records the creation date of the installer application code
4. Back office administrators can specify which operations are authorized for the company
5. The company status can be set to "pending" or "active"
6. Company information can be edited after creation
7. Company accounts cannot be deleted, only deactivated
8. The UI provides a simple form-based interface for company management
9. Validation ensures all required fields are completed
10. SIRET number validation follows French company identification rules

## Technical Details

### API Endpoints

- `POST /api/companies` - Create a new company
- `GET /api/companies` - List all companies (with filtering and pagination)
- `GET /api/companies/{id}` - Get company details
- `PUT /api/companies/{id}` - Update company details
- `PATCH /api/companies/{id}/status` - Update company status
- `PATCH /api/companies/{id}/operations` - Update authorized operations

### Data Model

The company entity should include:
- `id` (string): Unique identifier
- `name` (string): Company name
- `siret` (string): SIRET number
- `address` (string): Street address
- `postal_code` (string): Postal code
- `city` (string): City
- `contact_name` (string): Name of primary contact
- `contact_email` (string): Email of primary contact
- `contact_phone` (string): Phone number of primary contact
- `installer_application_code` (string): Unique code for user registration
- `installer_application_code_creation_date` (date): When the code was created
- `operation_authorized` (array): List of authorized operation codes
- `verification_documents` (object): References to uploaded documents
- `status` (string): Current status (pending, active, suspended, inactive)
- `creation_date` (date): When the company was created
- `modification_date` (date): When the company was last modified

### UI Components

1. **Company List View**
   - Sortable and filterable table of companies
   - Status indicators with color coding
   - Action buttons for edit, status change, etc.

2. **Company Creation Form**
   - Multi-section form with validation
   - Document upload controls
   - Code generation button
   - Operation selection interface

3. **Company Detail View**
   - Tabbed interface for different information categories
   - Document preview
   - Status history
   - Associated users list

### Validation Rules

1. SIRET number must be 14 digits and follow French validation rules
2. Email addresses must be valid format
3. Phone numbers must be valid format
4. Required fields: name, siret, address, postal_code, city, contact_name, contact_email
5. Installer application code must be unique

### Security Considerations

1. Only users with back office administrator role can access company management
2. All actions are logged for audit purposes
3. Document access is restricted to authorized personnel
4. SIRET and other sensitive information is handled according to data protection regulations

## Dependencies

- User authentication and authorization system
- Document storage system
- Code generation service
- SIRET validation service

## Testing Scenarios

### Functional Testing

1. Create a new company with valid information
2. Attempt to create a company with invalid SIRET
3. Generate installer application code
4. Edit company information
5. Change company status
6. Update authorized operations
7. View company details
8. Filter and sort company list

### Edge Cases

1. Attempt to create a company with duplicate SIRET
2. Handle very long company names or addresses
3. Test with various document formats and sizes
4. Test pagination with large number of companies
5. Verify behavior when document storage is unavailable

## Implementation Notes

1. Use a secure algorithm for generating installer application codes
2. Implement proper error handling for all API endpoints
3. Ensure responsive design for administrative interface
4. Include comprehensive logging for audit purposes
5. Consider implementing a document verification workflow

## Estimation

- Backend API development: 3 days
- UI development: 4 days
- Testing and bug fixes: 2 days
- Documentation: 1 day
- Total: 10 days

## Definition of Done

1. All acceptance criteria are met
2. Code passes all automated tests
3. UI is responsive and follows design guidelines
4. Documentation is complete
5. Code has been reviewed and approved
6. Feature has been tested in staging environment
