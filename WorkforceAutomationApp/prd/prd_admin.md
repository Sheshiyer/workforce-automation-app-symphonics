# Administration Module Requirements

This document details the requirements for the administration functionality of the Workforce Automation App, focusing on company account creation, user management workflows, permissions, and statuses.

## Company Account Management

### ADM001: Company Account Creation (MVP)

**Description**: Creation of installer company accounts by Symphonics back office staff.

**Requirements**:
1. Company accounts cannot be created through self-registration and must be verified by Symphonics back office teams.
2. Back office staff must verify company information (KBIS, RGE certification) before account creation.
3. An administration page must allow entry of company identification information.
4. A unique installer application code (`installer_application_code`) must be generated and recorded with creation date (`installer_application_code_creation_date`).
5. The system must allow specification of authorized operations (`operation_authorized`) for the company.
6. The unique code must be transmitted to the installer company for use during user registration.
7. User creation should only be possible after company information has been entered.
8. Admin UI should be simple, based on forms and lists.

## User Management

### ADM002: User Self-Registration (MVP)

**Description**: Process for installers to create their own user accounts.

**Requirements**:
1. From the application URL, visitors must have access to:
   - Login functionality with username and password
   - Password recovery via email
   - New user registration form
2. New user registration must require the company's unique installer application code.
3. After submission, the form must be saved and sent to bo@symphonics.fr for admin validation.
4. The system must validate that the installer code exists and is valid.

### ADM003: User Status Verification (Post-MVP)

**Description**: Verification of user status during login.

**Requirements**:
1. The system must verify that the user status is "active" before allowing login.
2. Inactive users should be prevented from accessing the system.
3. Appropriate error messages should be displayed for inactive accounts.

### ADM004: User Management Panel (MVP)

**Description**: Administrative interface for managing users.

**Requirements**:
1. Back office staff must be able to:
   - Create new users directly
   - View a list of all users
   - Validate access for new users by updating their status
   - Deactivate users by changing their status and setting an end date
2. The interface should provide filtering and sorting capabilities for efficient user management.
3. User management should be accessible from the same interface as company management.

### ADM005: Operations and Products Management (Post-MVP)

**Description**: Administration of available operations and associated products.

**Requirements**:
1. Back office staff must be able to:
   - Create, modify, and suspend operations (CEE operations like BAR-TH-173, BAR-EQ-115)
   - Define which operations are available for selection during intervention creation
   - Manage products associated with operations (e.g., Thaléos thermostat at €23 for BAR-TH-173)
   - Define product descriptions and prices for inclusion on quotes and invoices
2. The system must maintain relationships between operations and eligible products.

### ADM006: Intervention List Administration (MVP)

**Description**: Management of intervention lists and assignment to users.

**Requirements**:
1. Back office staff must have access to a page for creating and managing intervention lists.
2. For MVP, back office staff should be able to manually enter each job in the admin view.
3. The list view must allow:
   - Creation and modification of interventions (deletion should not be permitted)
   - Modification of status, type, and assigned user for each job
4. The interface should provide filtering and sorting capabilities.

### ADM007: Automated Intervention List Loading (MVP)

**Description**: Capability to load intervention lists from CSV files.

**Requirements**:
1. Back office staff must be able to upload CSV files containing target campaign lines.
2. The system must parse the CSV and create corresponding intervention records.
3. Validation rules must be applied to ensure data integrity.
4. Error handling must provide clear feedback on any issues with the uploaded data.

## Security and Access Control

**General Requirements**:
1. All administrative functions must be restricted to authorized back office staff.
2. The system must maintain audit logs of all administrative actions.
3. Password policies must enforce strong security (minimum length, complexity, etc.).
4. Session management must include appropriate timeouts and security measures.

## Future Enhancements

1. Role-based access control within the back office team
2. Enhanced reporting on administrative activities
3. Bulk operations for user and company management
4. Integration with corporate directory services
