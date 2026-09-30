# API Testing Test Plan

## 1. Document Information

| Field | Description |
|---|---|
| Project | Digital Insurance Platform |
| Test Area | REST API Testing |
| Version | 1.0 |
| Testing Type | Functional & Integration Testing |
| Tool | Postman |

---

## 2. Test Objective

The objective of API testing is to verify that the Digital Insurance Platform APIs:

- Meet defined functional requirements
- Return correct HTTP status codes
- Validate request data correctly
- Apply business rules correctly
- Return expected response structures
- Handle invalid requests appropriately
- Handle external service failures correctly
- Maintain data consistency between related services

---

## 3. Scope

### In Scope

The following API operations are included:

- Create quotation
- Retrieve quotation
- Create application
- Process payment
- Retrieve policy
- Error handling
- Input validation
- Business rule validation

### Out of Scope

- UI testing
- Performance/load testing
- Security penetration testing
- Real payment processing
- Production environment testing

---

## 4. API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/v1/quotes` | Create quotation |
| GET | `/api/v1/quotes/{quoteId}` | Retrieve quotation |
| POST | `/api/v1/applications` | Create application |
| POST | `/api/v1/payments` | Process payment |
| GET | `/api/v1/policies/{policyId}` | Retrieve policy |

---

## 5. Test Types

### Functional Testing

Verifies that each endpoint performs its intended business function.

### Positive Testing

Valid requests should return the expected successful response.

### Negative Testing

Invalid requests should be rejected with appropriate error responses.

### Boundary Testing

Values at or around business limits should be tested.

Examples:

- Vehicle model year
- Premium amount
- Required fields
- String length

### Business Rule Testing

API behavior should comply with defined business rules.

Example:

```text
Expired quotation
        ↓
Application creation rejected
```

### Integration Testing

Verifies communication between:

- Quote Service
- Vehicle Data Service
- Application Service
- Payment Service
- Policy Service

---

## 6. Test Environment

Example environment:

```text
Environment: QA
Base URL: https://qa-api.example.com
Authentication: Bearer Token
Content-Type: application/json
```

> The URLs and credentials used in this portfolio are fictional and must not contain real company information.

---

## 7. Test Data

Test data should include:

### Valid Data

- Valid customer
- Valid vehicle
- Active insurance product
- Valid quotation
- Valid payment information

### Invalid Data

- Missing customer ID
- Invalid vehicle plate
- Unsupported vehicle model year
- Expired quotation
- Invalid product
- Invalid payment request

---

## 8. Entry Criteria

Testing can begin when:

- API endpoints are available.
- API documentation is available.
- Test environment is accessible.
- Required test data is available.
- Business requirements are approved.

---

## 9. Exit Criteria

Testing can be completed when:

- Planned test cases are executed.
- Critical and blocker defects are resolved.
- Business-critical scenarios pass.
- Failed test cases have been analyzed.
- Test results are documented.

---

## 10. Defect Severity

| Severity | Description |
|---|---|
| Critical | Prevents a core business process from functioning |
| High | Major functionality is unavailable or incorrect |
| Medium | Functionality works with a significant limitation |
| Low | Minor issue with limited business impact |

---

## 11. Expected Test Deliverables

The testing phase will produce:

- Test Cases
- Postman Collection
- Test Execution Results
- Defect Reports
- Test Summary

---

## 12. Traceability

API test cases will be linked to:

```text
Business Requirement
        ↓
Functional Requirement
        ↓
User Story
        ↓
API Endpoint
        ↓
Test Case
        ↓
Test Result
        ↓
Defect (if applicable)
```

This ensures end-to-end requirement and test traceability.
