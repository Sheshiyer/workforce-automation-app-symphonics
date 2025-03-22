# QA Scenarios

This document defines test cases covering main workflows, validation, and data integrity checks for the Workforce Automation App.

## Authentication and User Management

### TC-AUTH-001: User Registration

**Objective**: Verify that installers can register using a valid company code.

**Preconditions**:
- Company account exists with a valid installer application code
- User has the mobile application installed

**Steps**:
1. Open the application
2. Tap on "Register" button
3. Enter personal information (name, email, phone)
4. Enter the company code
5. Submit the registration form

**Expected Results**:
- Registration is submitted successfully
- User receives confirmation message
- Back office receives notification for approval
- User cannot log in until approved

### TC-AUTH-002: User Login

**Objective**: Verify that approved users can log in to the application.

**Preconditions**:
- User account exists and is approved
- User has valid credentials

**Steps**:
1. Open the application
2. Enter email and password
3. Tap on "Login" button

**Expected Results**:
- User is authenticated successfully
- User is redirected to the dashboard
- User's interventions are displayed

### TC-AUTH-003: Password Recovery

**Objective**: Verify that users can recover their password.

**Preconditions**:
- User account exists

**Steps**:
1. Open the application
2. Tap on "Forgot Password" link
3. Enter registered email address
4. Submit the form
5. Receive password reset email
6. Follow the link in the email
7. Enter new password
8. Submit the form

**Expected Results**:
- Password reset email is sent
- User can set a new password
- User can log in with the new password

## Company Management

### TC-COMP-001: Create Company Account

**Objective**: Verify that back office administrators can create company accounts.

**Preconditions**:
- Administrator is logged in to the back office portal

**Steps**:
1. Navigate to Company Management
2. Click on "Create Company" button
3. Enter company details (name, SIRET, address, contact information)
4. Upload verification documents (KBIS, RGE certification)
5. Select authorized operations
6. Save the company profile

**Expected Results**:
- Company account is created successfully
- Unique installer application code is generated
- Company appears in the company list
- Company status is set to "active"

### TC-COMP-002: Edit Company Information

**Objective**: Verify that back office administrators can edit company information.

**Preconditions**:
- Company account exists
- Administrator is logged in to the back office portal

**Steps**:
1. Navigate to Company Management
2. Select a company from the list
3. Click on "Edit" button
4. Modify company information
5. Save changes

**Expected Results**:
- Company information is updated successfully
- Changes are reflected in the company details view
- Modification date is updated

## Intervention Management

### TC-INT-001: Create New Intervention

**Objective**: Verify that installers can create new interventions.

**Preconditions**:
- Installer is logged in to the application

**Steps**:
1. Navigate to the dashboard
2. Tap on "+" button
3. Verify that a new intervention form opens
4. Verify that the intervention status changes to "En_cours"

**Expected Results**:
- New intervention form opens
- Form fields are empty
- Intervention status is "En_cours"
- Progress indicator shows step 1 of 3

### TC-INT-002: Enter Customer Information

**Objective**: Verify that installers can enter customer information.

**Preconditions**:
- Installer is logged in to the application
- New intervention has been created

**Steps**:
1. Enter beneficiary information (name, email, phone)
2. Enter address information (street, postal code, city)
3. Enter PDL number
4. Tap on "Next" button

**Expected Results**:
- Information is saved successfully
- PDL is validated with Symphonics API
- User is navigated to step 2 (Operations)
- Progress indicator shows step 2 of 3

### TC-INT-003: Select Operations

**Objective**: Verify that installers can select operations for the intervention.

**Preconditions**:
- Installer is logged in to the application
- Customer information has been entered

**Steps**:
1. Select "Opération Ballon" from the operations list
2. Verify that the operation is added to the intervention
3. Tap on "Next" button

**Expected Results**:
- Operation is added successfully
- User is navigated to step 3 (Photos)
- Progress indicator shows step 3 of 3

### TC-INT-004: Capture Photos

**Objective**: Verify that installers can capture and upload photos.

**Preconditions**:
- Installer is logged in to the application
- Operations have been selected
- Device has camera access and geolocation enabled

**Steps**:
1. Tap on "Add Photo" in the "Before" section
2. Capture a photo using the device camera
3. Verify that the photo is uploaded with geolocation data
4. Repeat for "After" section
5. Repeat for "Application" section
6. Repeat for "Meter" section
7. Tap on "Finalize Intervention" button

**Expected Results**:
- Photos are captured and uploaded successfully
- Photos include geolocation data and timestamps
- Photos are associated with the correct categories
- Intervention can be finalized when all required photos are uploaded

### TC-INT-005: Finalize Intervention

**Objective**: Verify that installers can finalize interventions and generate documents.

**Preconditions**:
- Installer is logged in to the application
- All required information has been entered
- All required photos have been captured

**Steps**:
1. Tap on "Finalize Intervention" button
2. Verify that validation checks are performed
3. Verify that documents are generated (invoice, attestation, contract)
4. Verify that documents are sent for electronic signature
5. Verify that intervention status changes to "invoice_sent"

**Expected Results**:
- Validation checks pass successfully
- Documents are generated correctly
- Documents are sent for electronic signature
- Intervention status changes to "invoice_sent"
- Installer is redirected to the dashboard

### TC-INT-006: Document Signing

**Objective**: Verify that customers can sign documents electronically.

**Preconditions**:
- Intervention has been finalized
- Documents have been sent for signature

**Steps**:
1. Customer receives email with signature request
2. Customer opens the document
3. Customer reviews the document
4. Customer signs the document
5. Verify that the signature is recorded
6. Verify that the intervention status changes to "installation_done"

**Expected Results**:
- Customer can access and view the document
- Customer can sign the document electronically
- Signature is recorded with timestamp and IP address
- Intervention status changes to "installation_done"
- System initiates publication to Symphonics

## Integration Testing

### TC-INT-001: PDL Validation

**Objective**: Verify that PDL numbers are validated correctly with the Symphonics API.

**Preconditions**:
- Installer is logged in to the application
- New intervention has been created
- System has connectivity to Symphonics API

**Steps**:
1. Enter a valid PDL number
2. Tap on "Validate" button or proceed to next step
3. Observe API call to Symphonics
4. Observe response handling

**Expected Results**:
- API call is made to Symphonics PDL validation endpoint
- Valid PDL returns success response
- Invalid PDL displays appropriate error message
- Address information is auto-populated for valid PDL

### TC-INT-002: Publish to Symphonics

**Objective**: Verify that completed interventions are published to the Symphonics platform.

**Preconditions**:
- Intervention has status "installation_done"
- All documents are signed
- System has connectivity to Symphonics API

**Steps**:
1. System automatically initiates publication to Symphonics
2. Observe API call to Symphonics
3. Observe response handling
4. Verify that intervention status changes to "published"

**Expected Results**:
- API call is made to Symphonics CRM endpoint
- All required data is included in the request
- Successful response updates intervention status to "published"
- Error response triggers appropriate error handling and retry mechanism

## Offline Functionality

### TC-OFF-001: Create Intervention Offline

**Objective**: Verify that installers can create interventions while offline.

**Preconditions**:
- Installer is logged in to the application
- Device is in offline mode (no network connectivity)

**Steps**:
1. Navigate to the dashboard
2. Tap on "+" button
3. Create a new intervention
4. Enter customer information
5. Select operations
6. Capture photos
7. Attempt to finalize intervention

**Expected Results**:
- Intervention can be created and data entered while offline
- Photos can be captured and stored locally
- System indicates offline status
- Finalization is queued for when connectivity is restored
- Data is synchronized when connectivity is restored

### TC-OFF-002: Synchronization After Offline Use

**Objective**: Verify that offline data is synchronized correctly when connectivity is restored.

**Preconditions**:
- Installer has created or modified interventions while offline
- Device regains network connectivity

**Steps**:
1. Enable network connectivity
2. Observe synchronization process
3. Verify that offline changes are uploaded to the server
4. Verify that server changes are downloaded to the device

**Expected Results**:
- Synchronization starts automatically when connectivity is restored
- Offline changes are uploaded successfully
- Server changes are downloaded successfully
- Conflicts are resolved according to defined rules
- User is notified of synchronization status

## Performance Testing

### TC-PERF-001: Application Launch Time

**Objective**: Verify that the application launches within acceptable time limits.

**Steps**:
1. Close the application completely
2. Measure the time to launch the application
3. Measure the time until the dashboard is fully loaded and interactive

**Expected Results**:
- Application launches within 3 seconds
- Dashboard is fully loaded and interactive within 5 seconds

### TC-PERF-002: Photo Upload Performance

**Objective**: Verify that photo uploads perform efficiently.

**Preconditions**:
- Installer is logged in to the application
- Intervention is in progress

**Steps**:
1. Capture a high-resolution photo
2. Measure the time for processing and uploading the photo
3. Repeat under various network conditions (4G, 3G, weak signal)

**Expected Results**:
- Photo processing takes less than 2 seconds
- Upload time is reasonable for the network condition
- Progress indicator shows upload status
- System handles slow uploads gracefully

## Security Testing

### TC-SEC-001: Authentication Security

**Objective**: Verify that authentication mechanisms are secure.

**Steps**:
1. Attempt to access protected resources without authentication
2. Attempt to bypass authentication with invalid tokens
3. Attempt to use expired tokens
4. Attempt to brute force passwords

**Expected Results**:
- Unauthenticated access is denied
- Invalid tokens are rejected
- Expired tokens are rejected
- Brute force attempts are blocked after multiple failures

### TC-SEC-002: Data Protection

**Objective**: Verify that sensitive data is protected.

**Steps**:
1. Examine data storage on the device
2. Examine data transmission over the network
3. Examine data storage on the server

**Expected Results**:
- Sensitive data is encrypted on the device
- Data transmission uses HTTPS
- Sensitive data is encrypted on the server
- Personal data is handled according to GDPR requirements

## Edge Cases

### TC-EDGE-001: Handle Large Numbers of Interventions

**Objective**: Verify that the system handles large numbers of interventions efficiently.

**Preconditions**:
- User account has a large number of interventions (100+)

**Steps**:
1. Log in to the application
2. Navigate to the dashboard
3. Observe loading time and performance
4. Filter and search for specific interventions

**Expected Results**:
- Dashboard loads within acceptable time limits
- Pagination or virtualization is implemented for large lists
- Filtering and searching work efficiently
- System remains responsive

### TC-EDGE-002: Handle Poor Network Conditions

**Objective**: Verify that the application functions under poor network conditions.

**Steps**:
1. Set network to slow/unstable connection (e.g., throttled 2G)
2. Perform key workflows (login, create intervention, upload photos)
3. Observe application behavior

**Expected Results**:
- Application remains functional with degraded performance
- Appropriate loading indicators are displayed
- Operations eventually complete or fail gracefully
- User is informed of network issues
- Critical data is not lost

### TC-EDGE-003: Handle Device Constraints

**Objective**: Verify that the application functions on devices with limited resources.

**Steps**:
1. Test on devices with minimal specifications
2. Perform key workflows
3. Monitor resource usage (memory, CPU, battery)

**Expected Results**:
- Application functions on minimum specification devices
- Resource usage is within acceptable limits
- No crashes due to resource constraints
- Battery usage is optimized
