# INT001: Start Intervention

## Overview

This ticket covers the implementation of the intervention creation functionality for installers. This is a core MVP feature that enables installers to start new interventions or begin work on assigned interventions.

## User Story

**As an** installer,  
**I want to** create new interventions or start assigned interventions,  
**So that** I can begin the data collection and installation process for a customer.

## Acceptance Criteria

1. Installers can create a new intervention from the homepage using the "+" button
2. Installers can view and select interventions assigned to them from the "dossiers à traiter" (jobs to process) list
3. When an installer opens an intervention, its status automatically changes from "A_traiter" to "En_cours"
4. If the intervention was pre-loaded by administration, form fields should be pre-populated
5. If the intervention is created by the installer, the form should open empty
6. The interface must be optimized for mobile use
7. The system must handle offline creation with synchronization when connectivity is restored
8. The intervention creation process must follow the three-step workflow (customer info, operations, photos)
9. The system must save intervention data automatically as the installer progresses through the steps
10. The installer must be able to navigate between steps using next/previous buttons

## Technical Details

### API Endpoints

- `POST /api/interventions` - Create a new intervention
- `GET /api/interventions` - List interventions (with filtering and pagination)
- `GET /api/interventions/{id}` - Get intervention details
- `PATCH /api/interventions/{id}/status` - Update intervention status
- `PUT /api/interventions/{id}` - Update intervention details

### Data Model

The intervention entity should include:
- `id` (string): Unique identifier
- `user_id` (string): Reference to the user managing the intervention
- `company_id` (string): Reference to the company performing the intervention
- `beneficiary_first_name` (string): First name of the customer
- `beneficiary_last_name` (string): Last name of the customer
- `beneficiary_email` (string): Email address of the customer
- `beneficiary_phone` (string): Phone number of the customer
- `address` (string): Street address of the intervention location
- `postal_code` (string): Postal code of the intervention location
- `city` (string): City of the intervention location
- `pdl` (string): PDL (Point de Livraison) number for electricity
- `pce` (string): PCE (Point de Comptage et d'Estimation) number for gas
- `status` (string): Current status of the intervention
- `creation_date` (date): When the intervention was created
- `modification_date` (date): When the intervention was last modified
- `scheduled_date` (date): When the intervention is scheduled (if applicable)
- `source` (string): Source of the intervention (user, admin, api)

### UI Components

1. **Homepage Dashboard**
   - Status-based grouping of interventions
   - Floating action button ("+") for creating new interventions
   - Search functionality
   - Pull-to-refresh for updates

2. **Intervention List**
   - Card-based list items with key information
   - Status indicators with color coding
   - Sort and filter options
   - Offline indicators when appropriate

3. **Intervention Creation Flow**
   - Progress indicator ("metro line" style)
   - Form with appropriate input controls
   - Validation feedback
   - Navigation controls (next/previous)
   - Auto-save functionality

### Validation Rules

1. Required fields for creation: user_id, company_id
2. Status transitions must follow the defined state machine
3. Creation date and modification date are system-generated
4. Source must be one of the defined values (user, admin, api)

### Offline Support

1. New interventions can be created offline and stored locally
2. Local storage uses a consistent schema with the API
3. Synchronization occurs automatically when connectivity is restored
4. Conflict resolution handles cases where the same intervention is modified online and offline

## Dependencies

- User authentication and authorization system
- Company management system
- Offline storage and synchronization mechanism
- Mobile-optimized UI components

## Testing Scenarios

### Functional Testing

1. Create a new intervention while online
2. Create a new intervention while offline
3. Open an assigned intervention
4. Verify status change from "A_traiter" to "En_cours"
5. Navigate through the intervention steps
6. Test auto-save functionality
7. Test synchronization after offline creation

### Edge Cases

1. Handle connectivity loss during intervention creation
2. Test with very large numbers of assigned interventions
3. Verify behavior when server is unavailable
4. Test with various mobile device sizes and orientations
5. Verify performance with slow network connections

## Implementation Notes

1. Use a responsive design approach for mobile optimization
2. Implement efficient local storage for offline support
3. Ensure proper error handling for all API endpoints
4. Include loading indicators for network operations
5. Implement optimistic UI updates for better user experience

## Estimation

- Backend API development: 2 days
- UI development: 3 days
- Offline support: 2 days
- Testing and bug fixes: 2 days
- Documentation: 1 day
- Total: 10 days

## Definition of Done

1. All acceptance criteria are met
2. Code passes all automated tests
3. UI is responsive and follows design guidelines
4. Offline functionality works as expected
5. Documentation is complete
6. Code has been reviewed and approved
7. Feature has been tested on multiple mobile devices
