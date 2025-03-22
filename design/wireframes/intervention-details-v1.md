# Intervention Details Screen Wireframe

This wireframe illustrates the intervention details screen for the Workforce Automation App, which displays comprehensive information about a specific intervention and allows the installer to view and update its status.

## Screen Layout

```mermaid
graph TD
    subgraph Mobile Screen
        Header["HEADER: Title & Actions"]
        StatusBar["STATUS INDICATOR"]
        Tabs["NAVIGATION TABS"]
        Content["CONTENT AREA"]
        ActionBar["ACTION BUTTONS"]
    end

    Header --> StatusBar
    StatusBar --> Tabs
    Tabs --> Content
    Content --> ActionBar
```

## Detailed Components

```mermaid
classDiagram
    class InterventionDetailsScreen {
        +Header
        +StatusIndicator
        +NavigationTabs
        +ContentArea
        +ActionButtons
    }
    
    class Header {
        +BackButton
        +Title
        +OptionsMenu
    }
    
    class StatusIndicator {
        +StatusLabel
        +StatusIcon
        +LastUpdated
    }
    
    class NavigationTabs {
        +InfoTab
        +OperationsTab
        +PhotosTab
        +DocumentsTab
    }
    
    class ContentArea {
        +InfoContent
        +OperationsContent
        +PhotosContent
        +DocumentsContent
    }
    
    class InfoContent {
        +CustomerSection
        +AddressSection
        +DetailsSection
        +NotesSection
    }
    
    class OperationsContent {
        +OperationsList
        +AddOperationButton
    }
    
    class PhotosContent {
        +PhotoCategories
        +PhotoGrid
        +AddPhotoButton
    }
    
    class DocumentsContent {
        +DocumentsList
        +GenerateDocumentsButton
    }
    
    class ActionButtons {
        +PrimaryButton
        +SecondaryButton
    }
    
    InterventionDetailsScreen *-- Header
    InterventionDetailsScreen *-- StatusIndicator
    InterventionDetailsScreen *-- NavigationTabs
    InterventionDetailsScreen *-- ContentArea
    InterventionDetailsScreen *-- ActionButtons
    ContentArea *-- InfoContent
    ContentArea *-- OperationsContent
    ContentArea *-- PhotosContent
    ContentArea *-- DocumentsContent
```

## Status Transition Flow

```mermaid
stateDiagram-v2
    [*] --> A_traiter: Intervention Created
    A_traiter --> En_cours: Open Intervention
    En_cours --> quote_sent: Send Quote
    En_cours --> invoice_sent: Skip Quote
    quote_sent --> quote_signed: Customer Signs
    quote_signed --> invoice_sent: Send Invoice
    invoice_sent --> installation_done: Customer Signs
    installation_done --> published: Publish to Symphonics
    
    A_traiter --> canceled: Cancel
    En_cours --> canceled: Cancel
    quote_sent --> canceled: Cancel
    quote_signed --> canceled: Cancel
    invoice_sent --> canceled: Cancel
    
    installation_done --> maintenance_required: Issue Detected
    published --> maintenance_required: Issue Detected
    maintenance_required --> En_cours: Start Maintenance
    maintenance_required --> installation_done: Resolve Issue
    
    published --> [*]: Complete
    canceled --> [*]: End
```

## UI Element Visibility by Status

```mermaid
graph TD
    subgraph Status[Status-Based UI Elements]
        A[A_traiter] -->|Visible Elements| A1[All fields editable]
        A -->|Primary Action| A2[START INTERVENTION]
        
        B[En_cours] -->|Visible Elements| B1[Customer info read-only]
        B -->|Visible Elements| B2[Operations editable]
        B -->|Visible Elements| B3[Photos enabled]
        B -->|Primary Action| B4[SEND QUOTE / SEND INVOICE]
        
        C[quote_sent] -->|Visible Elements| C1[Operations read-only]
        C -->|Visible Elements| C2[Quote status indicator]
        C -->|Primary Action| C3[RESEND QUOTE]
        
        D[quote_signed] -->|Visible Elements| D1[Quote signature indicator]
        D -->|Primary Action| D2[SEND INVOICE]
        
        E[invoice_sent] -->|Visible Elements| E1[Invoice status indicator]
        E -->|Primary Action| E2[MARK COMPLETE]
        
        F[installation_done] -->|Visible Elements| F1[All fields read-only]
        F -->|Primary Action| F2[PUBLISH TO SYMPHONICS]
        
        G[published] -->|Visible Elements| G1[All fields read-only]
        G -->|Visible Elements| G2[Symphonics reference number]
        G -->|Primary Action| G3[VIEW IN SYMPHONICS]
        
        H[maintenance_required] -->|Visible Elements| H1[Maintenance notes field]
        H -->|Primary Action| H2[START MAINTENANCE]
    end
```

## User Interaction Flow

```mermaid
flowchart TD
    A[Open Intervention Details] --> B{Check Status}
    
    B -->|A_traiter| C[Show Editable Form]
    C --> D[User Fills Information]
    D --> E[Validate Fields]
    E -->|Valid| F[Update Status to En_cours]
    E -->|Invalid| G[Show Validation Errors]
    G --> D
    
    B -->|En_cours| H[Show Operations Tab]
    H --> I[User Selects Operations]
    I --> J[User Captures Photos]
    J --> K{All Required Photos?}
    K -->|Yes| L[Enable Document Generation]
    K -->|No| M[Show Missing Photos]
    M --> J
    
    L --> N{Quote Required?}
    N -->|Yes| O[Generate Quote]
    O --> P[Send for Signature]
    P --> Q[Update Status to quote_sent]
    
    N -->|No| R[Generate Invoice]
    R --> S[Send for Signature]
    S --> T[Update Status to invoice_sent]
    
    B -->|quote_sent| U[Show Quote Status]
    U --> V{Quote Signed?}
    V -->|Yes| W[Update Status to quote_signed]
    V -->|No| X[Show Pending Status]
    
    B -->|quote_signed| Y[Enable Invoice Generation]
    Y --> Z[Generate and Send Invoice]
    Z --> AA[Update Status to invoice_sent]
    
    B -->|invoice_sent| AB[Show Invoice Status]
    AB --> AC{Invoice Signed?}
    AC -->|Yes| AD[Update Status to installation_done]
    AC -->|No| AE[Show Pending Status]
    
    B -->|installation_done| AF[Enable Symphonics Publishing]
    AF --> AG[Publish to Symphonics]
    AG --> AH[Update Status to published]
    
    B -->|published| AI[Show Symphonics Reference]
    AI --> AJ[All Actions Complete]
    
    B -->|maintenance_required| AK[Show Maintenance Form]
    AK --> AL[User Performs Maintenance]
    AL --> AM[Update Status to installation_done]
```

## Photo Capture Flow

```mermaid
sequenceDiagram
    participant U as User
    participant A as App
    participant C as Camera
    participant G as Geolocation
    participant S as Storage
    
    U->>A: Select Photo Type
    A->>C: Request Camera Access
    C-->>A: Camera Ready
    A->>G: Request Location
    G-->>A: Location Data
    
    U->>C: Capture Photo
    C-->>A: Photo Captured
    A->>A: Validate Geolocation
    
    alt Geolocation Available
        A->>A: Tag Photo with Location
        A->>S: Save Photo
        S-->>A: Photo Saved
        A-->>U: Show Photo Thumbnail
    else Geolocation Unavailable
        A-->>U: Show Error Message
        A->>G: Request Permission Again
        G-->>A: Location Data
        A->>A: Retry Photo Capture
    end
    
    A->>A: Update Photo Count
    A->>A: Check Required Photos
    
    alt All Required Photos Captured
        A-->>U: Enable Next Step
    else Missing Required Photos
        A-->>U: Show Missing Photo Indicators
    end
```

## Document Generation Flow

```mermaid
sequenceDiagram
    participant U as User
    participant A as App
    participant T as Templates
    participant D as Document Service
    participant E as Email Service
    
    U->>A: Request Document Generation
    A->>A: Validate Required Data
    
    alt All Required Data Available
        A->>T: Fetch Document Templates
        T-->>A: Templates Retrieved
        A->>A: Merge Data with Templates
        A->>D: Generate Documents
        D-->>A: Documents Generated
        A-->>U: Show Generated Documents
        
        U->>A: Request Send for Signature
        A->>E: Send Documents
        E-->>A: Delivery Confirmation
        A->>A: Update Document Status
        A-->>U: Show Sent Status
    else Missing Required Data
        A-->>U: Show Missing Data Error
        A-->>U: Highlight Required Fields
    end
```

## Error Handling Flow

```mermaid
flowchart TD
    A[User Action] --> B{Validation Check}
    
    B -->|Valid| C[Process Action]
    C --> D{API Call Required?}
    
    D -->|Yes| E{Network Available?}
    E -->|Yes| F[Make API Call]
    F --> G{API Success?}
    
    G -->|Yes| H[Update UI with Success]
    G -->|No| I[Show API Error]
    I --> J[Log Error Details]
    J --> K[Show Retry Option]
    
    E -->|No| L[Store Action Offline]
    L --> M[Show Offline Indicator]
    M --> N[Queue for Sync]
    
    B -->|Invalid| O[Show Validation Errors]
    O --> P[Highlight Error Fields]
    P --> Q[Focus First Error]
    
    D -->|No| R[Process Locally]
    R --> S[Update UI]
```

## UI Mockup - Info Tab

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph InterventionDetailsScreen["Intervention Details Screen"]
        subgraph Header["Header"]
            BackButton["←"]
            Title["Intervention Details"]
            OptionsButton["⋮"]
        end
        
        subgraph StatusBar["Status Bar"]
            StatusLabel["Status: En_cours"]
            LastUpdated["Updated: Today, 14:30"]
        end
        
        subgraph Tabs["Navigation Tabs"]
            InfoTab["Info"]
            OperationsTab["Operations"]
            PhotosTab["Photos"]
            DocumentsTab["Documents"]
        end
        
        subgraph InfoContent["Information Content"]
            subgraph CustomerSection["Customer Information"]
                NameLabel["Name:"]
                NameValue["Thomas Martin"]
                EmailLabel["Email:"]
                EmailValue["thomas.martin@example.com"]
                PhoneLabel["Phone:"]
                PhoneValue["+33612345678"]
            end
            
            subgraph AddressSection["Address Information"]
                AddressLabel["Address:"]
                AddressValue["45 Rue des Fleurs"]
                CityLabel["City:"]
                CityValue["Paris"]
                PostalLabel["Postal Code:"]
                PostalValue["75015"]
            end
            
            subgraph DetailsSection["Intervention Details"]
                PDLLabel["PDL:"]
                PDLValue["12345678901234"]
                DateLabel["Scheduled:"]
                DateValue["March 25, 2025 - 14:30"]
                CreatedLabel["Created:"]
                CreatedValue["March 20, 2025"]
            end
            
            subgraph NotesSection["Notes"]
                NotesLabel["Notes:"]
                NotesValue["Customer requested installation before noon. Access code for building: 1234."]
                EditNotesButton["Edit Notes"]
            end
        end
        
        subgraph ActionBar["Action Buttons"]
            PrimaryButton["[ NEXT STEP ]"]
            SecondaryButton["[ CANCEL ]"]
        end
    end
    
    BackButton --- Title
    Title --- OptionsButton
    
    StatusLabel --- LastUpdated
    
    InfoTab --- OperationsTab
    OperationsTab --- PhotosTab
    PhotosTab --- DocumentsTab
    
    NameLabel --- NameValue
    NameValue --- EmailLabel
    EmailLabel --- EmailValue
    EmailValue --- PhoneLabel
    PhoneLabel --- PhoneValue
    
    AddressLabel --- AddressValue
    AddressValue --- CityLabel
    CityLabel --- CityValue
    CityValue --- PostalLabel
    PostalLabel --- PostalValue
    
    PDLLabel --- PDLValue
    PDLValue --- DateLabel
    DateLabel --- DateValue
    DateValue --- CreatedLabel
    CreatedLabel --- CreatedValue
    
    NotesLabel --- NotesValue
    NotesValue --- EditNotesButton
    
    PrimaryButton --- SecondaryButton
```

## UI Mockup - Operations Tab

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph OperationsContent["Operations Content"]
        OperationsTitle["Selected Operations"]
        
        subgraph Operation1["Operation 1"]
            Op1Code["BAR-TH-173"]
            Op1Name["Régulateur de ballon d'eau chaude"]
            Op1Price["Price: 150.00 €"]
            Op1Actions["[ EDIT ] [ REMOVE ]"]
        end
        
        subgraph AddOperation["Add Operation"]
            AddOperationButton["[ + ADD OPERATION ]"]
        end
        
        subgraph PriceSection["Price Summary"]
            SubtotalLabel["Subtotal:"]
            SubtotalValue["150.00 €"]
            TaxLabel["Tax (20%):"]
            TaxValue["30.00 €"]
            TotalLabel["Total:"]
            TotalValue["180.00 €"]
        end
    end
    
    OperationsTitle --- Operation1
    Operation1 --- AddOperation
    AddOperation --- PriceSection
    
    Op1Code --- Op1Name
    Op1Name --- Op1Price
    Op1Price --- Op1Actions
    
    SubtotalLabel --- SubtotalValue
    SubtotalValue --- TaxLabel
    TaxLabel --- TaxValue
    TaxValue --- TotalLabel
    TotalLabel --- TotalValue
```

## UI Mockup - Photos Tab

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph PhotosContent["Photos Content"]
        PhotosTitle["Required Photos"]
        
        subgraph BeforePhotos["Before Photos"]
            BeforeTitle["Before Installation (1/2)"]
            BeforePhoto1["📷"]
            BeforeAddButton["[ + ADD PHOTO ]"]
        end
        
        subgraph AfterPhotos["After Photos"]
            AfterTitle["After Installation (0/2)"]
            AfterAddButton["[ + ADD PHOTO ]"]
        end
        
        subgraph MeterPhotos["Meter Photos"]
            MeterTitle["Meter (1/1)"]
            MeterPhoto1["📷"]
        end
        
        subgraph AppPhotos["Application Photos"]
            AppTitle["Application (0/1)"]
            AppAddButton["[ + ADD PHOTO ]"]
        end
    end
    
    PhotosTitle --- BeforePhotos
    BeforePhotos --- AfterPhotos
    AfterPhotos --- MeterPhotos
    MeterPhotos --- AppPhotos
    
    BeforeTitle --- BeforePhoto1
    BeforePhoto1 --- BeforeAddButton
    
    AfterTitle --- AfterAddButton
    
    MeterTitle --- MeterPhoto1
    
    AppTitle --- AppAddButton
```

## UI Mockup - Documents Tab

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph DocumentsContent["Documents Content"]
        DocumentsTitle["Documents"]
        
        subgraph NoDocuments["No Documents"]
            NoDocsMessage["No documents have been generated yet."]
            GenerateButton["[ GENERATE DOCUMENTS ]"]
            GenerateNote["Documents will be generated when all required information and photos are provided."]
        end
        
        subgraph DocumentsList["Documents List (Alternative View)"]
            subgraph Document1["Invoice"]
                Doc1Icon["📄"]
                Doc1Name["Invoice - Thomas Martin - 25/03/2025"]
                Doc1Status["Status: Sent for signature"]
                Doc1Date["Generated: March 25, 2025"]
                Doc1Actions["[ VIEW ] [ SEND ]"]
            end
            
            subgraph Document2["Attestation"]
                Doc2Icon["📄"]
                Doc2Name["Attestation d'Honneur - Thomas Martin"]
                Doc2Status["Status: Draft"]
                Doc2Date["Generated: March 25, 2025"]
                Doc2Actions["[ VIEW ] [ EDIT ]"]
            end
        end
    end
    
    DocumentsTitle --- NoDocuments
    
    NoDocsMessage --- GenerateButton
    GenerateButton --- GenerateNote
    
    %% This is an alternative view that would replace NoDocuments when documents exist
    %% DocumentsTitle --- DocumentsList
    %% Document1 --- Document2
    
    %% Doc1Icon --- Doc1Name
    %% Doc1Name --- Doc1Status
    %% Doc1Status --- Doc1Date
    %% Doc1Date --- Doc1Actions
    
    %% Doc2Icon --- Doc2Name
    %% Doc2Name --- Doc2Status
    %% Doc2Status --- Doc2Date
    %% Doc2Date --- Doc2Actions
```

## Photo Capture Modal

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph PhotoCaptureModal["Photo Capture Modal"]
        subgraph ModalHeader["Modal Header"]
            ModalTitle["Capture Before Photo"]
            CloseButton["×"]
        end
        
        subgraph CameraView["Camera View"]
            CameraPreview["[Camera Preview]"]
            LocationIndicator["📍 Location: 48.8566, 2.3522"]
            CaptureButton["📷"]
        end
        
        subgraph Instructions["Instructions"]
            InstructionText["Please take a clear photo of the equipment before installation. Ensure the entire unit is visible."]
        end
        
        subgraph Controls["Controls"]
            FlashButton["Flash: Auto"]
            GridButton["Grid: On"]
        end
    end
    
    ModalTitle --- CloseButton
    CameraPreview --- LocationIndicator
    LocationIndicator --- CaptureButton
    InstructionText --- Controls
    FlashButton --- GridButton
```

## Document Preview Modal

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph DocumentPreviewModal["Document Preview Modal"]
        subgraph ModalHeader["Modal Header"]
            ModalTitle["Invoice Preview"]
            CloseButton["×"]
        end
        
        subgraph DocumentView["Document View"]
            DocumentPreview["[PDF Preview]"]
            PageControls["Page 1 of 2  ◀ ▶"]
        end
        
        subgraph Actions["Actions"]
            SendButton["[ SEND FOR SIGNATURE ]"]
            DownloadButton["[ DOWNLOAD ]"]
            EditButton["[ EDIT ]"]
        end
    end
    
    ModalTitle --- CloseButton
    DocumentPreview --- PageControls
    PageControls --- Actions
    SendButton --- DownloadButton
    DownloadButton --- EditButton
```

## Operation Selection Modal

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph OperationSelectionModal["Operation Selection Modal"]
        subgraph ModalHeader["Modal Header"]
            ModalTitle["Select Operation"]
            CloseButton["×"]
        end
        
        subgraph SearchSection["Search Section"]
            SearchInput["🔍 Search operations..."]
            FilterButton["Filter ▼"]
        end
        
        subgraph OperationsList["Operations List"]
            subgraph Operation1["Operation 1"]
                Op1Code["BAR-TH-173"]
                Op1Name["Régulateur de ballon d'eau chaude"]
                Op1Description["Installation d'un régulateur sur un ballon d'eau chaude électrique existant."]
                Op1Price["Price: 150.00 €"]
            end
            
            subgraph Operation2["Operation 2"]
                Op2Code["BAR-TH-171"]
                Op2Name["Thermostat programmable"]
                Op2Description["Installation d'un thermostat programmable pour système de chauffage."]
                Op2Price["Price: 120.00 €"]
            end
        end
        
        subgraph Actions["Actions"]
            SelectButton["[ SELECT ]"]
            CancelButton["[ CANCEL ]"]
        end
    end
    
    ModalTitle --- CloseButton
    SearchInput --- FilterButton
    Operation1 --- Operation2
    SelectButton --- CancelButton
```

## Notes Edit Modal

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#f4f4f4', 'primaryTextColor': '#333', 'primaryBorderColor': '#ddd', 'lineColor': '#a0a0a0', 'secondaryColor': '#006699', 'tertiaryColor': '#fff'}}}%%
graph TD
    subgraph NotesEditModal["Notes Edit Modal"]
        subgraph ModalHeader["Modal Header"]
            ModalTitle["Edit Notes"]
            CloseButton["×"]
        end
        
        subgraph NotesForm["Notes Form"]
            NotesTextarea["Customer requested installation before noon. Access code for building: 1234."]
        end
        
        subgraph Actions["Actions"]
            SaveButton["[ SAVE ]"]
            CancelButton["[ CANCEL ]"]
        end
    end
    
    ModalTitle --- CloseButton
    NotesTextarea --- Actions
    SaveButton --- CancelButton
```

## Specifications

### Layout Specifications
- **Screen Size**: Optimized for mobile (375px width)
- **Header Height**: 60px
- **Status Bar Height**: 40px
- **Tabs Height**: 50px
- **Action Bar Height**: 60px
- **Content Area**: Remaining screen height, scrollable

### Component Specifications

#### Header
- **Back Button**: Left-aligned arrow icon (24px)
- **Title**: "Intervention Details" (18px Roboto Medium)
- **Options Button**: Three-dot menu icon (24px)

#### Status Indicator
- **Status Label**: "Status: [status]" with status-specific color (16px Roboto Medium)
  - A_traiter: Gray (#808080)
  - En_cours: Blue (#0066CC)
  - quote_sent: Orange (#FF9900)
  - quote_signed: Light Green (#66CC66)
  - invoice_sent: Orange (#FF9900)
  - installation_done: Green (#00CC00)
  - published: Purple (#9966CC)
  - canceled: Red (#CC0000)
  - maintenance_required: Red (#CC0000)
- **Last Updated**: "Updated: [date/time]" (12px Roboto Regular)

#### Navigation Tabs
- **Container**: Full width, horizontal tabs
- **Tabs**: Equal width, horizontally distributed
  - Active tab: Primary color underline, bold text
  - Inactive tab: No underline, regular text
- **Tab Options**:
  - Info
  - Operations
  - Photos
  - Documents

#### Content Area - Info Tab

##### Customer Information Section
- **Section Title**: "Customer Information" (16px Roboto Medium)
- **Fields**: Name, Email, Phone
- **Labels**: Right-aligned, gray text (14px Roboto Regular)
- **Values**: Left-aligned, black text (14px Roboto Regular)

##### Address Information Section
- **Section Title**: "Address Information" (16px Roboto Medium)
- **Fields**: Address, City, Postal Code
- **Labels**: Right-aligned, gray text (14px Roboto Regular)
- **Values**: Left-aligned, black text (14px Roboto Regular)

##### Intervention Details Section
- **Section Title**: "Intervention Details" (16px Roboto Medium)
- **Fields**: PDL, Scheduled Date, Created Date
- **Labels**: Right-aligned, gray text (14px Roboto Regular)
- **Values**: Left-aligned, black text (14px Roboto Regular)

##### Notes Section
- **Section Title**: "Notes" (16px Roboto Medium)
- **Notes Text**: Multi-line text (14px Roboto Regular)
- **Edit Button**: "Edit Notes" text button (14px Roboto Medium)

#### Content Area - Operations Tab

##### Operations List
- **Section Title**: "Selected Operations" (16px Roboto Medium)
- **Operation Cards**: Full width, rounded corners (8px), white background, subtle shadow

##### Operation Card
- **Code**: CEE operation code (14px Roboto Medium)
- **Name**: Operation name (16px Roboto Regular)
- **Price**: Price with currency (14px Roboto Regular)
- **Action Buttons**: "EDIT" and "REMOVE" text buttons (14px Roboto Medium)

##### Add Operation Button
- **Text**: "+ ADD OPERATION" (16px Roboto Medium)
- **Full width**, 44px height, rounded corners (4px)
- **Border**: Dashed border, primary color
- **Background**: White

##### Price Summary
- **Section Title**: "Price Summary" (16px Roboto Medium)
- **Fields**: Subtotal, Tax, Total
- **Labels**: Left-aligned (14px Roboto Regular)
- **Values**: Right-aligned, bold for total (14px Roboto Medium)

#### Content Area - Photos Tab

##### Photo Categories
- **Section Title**: "Required Photos" (16px Roboto Medium)
- **Category Cards**: Full width, rounded corners (8px), white background, subtle shadow

##### Photo Category Card
- **Title**: Category name with count (16px Roboto Medium)
- **Photos**: Horizontal scrollable area with thumbnails
- **Add Button**: "+ ADD PHOTO" text button (14px Roboto Medium)
- **Thumbnail**: Square image preview (80x80px) with tap to enlarge

#### Content Area - Documents Tab

##### Documents List
- **Section Title**: "Documents" (16px Roboto Medium)
- **Document Cards**: Full width, rounded corners (8px), white background, subtle shadow

##### Document Card
- **Icon**: Document type icon (24px)
- **Name**: Document name (16px Roboto Regular)
- **Status**: Status with color-coding (14px Roboto Regular)
- **Date**: Generation date (12px Roboto Regular)
- **Action Buttons**: "VIEW" and "SEND"/"EDIT" text buttons (14px Roboto Medium)

##### No Documents State
- **Message**: "No documents have been generated yet." (16px Roboto Regular)
- **Generate Button**: "GENERATE DOCUMENTS" button (16px Roboto Medium)
- **Note**: Explanatory text (12px Roboto Italic)

#### Action Buttons
- **Primary Button**: "NEXT STEP" (16px Roboto Medium, uppercase)
- **Secondary Button**: "CANCEL" (16px Roboto Medium, uppercase)
- **Full width**, 48px height, rounded corners (4px)
- **Primary**: Primary color background (#006699), white text
- **Secondary**: White background, primary color border and text

### Behavior Specifications

1. **Header**:
   - Back button: Returns to previous screen
   - Options menu: Opens dropdown with additional actions (edit, delete, etc.)

2. **Status Indicator**:
   - Status label: Color-coded based on status
   - Last updated: Shows relative time (e.g., "Today, 14:30") or date

3. **Navigation Tabs**:
   - Tap to switch between tabs
   - Content area updates to show selected tab
   - Active tab is highlighted
   - Swipe gestures can be used to navigate between tabs

4. **Info Tab**:
   - Read-only display of intervention details
   - Edit notes button opens a text input modal

5. **Operations Tab**:
   - Displays list of selected operations
   - Add operation button opens operation selection screen
   - Edit button opens operation details for editing
   - Remove button prompts for confirmation before removing
   - Price summary updates automatically

6. **Photos Tab**:
   - Organized by required photo categories
   - Shows progress (e.g., "1/2 photos")
   - Add photo button opens camera or photo library
   - Tap on photo to view full-screen
   - Swipe to delete photo (with confirmation)

7. **Documents Tab**:
   - Shows list of generated documents
   - Generate documents button is disabled until all required information is provided
   - View button opens document preview
   - Send button triggers email/SMS delivery
   - Edit button allows modification of document details

8. **Action Buttons**:
   - Primary button action changes based on current status:
     - A_traiter → En_cours: "START INTERVENTION"
     - En_cours → quote_sent: "SEND QUOTE" (if applicable)
     - En_cours → invoice_sent: "SEND INVOICE" (if skipping quote)
     - quote_signed → invoice_sent: "SEND INVOICE"
     - invoice_sent → installation_done: "MARK COMPLETE"
     - installation_done → published: "PUBLISH TO SYMPHONICS"
   - Secondary button: "CANCEL" with confirmation dialog
   - Buttons are disabled during processing operations

### Status-Based UI Adaptations

The interface adapts based on the intervention status:

1. **A_traiter**:
   - All fields are editable
   - Primary action: "START INTERVENTION"
   - No document generation available
   - Photo capture disabled

2. **En_cours**:
   - Customer and address information become read-only
   - Operations can be added/edited
   - Photos can be captured
   - Primary action: "SEND QUOTE" or "SEND INVOICE"
   - Document generation available when all required data is present

3. **quote_sent**:
   - Operations become read-only
   - Documents tab shows quote status
   - Primary action: "RESEND QUOTE"
   - Quote tracking information displayed

4. **quote_signed**:
   - Quote signature details displayed
   - Primary action: "SEND INVOICE"
   - Invoice generation enabled

5. **invoice_sent**:
   - Documents tab shows invoice and signature status
   - Primary action: "MARK COMPLETE"
   - Signature tracking information displayed

6. **installation_done**:
   - All fields become read-only
   - Primary action: "PUBLISH TO SYMPHONICS"
   - Symphonics API validation status displayed

7. **published**:
   - All fields are read-only
   - Shows Symphonics reference number
   - Primary action: "VIEW IN SYMPHONICS"
   - Symphonics sync details displayed

8. **maintenance_required**:
   - Maintenance notes field is editable
   - Primary action: "START MAINTENANCE"
   - Issue details and history displayed

### Responsive Behavior

- On larger screens (tablet):
  - Two-column layout for information sections
  - Larger photo thumbnails (120x120px)
  - Side-by-side display of documents
  - Persistent tabs (no swipe navigation)
  - Floating action buttons

- On desktop:
  - Three-column layout
  - Persistent navigation sidebar
  - Document preview panel
  - Split-view for before/after photos
  - Advanced filtering and sorting options

### Offline Capabilities

1. **Data Storage**:
   - Intervention details cached for offline access
   - Photos stored locally until sync is possible
   - Document templates available offline

2. **Sync Indicators**:
   - Sync status icon in header
   - Last sync timestamp displayed
   - Pending changes count indicator

3. **Background Sync**:
   - Automatic sync when connectivity is restored
   - Priority-based sync (documents first, then photos)
   - Conflict resolution with user prompts

### Accessibility Considerations

1. **Color Contrast**:
   - All text meets WCAG AA standards for contrast
   - Status colors have text alternatives
   - High contrast mode available

2. **Touch Targets**:
   - All interactive elements are at least 44x44px
   - Adequate spacing between touch targets
   - Haptic feedback for important actions

3. **Screen Readers**:
   - All elements have appropriate labels
   - Status changes are announced
   - Tab structure is properly defined
   - Custom actions for photo capture

4. **Keyboard Navigation**:
   - Logical tab order
   - Focus indicators for all interactive elements
   - Keyboard shortcuts for common actions

### Performance Optimizations

1. **Image Handling**:
   - Progressive image loading
   - Thumbnails generated on device
   - Lazy loading for off-screen content
   - Optimized image formats (WebP with JPEG fallback)

2. **UI Responsiveness**:
   - Skeleton screens during loading
   - Background processing for document generation
   - Virtualized lists for large collections
   - Debounced input handlers

3. **Network Efficiency**:
   - Incremental sync of changed data only
   - Compressed data transfer
   - Batch operations for multiple changes
   - Prioritized loading of critical content

## Implementation Notes

1. Use skeleton screens during loading for better perceived performance
2. Implement efficient tab switching without losing scroll position
3. Cache intervention details for offline access
4. Use appropriate animations for transitions and interactions
5. Ensure all interactive elements have appropriate feedback states
6. Implement proper error handling for failed data loading or updates
7. Use draft saving mechanism for partial updates
8. Implement comprehensive validation before status transitions
9. Optimize camera integration for fast photo capture
10. Ensure document generation works with limited connectivity
11. Implement secure storage for sensitive customer information
12. Use appropriate caching strategies for API responses
13. Optimize for variable network conditions (2G/3G/4G/WiFi)
14. Implement analytics for user interaction patterns
15. Use feature flags for gradual rollout of new capabilities
