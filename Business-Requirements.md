# Business Requirements Document (BRD)

## 1. Document Information

| Field | Description |
|---|---|
| Project | Digital Insurance Platform |
| Document | Business Requirements Document |
| Version | 1.0 |
| Status | Draft |
| Domain | Motor Insurance |
| Business Area | Digital Insurance |

---

## 2. Business Objective

The objective of the Digital Insurance Platform is to provide customers with a digital channel through which they can obtain a motor insurance quotation and complete the insurance purchasing process without requiring manual intervention.

The solution aims to:

- Reduce manual operations
- Improve customer experience
- Reduce quotation and policy issuance time
- Provide consistent premium calculation
- Enable digital payment
- Minimize data entry errors
- Provide traceability throughout the insurance lifecycle

---

## 3. Business Problem

The traditional insurance purchasing process may involve multiple manual steps, including customer information collection, vehicle information verification, quotation preparation, payment processing and policy issuance.

These processes can result in:

- Longer processing times
- Manual data-entry errors
- Inconsistent customer experience
- Increased operational workload
- Limited process visibility

The proposed platform addresses these challenges by providing an integrated digital journey.

---

## 4. Stakeholders

| Stakeholder | Responsibility / Interest |
|---|---|
| Customer | Obtains quotation and purchases insurance |
| Sales Team | Monitors sales and customer applications |
| Underwriting Team | Defines insurance rules and risk criteria |
| Operations Team | Manages policy-related operations |
| IT Team | Develops and maintains the platform |
| QA Team | Validates system quality |
| Payment Provider | Processes customer payments |
| Vehicle Data Provider | Provides vehicle information |

---

## 5. Business Requirements

### BR-001 — Customer Registration

The system shall allow customers to provide the information required to initiate an insurance quotation.

### BR-002 — Vehicle Information

The system shall allow customers to provide vehicle information required for quotation calculation.

### BR-003 — Vehicle Validation

The system shall validate vehicle information before proceeding with quotation calculation.

### BR-004 — Insurance Product Selection

The customer shall be able to select an available motor insurance product.

### BR-005 — Quotation

The system shall generate an insurance quotation based on customer, vehicle and product information.

### BR-006 — Premium Calculation

The system shall calculate the insurance premium according to predefined business rules.

### BR-007 — Quotation Review

The customer shall be able to review quotation details before starting the application process.

### BR-008 — Application

The customer shall be able to create an insurance application from a valid quotation.

### BR-009 — Payment

The platform shall allow the customer to complete payment through an integrated payment service.

### BR-010 — Policy Creation

The system shall create an insurance policy after successful payment.

### BR-011 — Policy Activation

The newly created policy shall become active only after all required business and payment validations are completed successfully.

### BR-012 — Error Handling

The system shall provide meaningful error messages when a business or technical validation fails.

### BR-013 — Traceability

The platform shall maintain traceability between customer, quotation, application, payment and policy records.

---

## 6. Business Rules

### BRULE-001 — Vehicle Information

A quotation cannot be generated without valid vehicle information.

### BRULE-002 — Vehicle Model Year

The vehicle model year must be within the range supported by the selected insurance product.

### BRULE-003 — Quotation Validity

A quotation shall have a defined validity period.

### BRULE-004 — Payment

A policy cannot become active when the required payment has not been successfully completed.

### BRULE-005 — Duplicate Policy

The system shall prevent creation of duplicate active policies for the same customer and vehicle when business rules do not permit duplication.

### BRULE-006 — Premium

The calculated premium must be greater than zero.

### BRULE-007 — Policy Number

Every successfully created policy must have a unique policy number.

---

## 7. Business Success Criteria

The project will be considered successful when:

- Customers can complete the quotation journey digitally.
- Valid quotations are generated successfully.
- Invalid data is rejected appropriately.
- Customers can complete payment.
- Policies are created after successful payment.
- The complete customer journey can be traced.
- Business and technical errors are handled appropriately.

---

## 8. Assumptions

The following assumptions apply:

- External vehicle data services are available.
- A payment provider is available through an API.
- Insurance product and pricing rules are maintained by the business.
- Customers provide valid personal information.
- Authentication is handled by the digital channel.

---

## 9. Out of Scope

The following items are outside the scope of this case study:

- Real payment processing
- Real customer personal data
- Real insurance company integrations
- Production deployment
- Real underwriting engines
- Real-time external vehicle databases

---

## 10. Business Process

```text
Customer
   ↓
Enter Vehicle Information
   ↓
Validate Vehicle
   ↓
Select Insurance Product
   ↓
Generate Quotation
   ↓
Review Quotation
   ↓
Create Application
   ↓
Make Payment
   ↓
Create Policy
   ↓
Activate Policy
```
