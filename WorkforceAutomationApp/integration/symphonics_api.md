# Symphonics API Integration

This document provides complete API documentation for the Symphonics platform integration, including authentication, endpoints, and data payloads.

## Overview

The Workforce Automation App integrates with the Symphonics platform to:
1. Validate customer information (PDL, user accounts)
2. Publish completed intervention data
3. Synchronize contract information

The integration follows a RESTful API approach with JSON payloads and standard HTTP methods.

## Authentication

### Authentication Method

The Symphonics API uses OAuth 2.0 for authentication:

1. **Client Credentials Flow**:
   - Used for server-to-server communication
   - Requires client ID and client secret
   - Returns access token with configurable expiration

### Authentication Endpoints

```
POST https://api.symphonics.fr/auth/token
```

**Request Headers**:
```
Content-Type: application/x-www-form-urlencoded
```

**Request Body**:
```
grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}
```

**Response**:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Using Authentication

For all API requests, include the access token in the Authorization header:

```
Authorization: Bearer {ACCESS_TOKEN}
```

### Token Management

- Tokens expire after the time specified in `expires_in` (in seconds)
- Implement token refresh logic to obtain a new token before expiration
- Store tokens securely and never expose them in client-side code

## API Endpoints

### Validation Endpoints

#### 1. PDL Validation

Validates a PDL (Point de Livraison) number.

```
GET https://api.symphonics.fr/validation/pdl/{pdl}
```

**Parameters**:
- `pdl` (path): The PDL number to validate

**Response (200 OK)**:
```json
{
  "valid": true,
  "pdl": "12345678901234",
  "address": {
    "street": "123 Rue de Paris",
    "postal_code": "75001",
    "city": "Paris"
  },
  "meter_type": "electronic"
}
```

**Response (400 Bad Request)**:
```json
{
  "valid": false,
  "error": "Invalid PDL format",
  "message": "PDL must be 14 digits"
}
```

#### 2. PDL Lookup by Address

Looks up a PDL based on address and customer name.

```
POST https://api.symphonics.fr/validation/pdl/lookup
```

**Request Body**:
```json
{
  "address": {
    "street": "123 Rue de Paris",
    "postal_code": "75001",
    "city": "Paris"
  },
  "customer_name": "Martin"
}
```

**Response (200 OK)**:
```json
{
  "pdl": "12345678901234",
  "confidence": 0.95,
  "address": {
    "street": "123 Rue de Paris",
    "postal_code": "75001",
    "city": "Paris"
  },
  "meter_type": "electronic"
}
```

#### 3. User Validation

Validates if a user exists in the Symphonics platform.

```
GET https://api.symphonics.fr/validation/user/{email}
```

**Parameters**:
- `email` (path): The email address to validate

**Response (200 OK)**:
```json
{
  "exists": true,
  "user_id": "usr_12345678",
  "status": "active"
}
```

**Response (404 Not Found)**:
```json
{
  "exists": false,
  "message": "User not found"
}
```

### CRM Endpoints

#### 1. Create Intervention

Creates a new intervention in the Symphonics platform.

```
POST https://api.symphonics.fr/crm/interventions
```

**Request Body**:
```json
{
  "customer": {
    "first_name": "Thomas",
    "last_name": "Martin",
    "email": "thomas.martin@example.com",
    "phone": "+33612345678"
  },
  "installation": {
    "address": "45 Rue des Fleurs",
    "postal_code": "75015",
    "city": "Paris",
    "pdl": "12345678901234",
    "pce": "98765432109876"
  },
  "services": [
    {
      "code": "BAR-TH-173",
      "name": "Régulateur de ballon d'eau chaude",
      "quantity": 1
    }
  ],
  "equipment": [
    {
      "name": "Régulateur Thaléos",
      "model": "RT-2000",
      "serial_number": "TH2023456789",
      "quantity": 1
    }
  ],
  "documents": [
    {
      "type": "invoice",
      "name": "Facture - Thomas Martin - 25/03/2025",
      "url": "https://app.workforce-automation.com/documents/int_12345678/invoice.pdf"
    },
    {
      "type": "attestation_honneur",
      "name": "Attestation d'Honneur - Thomas Martin - 25/03/2025",
      "url": "https://app.workforce-automation.com/documents/int_12345678/ah.pdf"
    },
    {
      "type": "contract",
      "name": "Contrat Symphonics - Thomas Martin - 25/03/2025",
      "url": "https://app.workforce-automation.com/documents/int_12345678/contract.pdf"
    }
  ],
  "photos": [
    {
      "type": "before",
      "url": "https://app.workforce-automation.com/photos/int_12345678/before_1.jpg",
      "coordinates": {
        "latitude": 48.856614,
        "longitude": 2.352222
      },
      "timestamp": "2025-03-25T10:30:00Z"
    },
    {
      "type": "after",
      "url": "https://app.workforce-automation.com/photos/int_12345678/after_1.jpg",
      "coordinates": {
        "latitude": 48.856614,
        "longitude": 2.352222
      },
      "timestamp": "2025-03-25T11:15:00Z"
    }
  ],
  "installer": {
    "id": "usr_87654321",
    "company_id": "cmp_12345678",
    "name": "Jean Dupont"
  },
  "completion_date": "2025-03-25T11:30:00Z",
  "external_id": "int_12345678"
}
```

**Response (201 Created)**:
```json
{
  "id": "sym_87654321",
  "status": "created",
  "external_id": "int_12345678",
  "created_at": "2025-03-25T11:35:00Z"
}
```

#### 2. Get Intervention Status

Retrieves the status of an intervention in the Symphonics platform.

```
GET https://api.symphonics.fr/crm/interventions/{id}
```

**Parameters**:
- `id` (path): The Symphonics intervention ID

**Response (200 OK)**:
```json
{
  "id": "sym_87654321",
  "external_id": "int_12345678",
  "status": "processing",
  "created_at": "2025-03-25T11:35:00Z",
  "updated_at": "2025-03-25T11:40:00Z",
  "next_steps": [
    {
      "type": "verification",
      "status": "pending",
      "estimated_completion": "2025-03-26T12:00:00Z"
    }
  ]
}
```

#### 3. Update Intervention

Updates an existing intervention in the Symphonics platform.

```
PATCH https://api.symphonics.fr/crm/interventions/{id}
```

**Parameters**:
- `id` (path): The Symphonics intervention ID

**Request Body**:
```json
{
  "documents": [
    {
      "type": "additional_proof",
      "name": "Preuve supplémentaire - Thomas Martin - 26/03/2025",
      "url": "https://app.workforce-automation.com/documents/int_12345678/additional_proof.pdf"
    }
  ]
}
```

**Response (200 OK)**:
```json
{
  "id": "sym_87654321",
  "status": "updated",
  "external_id": "int_12345678",
  "updated_at": "2025-03-26T10:15:00Z"
}
```

## Webhooks

Symphonics provides webhooks to notify the Workforce Automation App of events.

### Webhook Configuration

Webhooks must be configured in the Symphonics developer portal. Provide:
1. Webhook URL: `https://app.workforce-automation.com/api/webhooks/symphonics`
2. Events to subscribe to
3. Secret key for signature verification

### Webhook Events

#### 1. Intervention Status Update

```json
{
  "event": "intervention.status_update",
  "timestamp": "2025-03-26T14:30:00Z",
  "data": {
    "intervention_id": "sym_87654321",
    "external_id": "int_12345678",
    "previous_status": "processing",
    "new_status": "approved",
    "notes": "All documents verified successfully"
  }
}
```

#### 2. Document Verification

```json
{
  "event": "document.verification",
  "timestamp": "2025-03-26T13:45:00Z",
  "data": {
    "intervention_id": "sym_87654321",
    "external_id": "int_12345678",
    "document_type": "attestation_honneur",
    "verification_status": "verified",
    "notes": "Document verified successfully"
  }
}
```

### Webhook Security

1. **Signature Verification**:
   - Each webhook request includes an `X-Symphonics-Signature` header
   - Verify the signature using HMAC-SHA256 with your secret key
   - Reject requests with invalid signatures

2. **Timestamp Validation**:
   - Each webhook includes a timestamp
   - Reject requests with timestamps older than 5 minutes to prevent replay attacks

## Error Handling

### Error Codes

| HTTP Status | Error Code | Description |
|-------------|------------|-------------|
| 400 | INVALID_REQUEST | The request was malformed or missing required fields |
| 401 | UNAUTHORIZED | Authentication failed or token expired |
| 403 | FORBIDDEN | The authenticated user lacks permission |
| 404 | NOT_FOUND | The requested resource was not found |
| 409 | CONFLICT | The request conflicts with the current state |
| 422 | VALIDATION_ERROR | The request data failed validation |
| 429 | RATE_LIMITED | Too many requests, please retry later |
| 500 | SERVER_ERROR | An unexpected error occurred on the server |

### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid PDL format",
    "details": [
      {
        "field": "installation.pdl",
        "message": "PDL must be 14 digits"
      }
    ],
    "request_id": "req_abcdef123456"
  }
}
```

### Handling Strategies

1. **Retry Logic**:
   - Implement exponential backoff for 429 and 5xx errors
   - Maximum of 5 retries with increasing delays

2. **Logging**:
   - Log all error responses with request_id for troubleshooting
   - Include timestamp and context information

3. **Alerting**:
   - Set up alerts for persistent errors
   - Monitor error rates for anomaly detection

## Rate Limiting

The Symphonics API implements rate limiting to ensure fair usage:

- **Default Limits**:
  - 100 requests per minute per client
  - 5,000 requests per day per client

- **Headers**:
  - `X-RateLimit-Limit`: Total requests allowed in the current period
  - `X-RateLimit-Remaining`: Remaining requests in the current period
  - `X-RateLimit-Reset`: Time when the limit resets (Unix timestamp)

- **Handling Rate Limits**:
  - When a 429 response is received, check the `Retry-After` header
  - Wait for the specified number of seconds before retrying

## Implementation Best Practices

1. **Resilience**:
   - Implement circuit breaker pattern for API calls
   - Handle network failures gracefully
   - Use timeouts to prevent hanging requests

2. **Performance**:
   - Minimize payload size by including only necessary fields
   - Use compression for large requests/responses
   - Implement caching where appropriate

3. **Security**:
   - Store API credentials securely
   - Implement token rotation
   - Use HTTPS for all communications
   - Validate all webhook signatures

4. **Monitoring**:
   - Track API call success/failure rates
   - Monitor response times
   - Set up alerts for abnormal patterns

## Testing

### Sandbox Environment

Symphonics provides a sandbox environment for testing:
- Base URL: `https://sandbox-api.symphonics.fr`
- Test credentials are provided in the developer portal
- Test data is reset daily

### Test PDL Numbers

| PDL Number | Description |
|------------|-------------|
| 12345678901234 | Valid PDL, residential customer |
| 23456789012345 | Valid PDL, commercial customer |
| 34567890123456 | Valid PDL, precarious customer |
| 45678901234567 | Invalid PDL, will return error |

### Test User Emails

| Email | Description |
|-------|-------------|
| test.user@example.com | Existing user, active |
| inactive.user@example.com | Existing user, inactive |
| nonexistent@example.com | Non-existent user |

## Support

For API support, contact:
- Email: api-support@symphonics.fr
- Developer Portal: https://developers.symphonics.fr
- API Status: https://status.symphonics.fr
