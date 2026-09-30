# User Stories

## Epic 1 — Customer & Vehicle Information

### US-001 — Enter Customer Information

**As a** customer  
**I want to** enter my personal information  
**So that** the system can identify me during the insurance quotation process.

#### Acceptance Criteria

**AC-001**
- Given the customer is on the quotation page
- When the customer enters all mandatory information
- Then the system should accept the information.

**AC-002**
- Given a mandatory field is empty
- When the customer submits the form
- Then the system should display a validation message.

**AC-003**
- Given the customer enters an invalid date format
- When the customer submits the form
- Then the system should reject the input.

---

### US-002 — Enter Vehicle Information

**As a** customer  
**I want to** enter my vehicle information  
**So that** the system can calculate an appropriate insurance quotation.

#### Acceptance Criteria

- License plate must be mandatory.
- Vehicle model year must be valid.
- Vehicle information must be validated before quotation calculation.
- Invalid vehicle information must result in an appropriate error message.

---

### US-003 — Validate Vehicle

**As a** customer  
**I want the system to** validate my vehicle information  
**So that** I can receive an accurate quotation.

#### Acceptance Criteria

**AC-001**

Given valid vehicle information is entered  
When the system validates the vehicle  
Then the vehicle should be marked as valid.

**AC-002**

Given the vehicle cannot be found  
When validation is performed  
Then the system should return `VEHICLE_NOT_FOUND`.

**AC-003**

Given the external vehicle service is unavailable  
When validation is attempted  
Then the system should inform the customer that the service is temporarily unavailable.

---

# Epic 2 — Quotation

## US-004 — Select Insurance Product

**As a** customer  
**I want to** select an insurance product  
**So that** I can receive a quotation for the coverage I need.

#### Acceptance Criteria

- Available products should be displayed.
- Unavailable products should not be selectable.
- The selected product should be associated with the quotation request.

---

## US-005 — Generate Quotation

**As a** customer  
**I want to** request an insurance quotation  
**So that** I can see the premium before purchasing the policy.

#### Acceptance Criteria

**AC-001**

Given valid customer and vehicle information  
When the customer requests a quotation  
Then the system should calculate the premium.

**AC-002**

The system should generate a unique quotation ID.

**AC-003**

The quotation should contain:

- Product
- Vehicle
- Premium
- Currency
- Validity date

**AC-004**

If quotation calculation fails, the system should return an appropriate error.

---

## US-006 — Review Quotation

**As a** customer  
**I want to** review my quotation  
**So that** I can decide whether to continue with the insurance purchase.

#### Acceptance Criteria

The quotation page should display:

- Quote ID
- Insurance product
- Vehicle
- Coverage
- Premium
- Discounts
- Taxes
- Total amount
- Validity date

---

# Epic 3 — Insurance Application

## US-007 — Create Application

**As a** customer  
**I want to** create an insurance application from my quotation  
**So that** I can proceed with purchasing the policy.

#### Acceptance Criteria

- The quotation must exist.
- The quotation must be valid.
- The quotation must not be expired.
- A unique application ID must be generated.
- Application status should initially be `PENDING_PAYMENT`.

---

# Epic 4 — Payment

## US-008 — Complete Payment

**As a** customer  
**I want to** pay for my insurance application  
**So that** my insurance policy can be issued.

#### Acceptance Criteria

**Successful Payment**

Given the application is valid  
When the payment is completed successfully  
Then the payment status should become `SUCCESS`  
And the application should proceed to policy creation.

**Failed Payment**

Given the payment provider rejects the transaction  
When payment processing is completed  
Then the payment status should become `FAILED`  
And the policy must not be activated.

---

# Epic 5 — Policy

## US-009 — Create Policy

**As an** insurance operations user  
**I want the system to** create a policy after successful payment  
**So that** the customer can receive active insurance coverage.

#### Acceptance Criteria

- Successful payment must exist.
- A unique policy number must be generated.
- Customer and vehicle information must be associated with the policy.
- Policy status should become `ACTIVE` after successful policy creation.

---

## US-010 — View Policy

**As a** customer  
**I want to** view my policy information  
**So that** I can access my insurance details.

#### Acceptance Criteria

The customer should be able to view:

- Policy number
- Product
- Vehicle
- Coverage
- Premium
- Start date
- End date
- Policy status

---

# Epic 6 — Error Handling

## US-011 — Handle Integration Failure

**As a** customer  
**I want the system to** provide a meaningful message when an external service fails  
**So that** I understand what happened and what I should do next.

#### Acceptance Criteria

- External service failures must be logged.
- The customer should not receive technical stack traces.
- A meaningful error message should be displayed.
- The transaction should not create an invalid policy.
- The transaction should be traceable using a unique transaction ID.

---

# Epic 7 — Audit & Traceability

## US-012 — Track Insurance Transactions

**As an** operations user  
**I want to** trace quotation, application, payment and policy transactions  
**So that** I can investigate customer issues and operational incidents.

#### Acceptance Criteria

The system should maintain relationships between:

```text
Customer
   ↓
Quote
   ↓
Application
   ↓
Payment
   ↓
Policy
```

Each transaction should have a unique identifier.

---

# Story Mapping

```text
CUSTOMER JOURNEY

Enter Information
       ↓
Validate Vehicle
       ↓
Select Product
       ↓
Get Quote
       ↓
Review Quote
       ↓
Create Application
       ↓
Make Payment
       ↓
Create Policy
       ↓
View Policy
```

---

# Definition of Done

A user story can be considered **Done** when:

- Business requirements are understood.
- Acceptance criteria are met.
- Development is completed.
- Code review is completed where applicable.
- Functional testing is completed.
- API testing is completed where applicable.
- No critical or blocker defects remain.
- UAT criteria are satisfied.
- Required documentation is updated.
