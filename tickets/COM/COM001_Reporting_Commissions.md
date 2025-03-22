# COM001: Reporting and Commission System

## Overview

This ticket covers the implementation of the reporting and commission calculation system for tracking installer activity and calculating commissions. This is a core MVP feature that enables monitoring of performance and commission-based compensation for installers.

## User Story

**As a** back office administrator,  
**I want to** track installer activity and calculate commissions based on completed interventions,  
**So that** I can monitor performance and properly compensate installers for their work.

## Acceptance Criteria

1. The system integrates with a reporting tool (e.g., Looker) to query the database for activity tracking
2. The reporting system provides:
   - Intervention counts by installer
   - Intervention counts by status
   - Intervention counts by operation type
   - Intervention counts by time period
   - Conversion rates from quote to signed contract
3. The commission calculation system:
   - Calculates commissions based on completed interventions
   - Supports different commission rates by operation type
   - Supports different commission rates by installer or company
   - Supports commission adjustments based on performance metrics
4. The reporting interface provides appropriate filtering and export capabilities
5. Access to commission data is restricted based on user roles
6. Reports can be generated for custom date ranges
7. Data can be exported in common formats (CSV, Excel, PDF)
8. The system provides visualizations of key performance indicators

## Technical Details

### API Endpoints

- `GET /api/reports/interventions` - Get intervention statistics with filtering
- `GET /api/reports/commissions` - Get commission calculations with filtering
- `GET /api/reports/performance` - Get performance metrics with filtering
- `POST /api/reports/export` - Export report data in specified format

### Data Model

The reporting system will use the following data sources:
- Intervention records
- User records
- Company records
- Operation records
- Document records

Additional data structures:
- `commission_rates` (table): Defines commission rates by operation, company, and user
- `commission_calculations` (table): Stores calculated commissions for reporting periods
- `performance_metrics` (table): Stores derived performance indicators

### Reporting Metrics

1. **Activity Metrics**:
   - Total interventions
   - Interventions by status
   - Interventions by operation type
   - Daily/weekly/monthly trends

2. **Performance Metrics**:
   - Completion rate
   - Average time to completion
   - Quote-to-signature conversion rate
   - Customer satisfaction (future enhancement)

3. **Commission Metrics**:
   - Base commission amount
   - Performance adjustments
   - Total commission
   - Historical commission trends

### Commission Calculation

1. **Base Calculation**:
   ```
   base_commission = operation_count * commission_rate_per_operation
   ```

2. **Performance Adjustments**:
   ```
   performance_factor = completion_rate * conversion_rate * quality_factor
   adjusted_commission = base_commission * performance_factor
   ```

3. **Aggregation**:
   ```
   total_commission = sum(adjusted_commission) for all operations
   ```

### UI Components

1. **Dashboard**:
   - Summary cards with key metrics
   - Trend charts for interventions and commissions
   - Performance indicators with visual cues
   - Quick filters for time periods and users

2. **Detailed Reports**:
   - Tabular data with sorting and filtering
   - Expandable rows for detailed information
   - Export controls
   - Custom date range selection

3. **Commission Calculator**:
   - Commission rate configuration
   - Preview of calculations
   - Approval workflow for commission payments
   - Historical commission records

### Security Considerations

1. Access to commission data is restricted to authorized personnel
2. Personal performance data is only visible to the individual and their managers
3. Export functionality includes appropriate data protection measures
4. All reporting actions are logged for audit purposes

## Dependencies

- Intervention management system
- User and company management system
- Database with appropriate indexing for reporting queries
- Integration with reporting tool (e.g., Looker)
- Authentication and authorization system

## Testing Scenarios

### Functional Testing

1. Generate reports for various time periods
2. Calculate commissions with different rate structures
3. Export reports in different formats
4. Filter reports by different criteria
5. Verify metric calculations against manual calculations

### Edge Cases

1. Handle reporting with no data for selected period
2. Test with very large datasets
3. Verify performance with concurrent report generation
4. Test commission calculations with edge case values
5. Verify behavior when reporting tool is unavailable

## Implementation Notes

1. Use materialized views or pre-calculated aggregates for performance
2. Implement caching for frequently accessed reports
3. Schedule resource-intensive calculations during off-peak hours
4. Consider implementing a data warehouse for complex analytics
5. Design for extensibility to accommodate future reporting needs

## Estimation

- Database schema enhancements: 2 days
- Reporting API development: 3 days
- Commission calculation logic: 2 days
- UI development: 3 days
- Testing and validation: 2 days
- Documentation: 1 day
- Total: 13 days

## Definition of Done

1. All acceptance criteria are met
2. Reports are accurate and performant
3. Commission calculations are verified for correctness
4. UI is responsive and follows design guidelines
5. Documentation is complete
6. Code has been reviewed and approved
7. Feature has been tested with realistic data volumes
