# Functional Requirements Document (FRD)

## 1. Document Information

| Field | Description |
|---|---|
| Project | Digital Insurance Platform |
| Document | Functional Requirements Document |
| Version | 1.0 |
| Status | Draft |
| Domain | Motor Insurance |

---

# 2. Functional Requirements

## FR-001 — Customer Information

The system shall allow the customer to enter the information required to create a quotation.

### Input

- Customer ID
- First name
- Last name
- Date of birth
- Contact information

### Validation

- Mandatory fields must not be empty.
- Customer ID must be valid.
- Date of birth must follow the defined format.

---

## FR-002 — Vehicle Information

The system shall allow the customer to enter vehicle information.

### Input

- License plate
- Vehicle brand
- Vehicle model
- Model year
- Vehicle type

### Validation

- License plate is mandatory.
- Model year must be valid.
- Required vehicle information must be provided.

---

## FR-003 — Vehicle Validation

The system shall validate the entered vehicle information.

### Process

1. Customer submits vehicle information.
2. System validates mandatory fields.
3. System sends the vehicle information to the vehicle data service.
4. Vehicle information is validated.
5. System returns the validation result.

### Possible Results

```text
VALID
INVALID
NOT_FOUND
SERVICE_UNAVAILABLE
```

---

## FR-004 — Product Selection

The system shall display available insurance products for the customer.

The customer shall be able to select an available product.

Example:

```text
Product: CASCO
Coverage: Comprehensive
Status: Available
```

---

## FR-005 — Quotation Request

The system shall allow the customer to request a quotation after successful validation of customer and vehicle information.

### Preconditions

- Customer information is valid.
- Vehicle information is valid.
- Insurance product is selected.

### Result

The system shall create a unique quotation ID.

Example:

```text
Quote ID: Q-2026-000145
Status: CALCULATED
```

---

## FR-006 — Premium Calculation

The system shall calculate the insurance premium based on applicable business rules.

Potential calculation factors include:

- Vehicle model year
- Vehicle type
- Product type
- Customer information
- Coverage options
- Risk parameters

The calculation result shall contain:

- Base premium
- Applicable discounts
- Taxes
- Final premium

---

## FR-007 — Quotation Display

The system shall display the quotation details to the customer.

The quotation shall include:

- Quote ID
- Product
- Coverage
- Premium
- Currency
- Validity date
- Selected vehicle

---

## FR-008 — Application Creation

The customer shall be able to create an insurance application from a valid quotation.

### Preconditions

- Quotation exists.
- Quotation has not expired.
- Quotation status is `CALCULATED`.

### Result

An application ID shall be generated.

Example:

```text
Application ID: APP-2026-001245
Status: PENDING_PAYMENT
```

---

## FR-009 — Payment

The system shall send payment information to the payment service.

### Successful Payment

```text
HTTP Status: 200
Payment Status: SUCCESS
```

The application shall proceed to policy creation.

### Failed Payment

```text
Payment Status: FAILED
```

The application shall remain unpaid and the policy shall not be activated.

---

## FR-010 — Policy Creation

The system shall create a policy after successful payment.

The policy shall contain:

- Policy number
- Customer ID
- Vehicle ID
- Product
- Premium
- Start date
- End date
- Policy status

Example:

```text
Policy Number: POL-2026-000981
Status: ACTIVE
```

---

## FR-011 — Policy Status

The system shall support the following policy statuses:

```text
PENDING
ACTIVE
CANCELLED
EXPIRED
```

---

## FR-012 — Error Handling

The system shall return appropriate error responses.

Example:

```json
{
  "errorCode": "VEHICLE_NOT_FOUND",
  "message": "Vehicle information could not be found."
}
```

---

## FR-013 — API Response

The API shall return an appropriate HTTP status code.

| Scenario | HTTP Status |
|---|---:|
| Successful creation | 201 |
| Successful request | 200 |
| Invalid request | 400 |
| Unauthorized | 401 |
| Forbidden | 403 |
| Resource not found | 404 |
| Business conflict | 409 |
| Server error | 500 |

---

## FR-014 — Traceability

The system shall maintain relationships between:

```text
Customer
   ↓
Quotation
   ↓
Application
   ↓
Payment
   ↓
Policy
```

Each transaction shall have a unique identifier.

---

## FR-015 — Audit Information

The system should record relevant transaction information, including:

- Transaction ID
- Request timestamp
- Response timestamp
- Transaction status
- Error code where applicable

This information supports troubleshooting, auditing and operational analysis.

---

# 3. Non-Functional Requirements

## NFR-001 — Performance

For standard requests, the API should respond within the defined service-level target.

Target:

```text
95% of standard API requests < 2 seconds
```

## NFR-002 — Availability

The platform should be highly available during defined business and operational hours.

## NFR-003 — Security

Sensitive customer information must be protected during transmission and storage.

## NFR-004 — Scalability

The solution should support an increase in quotation and policy transactions without significant degradation.

## NFR-005 — Logging

Application and integration errors should be logged with sufficient information for troubleshooting.

---

# 4. Functional Flow

```text
START
  │
  ▼
Enter Customer Information
  │
  ▼
Enter Vehicle Information
  │
  ▼
Validate Vehicle
  │
  ├── Invalid ──► Display Error
  │
  ▼
Select Product
  │
  ▼
Calculate Premium
  │
  ▼
Display Quotation
  │
  ├── Reject ──► END
  │
  ▼
Create Application
  │
  ▼
Payment
  │
  ├── Failed ──► Payment Error
  │
  ▼
Create Policy
  │
  ▼
Activate Policy
  │
  ▼
END
```
