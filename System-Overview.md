# System Overview

## 1. Purpose

The Digital Insurance Platform is a web-based insurance solution that enables customers to obtain a motor insurance quotation and complete the policy purchase journey digitally.

The system integrates customer, vehicle, quotation, payment and policy processes through REST APIs and external services.

---

# 2. System Context

The platform interacts with several internal and external components.

```text
                         ┌──────────────────────┐
                         │      Customer        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │  Web / Mobile App    │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │     API Gateway      │
                         └──────────┬───────────┘
                                    │
                  ┌─────────────────┼─────────────────┐
                  │                 │                 │
                  ▼                 ▼                 ▼
          ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
          │ Quote        │  │ Policy       │  │ Payment      │
          │ Service      │  │ Service      │  │ Service      │
          └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
                 │                 │                 │
                 │                 │                 ▼
                 │                 │        ┌─────────────────┐
                 │                 │        │ Payment Provider│
                 │                 │        └─────────────────┘
                 │                 │
                 ▼                 ▼
          ┌──────────────────────────────┐
          │          Database            │
          └──────────────────────────────┘

                 │
                 ▼
        ┌─────────────────────┐
        │ Vehicle Data Service│
        └─────────────────────┘
```

---

# 3. Main Components

## 3.1 Web / Mobile Application

Provides the customer-facing interface.

Responsibilities:

- Customer information entry
- Vehicle information entry
- Product selection
- Quotation display
- Application management
- Payment initiation
- Policy display

---

## 3.2 API Gateway

Acts as the main entry point for backend APIs.

Responsibilities:

- Request routing
- Authentication
- Authorization
- Request validation
- Rate limiting
- API logging

Example:

```text
POST /api/v1/quotes
GET  /api/v1/quotes/{quoteId}
POST /api/v1/applications
POST /api/v1/payments
GET  /api/v1/policies/{policyId}
```

---

## 3.3 Quote Service

Responsible for quotation-related operations.

Responsibilities:

- Validate quotation request
- Validate vehicle information
- Check product eligibility
- Calculate premium
- Create quotation
- Manage quotation status

---

## 3.4 Policy Service

Responsible for insurance policy lifecycle operations.

Responsibilities:

- Create policy
- Generate policy number
- Activate policy
- Retrieve policy information
- Manage policy status

---

## 3.5 Payment Service

Responsible for communication with the external payment provider.

Responsibilities:

- Initiate payment
- Validate payment response
- Prevent duplicate payment
- Store payment status
- Notify policy service after successful payment

---

## 3.6 Database

Stores business and transaction data.

Main entities:

```text
Customer
Vehicle
InsuranceProduct
Quote
Application
Payment
Policy
```

---

## 3.7 Vehicle Data Service

An external service used to validate vehicle information.

Example request:

```http
GET /vehicles/{plate}
```

Possible responses:

```text
200 OK      → Vehicle found
404 Not Found → Vehicle not found
503 Service Unavailable → External service unavailable
```

---

## 3.8 Payment Provider

External system responsible for processing customer payments.

The platform communicates with this service through a secure API.

---

# 4. End-to-End System Flow

```text
Customer
   │
   │ 1. Enter vehicle information
   ▼
Web Application
   │
   │ 2. Request quotation
   ▼
API Gateway
   │
   ▼
Quote Service
   │
   │ 3. Validate vehicle
   ▼
Vehicle Data Service
   │
   │ 4. Vehicle valid
   ▼
Quote Service
   │
   │ 5. Calculate premium
   ▼
Database
   │
   │ 6. Store quotation
   ▼
Web Application
   │
   │ 7. Customer accepts quotation
   ▼
Application Service
   │
   │ 8. Payment request
   ▼
Payment Service
   │
   ▼
Payment Provider
   │
   │ 9. Payment SUCCESS
   ▼
Policy Service
   │
   │ 10. Create policy
   ▼
Database
   │
   ▼
ACTIVE POLICY
```

---

# 5. Integration Points

| Integration | Direction | Purpose |
|---|---|---|
| Web App → API Gateway | Inbound | Customer transactions |
| API Gateway → Quote Service | Internal | Quotation |
| Quote Service → Vehicle Service | Outbound | Vehicle validation |
| Quote Service → Database | Internal | Quote persistence |
| Application → Payment Service | Internal | Payment initiation |
| Payment Service → Payment Provider | Outbound | Payment processing |
| Payment Service → Policy Service | Internal | Policy creation trigger |
| Policy Service → Database | Internal | Policy persistence |

---

# 6. Error Handling Strategy

The system should distinguish between business and technical errors.

### Business Error

Example:

```json
{
  "errorCode": "VEHICLE_AGE_NOT_SUPPORTED",
  "message": "The vehicle is not eligible for this product."
}
```

HTTP status:

```text
400 Bad Request
```

### External Service Error

Example:

```json
{
  "errorCode": "VEHICLE_SERVICE_UNAVAILABLE",
  "message": "Vehicle information service is temporarily unavailable."
}
```

HTTP status:

```text
503 Service Unavailable
```

### Unexpected Technical Error

Example:

```json
{
  "errorCode": "INTERNAL_SERVER_ERROR",
  "message": "An unexpected error occurred."
}
```

HTTP status:

```text
500 Internal Server Error
```

Technical details must not be exposed to the customer.

---

# 7. Logging & Monitoring

The system should log important integration and transaction events.

Example:

```text
Transaction ID
Request ID
Timestamp
Service Name
Endpoint
HTTP Status
Response Time
Business Error Code
Technical Error
```

These logs support:

- Incident management
- Root cause analysis
- Troubleshooting
- Performance monitoring
- Auditability

---

# 8. Non-Functional Considerations

### Performance

Standard API requests should meet the defined response-time target.

### Availability

Critical services should be designed to minimize service disruption.

### Security

Customer and payment information must be protected.

### Scalability

The architecture should support increasing quotation and policy volumes.

### Observability

Logs and transaction identifiers should allow an operational team to trace a customer transaction across integrated services.

---

# 9. Architecture Principles

The solution follows these principles:

- Separation of responsibilities
- API-based integration
- Clear service boundaries
- Traceable transactions
- Validation before state changes
- Fail-safe policy activation
- Secure handling of customer information
- Explicit business and technical error handling
