# User Entity

This document defines the schema for users in the Workforce Automation App, including roles, statuses, and authentication details.

## Schema Definition

```json
{
  "id": {
    "type": "string",
    "description": "Unique identifier for the user",
    "required": true,
    "example": "usr_12345678"
  },
  "company_id": {
    "type": "string",
    "description": "Reference to the company the user belongs to",
    "required": true,
    "example": "cmp_87654321"
  },
  "first_name": {
    "type": "string",
    "description": "User's first name",
    "required": true,
    "example": "Jean"
  },
  "last_name": {
    "type": "string",
    "description": "User's last name",
    "required": true,
    "example": "Dupont"
  },
  "email": {
    "type": "string",
    "description": "User's email address (used for login)",
    "required": true,
    "format": "email",
    "example": "jean.dupont@example.com"
  },
  "phone": {
    "type": "string",
    "description": "User's phone number",
    "required": true,
    "format": "phone",
    "example": "+33612345678"
  },
  "password_hash": {
    "type": "string",
    "description": "Hashed password for authentication",
    "required": true,
    "example": "$2a$10$dRFLSkYedQ1Iv4uuUJVm1OwJwFh5NhpzQYwY.f1/fvJ8bh/2uBOi2"
  },
  "role": {
    "type": "string",
    "description": "User's role in the system",
    "required": true,
    "enum": ["installer", "back_office", "it"],
    "example": "installer"
  },
  "status": {
    "type": "string",
    "description": "Current status of the user account",
    "required": true,
    "enum": ["pending", "active", "inactive", "suspended"],
    "example": "active"
  },
  "creation_date": {
    "type": "date",
    "description": "Date when the user account was created",
    "required": true,
    "example": "2025-03-15T10:30:00Z"
  },
  "modification_date": {
    "type": "date",
    "description": "Date when the user account was last modified",
    "required": true,
    "example": "2025-03-20T14:45:00Z"
  },
  "end_date": {
    "type": "date",
    "description": "Date when the user account was deactivated (if applicable)",
    "required": false,
    "example": "2026-03-15T00:00:00Z"
  },
  "last_login": {
    "type": "date",
    "description": "Date of the user's last successful login",
    "required": false,
    "example": "2025-03-22T08:15:00Z"
  },
  "preferences": {
    "type": "object",
    "description": "User preferences and settings",
    "required": false,
    "properties": {
      "language": {
        "type": "string",
        "enum": ["fr", "en", "es", "pt"],
        "default": "fr",
        "example": "fr"
      },
      "notifications": {
        "type": "boolean",
        "default": true,
        "example": true
      },
      "theme": {
        "type": "string",
        "enum": ["light", "dark", "system"],
        "default": "system",
        "example": "light"
      }
    }
  }
}
```

## Role Definitions

### Installer
Field personnel who perform equipment installation and handle service subscriptions.

**Permissions**:
- View assigned interventions
- Create new interventions
- Update intervention details
- Capture and upload photos
- Generate and send documents for signature
- View personal performance metrics

### Back Office
Symphonics administrative staff managing companies, users, and interventions.

**Permissions**:
- Create and manage company accounts
- Approve and manage user accounts
- Create and assign interventions
- View all interventions
- Generate reports
- Configure system parameters

### IT
Technical staff managing system configuration and integration.

**Permissions**:
- Configure API integrations
- Manage document templates
- Monitor system performance
- Access system logs
- Manage data retention and privacy

## Status Definitions

### Pending
User has registered but has not been approved by back office staff.

**Transitions**:
- → Active (when approved by back office)
- → Inactive (if rejected or not approved within a certain timeframe)

### Active
User is approved and can access the system.

**Transitions**:
- → Suspended (temporary restriction)
- → Inactive (permanent deactivation)

### Suspended
User access is temporarily restricted.

**Transitions**:
- → Active (when suspension is lifted)
- → Inactive (when permanently deactivated)

### Inactive
User account is deactivated and cannot access the system.

**Transitions**:
- → Active (if reactivated by back office)

## Authentication

1. **Login Process**:
   - Users authenticate with email and password
   - System validates credentials against stored password hash
   - System verifies user status is "active"
   - System records login timestamp

2. **Password Requirements**:
   - Minimum 8 characters
   - At least one uppercase letter
   - At least one lowercase letter
   - At least one number
   - At least one special character

3. **Security Measures**:
   - Passwords are hashed using bcrypt with appropriate work factor
   - Failed login attempts are rate-limited
   - Session timeout after period of inactivity
   - Multi-factor authentication for sensitive operations (future enhancement)

## Data Validation

1. **Email Validation**:
   - Must be a valid email format
   - Must be unique in the system
   - Domain must be valid

2. **Phone Validation**:
   - Must be a valid phone format
   - International format with country code

3. **Name Validation**:
   - Cannot be empty
   - Maximum length restrictions

## Relationships

1. **Company**: Each user belongs to exactly one company (many-to-one relationship).
2. **Interventions**: Each user can manage multiple interventions (one-to-many relationship).

## Indexing

1. **Primary Key**: `id`
2. **Foreign Keys**: `company_id`
3. **Search Indexes**: `email`, `phone`, `last_name`
4. **Compound Indexes**: `(company_id, status)`, `(role, status)`

## Audit and Compliance

1. **Audit Trail**:
   - All changes to user data are logged
   - Log includes previous values, new values, timestamp, and actor

2. **GDPR Compliance**:
   - Personal data can be anonymized upon request
   - Data export functionality for subject access requests
   - Clear data retention policies

## API Endpoints

1. **GET /users**: List users (with filtering and pagination)
2. **GET /users/{id}**: Get user details
3. **POST /users**: Create new user
4. **PUT /users/{id}**: Update user
5. **PATCH /users/{id}/status**: Update user status
6. **DELETE /users/{id}**: Deactivate user (sets status to inactive)

## Implementation Notes

1. **Storage**: User data is stored in the primary database with appropriate encryption.
2. **Caching**: User authentication data is cached for performance.
3. **Offline Support**: Basic user profile data is available offline for the authenticated user.
