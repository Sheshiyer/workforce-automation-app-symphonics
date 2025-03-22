# User Flows

This document illustrates the key user journeys and interactions within the Workforce Automation App.

## User Types and Journeys

The application supports three primary user types, each with distinct journeys:

1. **Installer**: Field personnel performing equipment installation and service subscription
2. **Back Office**: Symphonics administrative staff managing companies, users, and interventions
3. **IT**: Technical staff managing system configuration and integration

## Installer Journeys

### 1. User Registration and Onboarding

```mermaid
graph TD
    A[Download App] --> B[Open Registration Form]
    B --> C[Enter Personal Information]
    C --> D[Enter Company Code]
    D --> E[Submit Registration]
    E --> F[Wait for Back Office Approval]
    F --> G[Receive Approval Notification]
    G --> H[Set Password]
    H --> I[Login to App]
    I --> J[View Dashboard]
```

### 2. Intervention Creation and Completion

```mermaid
graph TD
    A[Login to App] --> B[View Dashboard]
    B --> C[Create New Intervention]
    C --> D[Enter Customer Information]
    D --> E[Validate PDL]
    E --> F[Select Operations]
    F --> G[Take Before Photos]
    G --> H[Perform Physical Installation]
    H --> I[Take After Photos]
    I --> J[Take Application Photos]
    J --> K[Take Meter Photos]
    K --> L[Finalize Intervention]
    L --> M[Generate Documents]
    M --> N[Send for Electronic Signature]
    N --> O[Receive Signature Confirmation]
    O --> P[View Completed Intervention]
```

### 3. Intervention Management

```mermaid
graph TD
    A[Login to App] --> B[View Dashboard]
    B --> C[Filter by Status]
    C --> D[Select Intervention]
    D --> E[View Intervention Details]
    E --> F[Update Status]
    F --> G[Add Notes]
    G --> H[Upload Additional Photos]
    H --> I[Save Changes]
```

## Back Office Journeys

### 1. Company Account Creation

```mermaid
graph TD
    A[Login to Admin Portal] --> B[Access Company Management]
    B --> C[Create New Company]
    C --> D[Enter Company Information]
    D --> E[Verify Documentation]
    E --> F[Set Authorized Operations]
    F --> G[Generate Company Code]
    G --> H[Save Company Profile]
    H --> I[Send Code to Company]
```

### 2. User Management

```mermaid
graph TD
    A[Login to Admin Portal] --> B[Access User Management]
    B --> C[View User Requests]
    C --> D[Review User Information]
    D --> E[Verify Company Association]
    E --> F[Approve or Reject User]
    F --> G[Set User Permissions]
    G --> H[Save User Profile]
    H --> I[Send Notification to User]
```

### 3. Intervention Administration

```mermaid
graph TD
    A[Login to Admin Portal] --> B[Access Intervention Management]
    B --> C[Create Intervention List]
    C --> D[Upload CSV or Manual Entry]
    D --> E[Assign to Installers]
    E --> F[Set Priorities]
    F --> G[Save Intervention List]
    G --> H[Monitor Progress]
    H --> I[Generate Reports]
```

## IT Journeys

### 1. System Integration

```mermaid
graph TD
    A[Login to Admin Portal] --> B[Access Integration Settings]
    B --> C[Configure API Endpoints]
    C --> D[Set Authentication Parameters]
    D --> E[Define Data Mapping]
    E --> F[Test Integration]
    F --> G[Monitor Data Flow]
    G --> H[Generate Integration Reports]
```

### 2. Document Template Management

```mermaid
graph TD
    A[Login to Admin Portal] --> B[Access Template Management]
    B --> C[Create/Edit Document Templates]
    C --> D[Define Merge Fields]
    D --> E[Associate with Operations]
    E --> F[Test Document Generation]
    F --> G[Publish Templates]
```

## Key Interaction Flows

### 1. PDL Validation Flow

```mermaid
graph TD
    A[Enter Address] --> B[System Calls PDL Lookup API]
    B --> C{PDL Found?}
    C -->|Yes| D[Auto-populate PDL Field]
    C -->|No| E[Manual PDL Entry]
    D --> F[Validate PDL Format]
    E --> F
    F --> G{Valid PDL?}
    G -->|Yes| H[Proceed to Next Step]
    G -->|No| I[Display Error Message]
    I --> J[Allow Correction]
```

### 2. Photo Capture Flow

```mermaid
graph TD
    A[Select Photo Type] --> B[Access Device Camera]
    B --> C[Capture Photo]
    C --> D[Verify Geolocation Data]
    D --> E{Geolocation Available?}
    E -->|Yes| F[Tag Photo with Location]
    E -->|No| G[Display Error Message]
    F --> H[Save Photo to Intervention]
    G --> I[Request Location Permission]
    I --> B
```

### 3. Document Generation and Signature Flow

```mermaid
graph TD
    A[Finalize Intervention] --> B[System Validates All Required Data]
    B --> C{Validation Successful?}
    C -->|Yes| D[Generate Documents]
    C -->|No| E[Display Error Messages]
    D --> F[Send Documents for Signature]
    F --> G[Customer Receives Email]
    G --> H[Customer Reviews Documents]
    H --> I[Customer Signs Documents]
    I --> J[System Receives Signature Confirmation]
    J --> K[Update Intervention Status]
    K --> L[Trigger Symphonics API Integration]
```

## Error Handling Flows

### 1. Network Connectivity Issues

```mermaid
graph TD
    A[User Action] --> B{Network Available?}
    B -->|Yes| C[Process Action Online]
    B -->|No| D[Store Action Locally]
    D --> E[Display Offline Indicator]
    E --> F[Monitor Connectivity]
    F --> G{Network Restored?}
    G -->|Yes| H[Sync Pending Actions]
    G -->|No| I[Continue Offline Operation]
    H --> J[Update UI with Sync Status]
```

### 2. Validation Error Recovery

```mermaid
graph TD
    A[Submit Form] --> B[Validate All Fields]
    B --> C{Validation Errors?}
    C -->|Yes| D[Highlight Error Fields]
    C -->|No| E[Process Submission]
    D --> F[Display Error Messages]
    F --> G[Focus First Error Field]
    G --> H[User Corrects Errors]
    H --> A
```

## Status Transition Flows

```mermaid
graph TD
    A[A_traiter] -->|Open Form| B[En_cours]
    B -->|Send Quote| C[quote_sent]
    C -->|Quote Signed| D[quote_signed]
    B -->|Skip Quote| E[Finalize]
    D -->|Finalize| E
    E -->|Send Invoice| F[invoice_sent]
    F -->|Invoice Signed| G[installation_done]
    G -->|Publish to Symphonics| H[published]
```

These user flows illustrate the primary interactions and journeys within the Workforce Automation App. They serve as a foundation for both UI design and technical implementation, ensuring a consistent and efficient user experience across all aspects of the application.
