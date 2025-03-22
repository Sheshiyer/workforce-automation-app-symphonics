# Workforce Automation App - File Structure

This document provides an overview of the project's file structure to help navigate and understand the organization of the codebase.

## Root Directory

- `README.md` - Main project documentation
- `overview.md` - Project overview and introduction
- `mvp_scope.md` - Minimum Viable Product scope definition
- `future_scope.md` - Future features and enhancements planned
- `FILE_STRUCTURE.md` - This file, providing a map of the project structure

## Project Structure

```
workforce-automation-app/
├── data_model/                  # Data models and database schema
│   ├── README.md                # Overview of data models
│   ├── User.md                  # User model specification
│   ├── Company.md               # Company model specification
│   ├── Intervention.md          # Intervention model specification
│   ├── Operation.md             # Operation model specification
│   ├── Document.md              # Document model specification
│   ├── Signature.md             # Signature model specification
│   └── Photo.md                 # Photo model specification
│
├── design/                      # Design documentation
│   ├── design.md                # General design principles and guidelines
│   ├── user_flows.md            # User flow diagrams and descriptions
│   └── wireframes/              # UI wireframes and mockups
│       ├── README.md            # Wireframes overview
│       ├── authentication-login-v1.md
│       ├── authentication-registration-v1.md
│       ├── authentication-password-recovery-v1.md
│       ├── dashboard-home-v1.md
│       ├── dashboard-intervention-list-v1.md
│       ├── intervention-details-v1.md
│       ├── administration-company-management-v1.md
│       ├── administration-user-management-v1.md
│       ├── administration-intervention-management-v1.md
│       ├── reporting-status-dashboard-v1.md
│       ├── reporting-commission-reports-v1.md
│       └── reporting-performance-metrics-v1.md
│
├── integration/                 # Integration specifications
│   └── symphonics_api.md        # Symphonics API integration documentation
│
├── prd/                         # Product Requirements Documents
│   ├── prd_admin.md             # Admin module requirements
│   ├── prd_integration.md       # Integration requirements
│   ├── prd_intervention.md      # Intervention module requirements
│   └── prd_reporting.md         # Reporting module requirements
│
├── resources/                   # Additional resources
│   └── Workforce Automation Specification.docx.pdf
│
├── tests/                       # Testing documentation
│   ├── test_plan.md             # Overall test plan
│   ├── qa_scenarios.md          # QA testing scenarios
│   └── edge_cases.md            # Edge cases to test
│
└── tickets/                     # Development tickets
    ├── ADM/                     # Administration tickets
    │   └── ADM001_Create_Company_Account.md
    ├── COM/                     # Commission tickets
    │   └── COM001_Reporting_Commissions.md
    ├── INT/                     # Intervention tickets
    │   └── INT001_Start_Intervention.md
    └── IT/                      # IT tickets
        └── IT001_Publish_to_Symphonics.md
```

## Directory Descriptions

### data_model/
Contains specifications for all data entities in the system, defining their attributes, relationships, and constraints.

### design/
Houses all design-related documentation including general design principles, user flows, and detailed wireframes for each screen in the application.

### integration/
Documentation for external system integrations, currently focused on the Symphonics API.

### prd/
Product Requirements Documents that detail the functional and non-functional requirements for different modules of the application.

### resources/
Additional project resources and reference materials.

### tests/
Testing documentation including the overall test plan, QA scenarios, and edge cases to be tested.

### tickets/
Development tickets organized by module/area:
- ADM: Administration-related tickets
- COM: Commission-related tickets
- INT: Intervention-related tickets
- IT: IT/Infrastructure-related tickets
