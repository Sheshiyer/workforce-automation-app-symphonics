# MVP Scope Definition

This document provides an explicit definition of the Minimum Viable Product (MVP) features, use cases, and deliverables for the Workforce Automation App.

## MVP Objectives

The primary objectives of the MVP are to:

1. Enable field teams to perform water heater regulator installations and associated Symphonics contract signings
2. Provide a mobile-first application that works on standard 4G networks
3. Eliminate paper-based processes through digital documentation and electronic signatures
4. Integrate with the Symphonics platform for data exchange
5. Support basic reporting and commission tracking

## MVP Features by Module

### Administration Module

| Feature ID | Description | Priority |
|------------|-------------|----------|
| ADM001 | Company account creation by back office | MVP |
| ADM002 | User self-registration with company code | MVP |
| ADM004 | User management panel for back office | MVP |
| ADM006 | Manual intervention list administration | MVP |
| ADM007 | CSV upload for intervention lists | MVP |

### Intervention Module

| Feature ID | Description | Priority |
|------------|-------------|----------|
| INT001 | Create intervention | MVP |
| INT002 | Intervention data entry workflow | MVP |
| INT003 | Customer information form (Page 1) | MVP |
| INT005 | Operations selection form (Page 2) | MVP |
| INT009 | Photo documentation form (Page 3) | MVP |
| INT011 | Intervention finalization | MVP |
| INT013 | Photo geotagging | MVP |

### Reporting Module

| Feature ID | Description | Priority |
|------------|-------------|----------|
| REP001 | Intervention tracking dashboard | MVP |
| COM001 | Basic reporting and commission tracking | MVP |

### Integration Module

| Feature ID | Description | Priority |
|------------|-------------|----------|
| IT001 | Publish completed interventions to Symphonics | MVP |

## MVP Limitations

The following features are explicitly excluded from the MVP:

1. **User visibility rules**: Users will only see interventions they created or that are assigned to them, without organizational visibility rules.
2. **Products beyond water heater regulators**: Only water heater regulator installations will be supported.
3. **Price simulation and quote sending**: These features are not required for water heater regulator installations.
4. **Third-party integrations**: Only Symphonics platform integration is included; other integrations (HIVE, Thaléos SAV) are deferred.
5. **Advanced features**:
   - Address geolocation (INT004)
   - Contextual operations filtering (INT006)
   - Price simulation (INT007)
   - Quote sending (INT008)
   - Conditional photo blocks (INT010)
   - Dynamic document generation (INT012)
   - OCR processing (INT014)
   - Electronic signature via SMS (INT015)
   - Map-based visualization (REP002)
   - Calendar view (REP003)
   - Partner distribution (IT002)
   - Personal data management (RGP001)

## MVP Use Cases

### 1. Company and User Management

**Primary Actor**: Back Office Administrator

**Flow**:
1. Administrator creates a company account with verification details
2. Administrator generates a unique company code
3. Administrator communicates the code to the company
4. Installer downloads the app and registers using the company code
5. Administrator approves the user registration
6. Installer can now log in and access the application

### 2. Intervention Assignment

**Primary Actor**: Back Office Administrator

**Flow**:
1. Administrator creates intervention records manually or via CSV upload
2. Administrator assigns interventions to specific installers
3. Installers see assigned interventions in their dashboard

### 3. Intervention Execution

**Primary Actor**: Installer

**Flow**:
1. Installer logs into the app and views assigned interventions
2. Installer selects an intervention to begin
3. Installer enters customer and site information
4. Installer validates PDL number with Symphonics API
5. Installer selects water heater regulator operation
6. Installer takes "before" photos with geolocation
7. Installer performs the physical installation
8. Installer takes "after" photos with geolocation
9. Installer takes application photos
10. Installer takes meter photos
11. Installer finalizes the intervention
12. System generates required documents (invoice, attestation, contract)
13. Documents are sent for electronic signature
14. Customer signs documents via email
15. Intervention is marked as complete
16. Data is published to Symphonics platform

### 4. Reporting and Commission Tracking

**Primary Actor**: Back Office Administrator

**Flow**:
1. Administrator accesses reporting dashboard
2. Administrator views intervention statistics by installer
3. Administrator generates commission reports
4. Administrator exports data for further analysis

## MVP Technical Scope

### Mobile Application

- **Platform**: Progressive Web App (PWA) optimized for mobile browsers
- **Offline Support**: Basic offline functionality for data entry with synchronization
- **Device Features**: Camera access, geolocation, local storage
- **Responsive Design**: Optimized for smartphone screens with support for larger displays

### Backend Services

- **API Layer**: RESTful API for mobile application communication
- **Database**: Relational database for structured data storage
- **File Storage**: Secure storage for photos and documents
- **Authentication**: Email-based authentication with role-based access control
- **Integration**: API integration with Symphonics platform

### Infrastructure

- **Hosting**: Cloud-based hosting with appropriate scaling
- **Security**: HTTPS encryption, secure data storage, access controls
- **Monitoring**: Basic application monitoring and error tracking
- **Backup**: Regular database and file backups

## MVP Deliverables

1. **Mobile Application**: Progressive Web App accessible via mobile browsers
2. **Administrative Interface**: Web-based interface for back office operations
3. **API Documentation**: Documentation for all backend APIs
4. **User Documentation**: User guides for installers and administrators
5. **Deployment Documentation**: Instructions for deployment and configuration

## Success Criteria

The MVP will be considered successful if:

1. Field teams can complete water heater regulator installations end-to-end without paper processes
2. Customer data and signatures are captured digitally
3. Intervention data is successfully transmitted to the Symphonics platform
4. Back office staff can manage companies, users, and interventions
5. Basic reporting provides visibility into installer activity and commissions
6. The application functions reliably on standard 4G networks

## Timeline Considerations

The MVP development prioritizes rapid implementation of core functionality:

1. **Phase 1**: Core infrastructure, authentication, and company/user management
2. **Phase 2**: Intervention workflow and photo capture
3. **Phase 3**: Document generation and electronic signature
4. **Phase 4**: Symphonics integration and reporting
5. **Phase 5**: Testing, refinement, and deployment

All simplifications that enable rapid MVP deployment are encouraged, provided they maintain the core functionality required for water heater regulator installations and Symphonics contract signing.
