# Business Rules

## 1. Purpose

This document defines the business rules governing the Digital Insurance Platform.

The rules determine whether a quotation, application, payment or policy transaction can proceed.

---

# 2. Customer Rules

### BR-001 — Customer Identification

A customer must have a valid unique customer ID before a quotation can be created.

**Rule:**

```text
Customer ID must exist and be valid.
```

If the customer cannot be identified, the quotation request must be rejected.

---

### BR-002 — Mandatory Customer Information

The following information is required:

- Customer ID
- First name
- Last name
- Date of birth
- Contact information

A quotation cannot be generated when mandatory customer information is missing.

---

# 3. Vehicle Rules

### BR-003 — Mandatory Vehicle Information

The following information is required:

- License plate
- Vehicle brand
- Vehicle model
- Model year
- Vehicle type

---

### BR-004 — Vehicle Validation

Vehicle information must be validated before a quotation is generated.

Possible validation results:

```text
VALID
INVALID
NOT_FOUND
SERVICE_UNAVAILABLE
```

Only `VALID` vehicles can proceed to quotation calculation.

---

### BR-005 — Model Year Validation

The vehicle model year must be within the range supported by the selected insurance product.

If the vehicle is outside the supported range:

```text
VEHICLE_AGE_NOT_SUPPORTED
```

must be returned.

---

### BR-006 — Vehicle Ownership

The vehicle must be associated with the customer requesting the insurance product, unless the selected product explicitly supports a different ownership model.

---

# 4. Insurance Product Rules

### BR-007 — Product Availability

Only products with an `ACTIVE` status can be selected by customers.

```text
ACTIVE      → Selectable
INACTIVE    → Not selectable
```

---

### BR-008 — Product Eligibility

The customer and vehicle must satisfy the eligibility criteria defined for the selected insurance product.

If eligibility criteria are not satisfied, the system must not generate a quotation.

---

# 5. Quotation Rules

### BR-009 — Unique Quote ID

Every quotation must have a unique quotation ID.

Example:

```text
Q-2026-000145
```

---

### BR-010 — Premium Must Be Positive

The calculated premium must be greater than zero.

```text
Premium > 0
```

A quotation cannot be issued when:

```text
Premium <= 0
```

---

### BR-011 — Quotation Validity

Every quotation must have an expiration date.

Example:

```text
Quotation Date: 2026-09-30
Valid Until:    2026-10-07
```

An expired quotation cannot be converted into an insurance application.

---

### BR-012 — Quotation Status

A quotation may have the following statuses:

```text
CALCULATING
CALCULATED
EXPIRED
REJECTED
CANCELLED
```

Only `CALCULATED` quotations can proceed to application creation.

---

# 6. Application Rules

### BR-013 — Application Creation

An application can only be created from a valid quotation.

Required conditions:

```text
Quote exists
AND
Quote status = CALCULATED
AND
Quote is not expired
```

---

### BR-014 — Unique Application ID

Every application must have a unique application ID.

Example:

```text
APP-2026-001245
```

---

### BR-015 — Application Status

An application may have the following statuses:

```text
DRAFT
PENDING_PAYMENT
PAID
CANCELLED
COMPLETED
```

---

# 7. Payment Rules

### BR-016 — Payment Required

A policy cannot become active without successful payment.

```text
Payment Status != SUCCESS
        ↓
Policy cannot become ACTIVE
```

---

### BR-017 — Payment Failure

When payment fails:

```text
Payment Status = FAILED
Application Status = PENDING_PAYMENT
Policy = NOT_CREATED
```

The customer must be informed that the payment could not be completed.

---

### BR-018 — Duplicate Payment

The system must prevent duplicate payment processing for the same application and payment transaction.

A unique transaction reference should be used for payment tracking.

---

# 8. Policy Rules

### BR-019 — Policy Creation

A policy can only be created when:

```text
Application is valid
AND
Payment Status = SUCCESS
AND
Required validations are completed
```

---

### BR-020 — Unique Policy Number

Every policy must have a unique policy number.

Example:

```text
POL-2026-000981
```

---

### BR-021 — Policy Status

A policy may have the following statuses:

```text
PENDING
ACTIVE
CANCELLED
EXPIRED
```

---

### BR-022 — Policy Activation

A policy can become `ACTIVE` only after successful policy creation and completion of all required validations.

---

# 9. Duplicate Policy Rules

### BR-023 — Duplicate Active Policy

The system must check whether an active policy already exists for the same customer and vehicle when the selected insurance product does not allow multiple active policies.

Example:

```text
Customer: 10025
Vehicle: 35ABC123
Product: CASCO

Existing Policy: ACTIVE
```

Result:

```text
DUPLICATE_ACTIVE_POLICY
```

The new policy must not be created.

---

# 10. Integration Rules

### BR-024 — External Vehicle Service

If the external vehicle service is unavailable, the system must not continue with quotation calculation.

The transaction should be recorded for troubleshooting.

---

### BR-025 — Payment Service Failure

If the payment service is unavailable:

- Payment must not be marked as successful.
- Policy must not be activated.
- The customer must receive an appropriate message.
- The transaction must be traceable.

---

# 11. Error Handling Rules

### BR-026 — Business Errors

Business validation failures must return a meaningful business error code.

Example:

```json
{
  "errorCode": "VEHICLE_AGE_NOT_SUPPORTED",
  "message": "The vehicle is not eligible for the selected insurance product."
}
```

---

### BR-027 — Technical Errors

Technical errors must not expose internal system details to customers.

The customer should receive a generic message while detailed technical information is recorded in system logs.

---

# 12. Data Integrity Rules

### BR-028 — Referential Integrity

The following relationships must remain valid:

```text
Customer → Quote
Quote → Application
Application → Payment
Application → Policy
Vehicle → Policy
```

A child record must not reference a non-existent parent record.

---

### BR-029 — Policy-Payment Consistency

An active policy must have a corresponding successful payment record.

Example validation:

```sql
SELECT p.policy_number
FROM policies p
LEFT JOIN payments pay
    ON p.application_id = pay.application_id
WHERE p.policy_status = 'ACTIVE'
AND (pay.payment_status IS NULL
     OR pay.payment_status <> 'SUCCESS');
```

The query should return **zero records**.

---

# 13. Traceability Rules

###
