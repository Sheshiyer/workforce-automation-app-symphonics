# IT001: Publish to Symphonics

## Overview

This ticket covers the implementation of the integration with the Symphonics platform for publishing completed intervention data. This is a core MVP feature that enables automatic transmission of intervention data to the Symphonics platform once an intervention is completed and all documents are signed.

## User Story

**As an** IT administrator,  
**I want to** automatically publish completed intervention data to the Symphonics platform,  
**So that** customer information, installation details, and signed documents are properly recorded in the central system.

## Acceptance Criteria

1. When an intervention reaches status "installation_done" and the beneficiary has signed a Symphonics contract, the system automatically sends the dossier to the Symphonics platform
2. The integration uses the API documented at https://api.symphonics.fr/crm/docs
3. The system handles API responses appropriately, including success confirmations and error handling
4. The transmission includes all relevant intervention data:
   - Beneficiary information
   - Installation details
   - Operation codes
   - Product information
   - Document references
   - Photo references
5. The system maintains a record of successful transmissions and implements retry logic for failed attempts
6. Upon successful transmission, the intervention status is updated to "published"
7. The system provides monitoring and logging of all API interactions
8. Administrators can view the synchronization status of each intervention

## Technical Details

### API Endpoints

- `POST /api/interventions/{id}/publish` - Manually trigger publication to Symphonics
- `GET /api/interventions/{id}/sync-status` - Get synchronization status for an intervention
- `POST /api/webhooks/symphonics` - Receive callbacks from Symphonics API

### Integration Flow

1. **Trigger Conditions**:
   - Automatic: When intervention status changes to "installation_done" and all required documents are signed
   - Manual: Through admin interface or API endpoint

2. **Data Preparation**:
   - Collect all required intervention data
   - Format according to Symphonics API requirements
   - Validate data completeness and format

3. **API Communication**:
   - Authenticate with Symphonics API
   - Send data using appropriate endpoints
   - Process response and handle errors

4. **Status Tracking**:
   - Record transmission attempts
   - Update intervention with synchronization status
   - Implement retry mechanism for failed attempts

### Data Mapping

| Workforce App Field | Symphonics API Field | Notes |
|---------------------|----------------------|-------|
| beneficiary_first_name | customer.firstName | Required |
| beneficiary_last_name | customer.lastName | Required |
| beneficiary_email | customer.email | Required |
| beneficiary_phone | customer.phone | Required |
| address | installation.address | Required |
| postal_code | installation.postalCode | Required |
| city | installation.city | Required |
| pdl | installation.pdl | Required |
| pce | installation.pce | Optional |
| operations[].code | services[].code | Required |
| products[].name | equipment[].name | Required |
| products[].model | equipment[].model | Required |
| documents[].file_path | documents[].url | Required |
| photos[].file_path | photos[].url | Required |

### Error Handling

1. **Connection Errors**:
   - Implement exponential backoff for retries
   - Alert administrators after multiple failures
   - Store failed requests for manual resolution

2. **Validation Errors**:
   - Log detailed error information
   - Flag intervention for review
   - Provide clear error messages in admin interface

3. **Authentication Errors**:
   - Refresh authentication tokens when expired
   - Alert administrators on persistent auth failures
   - Implement secure credential storage

### Security Considerations

1. API credentials are stored securely using environment variables or a secrets manager
2. All API communication uses HTTPS
3. Authentication tokens are refreshed regularly
4. Access to synchronization functions is restricted to authorized personnel
5. Sensitive data is handled according to data protection regulations

## Dependencies

- Intervention management system
- Document storage system
- Authentication and authorization system
- Logging and monitoring infrastructure
- Symphonics API access credentials

## Testing Scenarios

### Functional Testing

1. Successful publication of a complete intervention
2. Handling of various API response codes
3. Retry mechanism for temporary failures
4. Manual triggering of publication
5. Status tracking and reporting

### Edge Cases

1. Handling very large interventions with many photos
2. Network interruptions during transmission
3. API rate limiting and throttling
4. Concurrent publication requests
5. Handling of unexpected API responses

## Implementation Notes

1. Use a queue-based architecture for reliability
2. Implement comprehensive logging for troubleshooting
3. Create a dedicated service for Symphonics integration
4. Consider implementing a circuit breaker pattern for API stability
5. Develop monitoring dashboards for integration health

## Estimation

- API integration development: 4 days
- Error handling and retry logic: 2 days
- Testing and validation: 3 days
- Documentation: 1 day
- Total: 10 days

## Definition of Done

1. All acceptance criteria are met
2. Integration passes all automated tests
3. Documentation is complete, including API reference
4. Monitoring and alerting are configured
5. Code has been reviewed and approved
6. Feature has been tested in staging environment with Symphonics test API
