# API Test Cases

## 1. Test Case Overview

The following test cases validate the functional behavior, input validation, business rules and error handling of the Digital Insurance Platform APIs.

Test cases include:

- Positive testing
- Negative testing
- Boundary testing
- Business rule validation
- Integration validation

---

# 2. Quote API

## TC-001 — Create Quote with Valid Data

**Endpoint**

```http
POST /api/v1/quotes
```

**Scenario:** Create a quotation using valid customer, vehicle and product information.

**Expected Result:**

- HTTP `201 Created`
- Unique `quoteId` is generated.
- Quote status is `CALCULATED`.
- Premium is greater than zero.

**Type:** Positive

---

## TC-002 — Create Quote Without Customer ID

**Scenario:** Submit quotation request without `customerId`.

**Expected Result:**

- HTTP `400 Bad Request`
- Error code: `CUSTOMER_ID_REQUIRED`
- Quote is not created.

**Type:** Negative

---

## TC-003 — Create Quote Without Vehicle Plate

**Scenario:** Submit quotation without vehicle plate.

**Expected Result:**

- HTTP `400 Bad Request`
- Error code: `PLATE_REQUIRED`
- Quote is not created.

**Type:** Negative

---

## TC-004 — Create Quote with Invalid Vehicle

**Scenario:** Submit a vehicle that cannot be found by the vehicle service.

**Expected Result:**

- HTTP `404 Not Found`
- Error code: `VEHICLE_NOT_FOUND`
- Quote is not created.

**Type:** Negative / Integration

---

## TC-005 — Create Quote with Unsupported Vehicle Year

**Scenario:** Submit a vehicle outside the supported model-year range.

**Expected Result:**

- HTTP `400 Bad Request`
- Error code: `VEHICLE_AGE_NOT_SUPPORTED`
- Quote is not created.

**Type:** Boundary / Business Rule

---

## TC-006 — Create Quote with Inactive Product

**Scenario:** Request a quotation using an inactive insurance product.

**Expected Result:**

- HTTP `400 Bad Request`
- Error code: `PRODUCT_NOT_AVAILABLE`
- Quote is not created.

**Type:** Negative / Business Rule

---

## TC-007 — Vehicle Service Unavailable

**Scenario:** Vehicle validation service is unavailable.

**Expected Result:**

- HTTP `503 Service Unavailable`
- Error code: `VEHICLE_SERVICE_UNAVAILABLE`
- Quote is not created.
- Technical details are not exposed to the customer.

**Type:** Negative / Integration

---

## TC-008 — Retrieve Existing Quote

**Endpoint**

```http
GET /api/v1/quotes/{quoteId}
```

**Scenario:** Retrieve an existing quotation.

**Expected Result:**

- HTTP `200 OK`
- Correct quotation details are returned.
- Quote ID matches the requested resource.

**Type:** Positive

---

## TC-009 — Retrieve Non-Existing Quote

**Scenario:** Request a quotation using an invalid quote ID.

**Expected Result:**

- HTTP `404 Not Found`
- Error code: `QUOTE_NOT_FOUND`

**Type:** Negative

---

# 3. Application API

## TC-010 — Create Application from Valid Quote

**Endpoint**

```http
POST /api/v1/applications
```

**Scenario:** Create an application using a valid and non-expired quotation.

**Expected Result:**

- HTTP `201 Created`
- Unique application ID is generated.
- Application status is `PENDING_PAYMENT`.

**Type:** Positive

---

## TC-011 — Create Application from Expired Quote

**Scenario:** Attempt to create an application from an expired quotation.

**Expected Result:**

- HTTP `400 Bad Request`
- Error code: `QUOTE_EXPIRED`
- Application is not created.

**Type:** Negative / Business Rule

---

## TC-012 — Create Application from Invalid Quote Status

**Scenario:** Attempt to create an application from a quotation with status `REJECTED`.

**Expected Result:**

- HTTP `400 Bad Request`
- Error code: `INVALID_QUOTE_STATUS`
- Application is not created.

**Type:** Negative / Business Rule

---

# 4. Payment API

## TC-013 — Successful Payment

**Endpoint**

```http
POST /api/v1/payments
```

**Scenario:** Process payment for a valid pending application.

**Expected Result:**

- HTTP `200 OK`
- Payment status is `SUCCESS`.
- Payment transaction ID is generated.
- Application status changes appropriately.
- Policy creation process is triggered.

**Type:** Positive / Integration

---

## TC-014 — Failed Payment

**Scenario:** Payment provider rejects the transaction.

**Expected Result:**

- HTTP `402 Payment Required`
- Payment status is `FAILED`.
- Application remains unpaid.
- Policy is not activated.

**Type:** Negative / Integration

---

## TC-015 — Duplicate Payment

**Scenario:** Submit the same payment transaction more than once.

**Expected Result:**

- Duplicate payment must not be processed.
- Existing transaction remains unchanged.
- Appropriate business error is returned.

Example:

```text
409 Conflict
DUPLICATE_PAYMENT
```

**Type:** Negative / Business Rule

---

## TC-016 — Payment for Non-Existing Application

**Scenario:** Submit payment using an invalid application ID.

**Expected Result:**

- HTTP `404 Not Found`
- Error code: `APPLICATION_NOT_FOUND`
- Payment is not processed.

**Type:** Negative

---

# 5. Policy API

## TC-017 — Retrieve Active Policy

**Endpoint**

```http
GET /api/v1/policies/{policyId}
```

**Scenario:** Retrieve an existing active policy.

**Expected Result:**

- HTTP `200 OK`
- Correct policy information is returned.
- Policy status is `ACTIVE`.

**Type:** Positive

---

## TC-018 — Policy Must Not Be Active Without Successful Payment

**Scenario:** Attempt to create or activate a policy when payment status is not `SUCCESS`.

**Expected Result:**

- Policy must not become `ACTIVE`.
- Appropriate business error is returned.
- Transaction is logged.

**Type:** Negative / Business Rule

---

## TC-019 — Duplicate Active Policy

**Scenario:** Attempt to create another active policy for the same customer, vehicle and product where duplication is not allowed.

**Expected Result:**

- HTTP `409 Conflict`
- Error code: `DUPLICATE_ACTIVE_POLICY`
- New policy is not created.

**Type:** Negative / Business Rule

---

# 6. Data Validation

## TC-020 — Policy-Payment Amount Mismatch

**Scenario:** Payment amount differs from the policy premium.

Example:

```text
Policy Premium: 28,500 TRY
Payment Amount: 25,000 TRY
```

**Expected Result:**

- Transaction must not activate the policy.
- Data inconsistency must be logged.
- Appropriate business error should be generated.

**Type:** Negative / Data Validation

---

# 7. Test Coverage Summary

| Test Type | Test Cases |
|---|---:|
| Positive | 4 |
| Negative | 8 |
| Business Rule | 6 |
| Integration | 4 |
| Boundary
