# Digital Insurance Platform
### Business Analysis · System Analysis · API Testing · SQL · UAT

An end-to-end insurance platform case study demonstrating practical skills in **Business Analysis, System Analysis, Software Testing, API Testing, SQL and User Acceptance Testing (UAT)**.

The project simulates a digital insurance journey where a customer can obtain a motor insurance quotation, submit an application, complete payment and receive an active insurance policy.

---

## 🎯 Project Objective

The objective of this project is to analyze and document an end-to-end digital insurance solution from both business and technical perspectives.

The project covers the complete lifecycle:

**Requirement → Analysis → System Design → API → Database → Testing → UAT**

---

## 🏢 Business Scenario

A customer wants to purchase motor insurance through a digital channel.

The system should allow the customer to:

1. Enter vehicle information
2. Retrieve vehicle details
3. Select an insurance product
4. Request an insurance quotation
5. Review the calculated premium
6. Submit an insurance application
7. Complete the payment
8. Create and activate the insurance policy
9. View policy information

### High-Level Process

```text
Customer
   │
   ▼
Vehicle Information
   │
   ▼
Quotation Request
   │
   ▼
Premium Calculation
   │
   ▼
Insurance Application
   │
   ▼
Payment
   │
   ▼
Policy Creation
   │
   ▼
Active Policy
```

---

## 👩‍💻 Business Analysis

The project includes:

- Business Requirements
- Functional Requirements
- User Stories
- Acceptance Criteria
- Business Rules
- Process Analysis
- BPMN
- Use Case Analysis

### Example User Story

> As a customer, I want to receive a motor insurance quotation by providing my vehicle information so that I can review the premium before purchasing the policy.

### Example Acceptance Criteria

**Given** the customer provides valid vehicle information  
**When** the customer submits a quotation request  
**Then** the system should validate the vehicle information  
**And** calculate the applicable premium  
**And** return the quotation to the customer.

---

## 🏗️ System Analysis

The system analysis covers:

- System architecture
- Application components
- REST API design
- Integration points
- Data flow
- UML diagrams
- Sequence diagrams
- Entity Relationship Diagram (ERD)
- Error handling

### High-Level Architecture

```text
┌───────────────┐
│    Customer   │
└───────┬───────┘
        │
        ▼
┌───────────────────┐
│ Web / Mobile App  │
└────────┬──────────┘
         │
         ▼
┌───────────────────┐
│    API Gateway    │
└────────┬──────────┘
         │
    ┌────┴─────────────┐
    ▼                  ▼
┌─────────────┐  ┌──────────────┐
│ Policy      │  │ Payment      │
│ Service     │  │ Service      │
└──────┬──────┘  └──────────────┘
       │
       ▼
┌─────────────────┐
│   Database      │
└─────────────────┘
```

---

## 🔌 API Analysis & Testing

The project includes REST API analysis and testing using **Postman**.

Example endpoint:

```http
POST /api/v1/quotes
```

Example request:

```json
{
  "customerId": 10025,
  "vehicle": {
    "plate": "35ABC123",
    "brand": "Toyota",
    "model": "Corolla",
    "modelYear": 2024
  },
  "product": "CASCO"
}
```

Example response:

```json
{
  "quoteId": "Q-2026-000145",
  "status": "CALCULATED",
  "premium": 28500,
  "currency": "TRY"
}
```

Testing covers:

- Positive scenarios
- Negative scenarios
- Boundary value testing
- Required field validation
- HTTP status code validation
- Response validation
- Business rule validation
- Error handling

---

## 🧪 Software Testing

The testing phase includes:

### Test Planning

Definition of:

- Test scope
- Test objectives
- Test approach
- Test data
- Entry criteria
- Exit criteria

### Test Scenarios

Examples:

| ID | Scenario | Expected Result |
|---|---|---|
| TC-001 | Create quotation with valid data | Quotation created |
| TC-002 | Submit quotation without vehicle plate | Validation error |
| TC-003 | Submit invalid vehicle year | Validation error |
| TC-004 | Create application from valid quotation | Application created |
| TC-005 | Complete successful payment | Payment confirmed |
| TC-006 | Create policy after successful payment | Policy activated |

---

## 🗄️ SQL & Data Validation

SQL is used to validate business and system data.

Example validation:

```sql
SELECT *
FROM policies
WHERE policy_status = 'ACTIVE'
AND premium <= 0;
```

This query identifies active policies with an invalid premium value.

Additional SQL scenarios include:

- Data validation
- Duplicate detection
- Referential integrity checks
- Business rule validation
- Policy and payment reconciliation
- Customer-policy relationship analysis

---

## 🧑‍💼 User Acceptance Testing

UAT scenarios validate whether the system meets the defined business requirements.

Example:

**Scenario:** Customer purchases a motor insurance policy.

```text
1. Customer enters vehicle information
2. System validates vehicle information
3. Customer requests quotation
4. System calculates premium
5. Customer accepts quotation
6. Customer completes payment
7. System creates policy
8. Policy becomes ACTIVE
```

Expected result:

> The customer successfully completes the insurance purchase journey and receives an active policy.

---

## 📊 Project Deliverables

| Area | Deliverables |
|---|---|
| Business Analysis | BRD, FRD, User Stories, Acceptance Criteria |
| Process Analysis | BPMN, Process Flow |
| System Analysis | Architecture, UML, Sequence Diagram |
| API | REST API Specification |
| Testing | Test Plan, Test Cases, Defect Reports |
| API Testing | Postman Collection |
| Database | ERD, SQL Scripts |
| Data Validation | SQL Queries |
| UAT | UAT Scenarios & Report |

---

## 🛠️ Tools & Technologies

- Business Analysis
- System Analysis
- REST API
- JSON
- SQL
- Postman
- Jira
- Confluence
- BPMN
- UML
- ERD
- Agile / Scrum
- UAT
- Functional Testing
- API Testing

---

## 📌 Project Scope

This is a **portfolio case study** created for demonstrating Business Analyst, System Analyst and QA capabilities.

The project does not contain confidential information, source code, business rules, customer data or internal documentation from any real insurance company.

---

## 👤 Skills Demonstrated

**Business Analysis**
- Requirements Analysis
- Functional Analysis
- User Stories
- Acceptance Criteria
- Business Rules
- Process Modeling

**System Analysis**
- System Architecture
- API Analysis
- Integration Analysis
- UML
- Data Modeling

**Quality Assurance**
- Test Planning
- Test Case Design
- Functional Testing
- API Testing
- Defect Reporting
- UAT

**Technical**
- SQL
- REST API
- JSON
- Postman
- Data Validation

---

## 📈 Future Enhancements

Potential future extensions include:

- Automated API tests
- CI/CD integration
- Additional insurance products
- Claims management module
- Authentication and authorization
- Dashboard and reporting
- Automated SQL validation
- Performance testing
