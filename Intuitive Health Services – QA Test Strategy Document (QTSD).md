
---

# **Intuitive Health Services–QA Test Strategy Document (QTSD)**

**Version 1.0 – Initial Draft**

---

## **Document Control**

| Field                     | Detail                                            |
| ------------------------- | ------------------------------------------------- |
| **Project Name**          | Intuitive Health Services Locum Staffing Platform |
| **Document Version**      | 1.0                                               |
| **Last Updated**          | November 2025                                     |
| **Author**                | Hashim Zaffar                                     |
| **Reviewed By**           | —                                                 |
| **Approved By**           | —                                                 |
| **Document Status**       | Draft                                             |
| **Confidentiality Level** | Internal                                          |

---

## **Document Revision History**

| Version | Date          | Author        | Description of Change |
| ------- | ------------- | ------------- | --------------------- |
| 1.0     | November 2025 | Hashim Zaffar | Initial draft         |

---

# **1. Introduction**

The purpose of this **QA Test Strategy Document (QTSD)** is to define the testing framework, quality standards, tools, and processes required to ensure the successful delivery of the Intuitive Health Services Locum Staffing Platform.

This strategy ensures:

* Functional correctness
* Performance and scalability
* Security and compliance
* Accessibility
* Reliability under peak load
* High-quality user experience for candidates, facilities, and internal teams

The document guides QA Analysts, Automation Engineers, Developers, and Product Owners.

---

# **2. Testing Objectives**

### **Primary Objectives**

* Validate all functional requirements as specified in the PRD.
* Ensure compliance with HIPAA-aligned data handling practices.
* Confirm system performance under peak load across job search, uploads, and application flows.
* Test the reliability of credential document storage and access permissions.
* Validate workflows for all user roles: Candidate, Facility Admin, Recruiter, Credential Specialist, and System Admin.
* Ensure system resilience and graceful error handling.
* Establish automated test coverage for core modules to support continuous deployment.

---

# **3. Testing Scope**

### **3.1 In-Scope (MVP)**

* Candidate registration, login, and profile creation
* Job marketplace (search, filter, apply)
* Facility staffing request workflow
* Recruiter dashboard workflows
* Credential upload, preview, and verification
* Assignment scheduling
* Email/SMS notifications
* Admin portal and role-based access
* API response correctness
* Database integrity
* S3 storage access permissions

### **3.2 Out-of-Scope (MVP)**

* Mobile native applications
* AI matching logic (Phase 2)
* External integrations (State License API, Payroll, EMR/EHR)
* Advanced analytics dashboards

---

# **4. Test Levels**

## **4.1 Unit Testing**

Performed by: **Developers**
Tools: Jest, Mocha, or Jasmine (based on Node.js stack)

Covers:

* API endpoints
* Utility functions
* Validation schemas
* Microservices logic

## **4.2 Integration Testing**

Performed by: **QA + Developers**
Tests interactions between:

* Job Service ↔ User Service
* Credentialing Service ↔ S3 Storage
* Facility Requests ↔ Assignment Module
* Notification Service ↔ Email/SMS provider

Tools: Supertest, Postman/Newman, Docker test containers

## **4.3 System Testing**

Performed by: **QA Team**

Validates complete business flows:

* Candidate → Apply → Recruiter → Scheduled Assignment
* Facility Admin → Request → Recruiter → Fulfillment
* Credential Upload → Verification → Assignment Approval

Includes:

* Functional tests
* Role-based access
* UI/UX validation

## **4.4 Regression Testing**

Performed every sprint to ensure:

* No new changes break existing modules
* Core candidate workflows stay intact

## **4.5 Performance Testing**

Performed using: JMeter, Locust

Metrics Tested:

* Job search load response
* File upload latency
* Concurrent logins (1,000+ users)
* Notification throughput (emails/SMS)

## **4.6 Security Testing**

Includes:

* Penetration tests
* Role-based access checks
* Unauthorized API access attempts
* S3 bucket permission testing
* Data encryption validation

Tools: OWASP ZAP, Burp Suite

## **4.7 Accessibility Testing**

* WCAG 2.1 AA compliance
* Screen reader support
* Keyboard navigation
* Color contrast analysis

Tools: Axe, Lighthouse

---

# **5. Test Types**

### **Functional Testing**

* CRUD operations for Users, Jobs, Credentials
* Form validations
* Workflow navigation

### **Non-Functional Testing**

* Load Handling
* Failover recovery
* Response time benchmarks

### **Usability Testing**

* Candidate profile creation ease
* Job filtering clarity
* Facility request simplicity

### **Compatibility Testing**

* Chrome, Edge, Safari, Firefox
* Laptop, tablet, mobile (responsive only)

---

# **6. Test Environments**

| Environment  | Purpose                                          |
| ------------ | ------------------------------------------------ |
| **DEV**      | Developer integration environment                |
| **QA / SIT** | Primary testing environment — stable builds only |
| **UAT**      | User acceptance and stakeholder verification     |
| **STAGE**    | Pre-production                                   |
| **PROD**     | Live environment (post-go-live monitoring)       |

### Environment Requirements:

* Mirrored configuration to production
* Isolated credential storage buckets
* Test data anonymization
* Daily database refresh (for QA/UAT)

---

# **7. Test Data Management**

### Test Data Categories:

* Candidate profiles (various disciplines)
* Job listings (CA + NY focus)
* Credential files (dummy PDFs, JPEGs)
* Facility accounts and staffing requests

### Data Policies:

* No real PHI/PII in test data
* All uploads must be synthetic
* Access logs must be reviewed weekly

---

# **8. Defect Management**

### Tool: Jira or Azure DevOps

### Workflow:

```
New → In Review → Assigned → In Progress → QA Validate → Closed
```

### Defect Severity Levels:

* **Critical**: Platform unusable or security breach
* **High**: Core workflow broken
* **Medium**: Non-critical features broken
* **Low**: UI issues or cosmetic defects

---

# **9. Acceptance Criteria**

The platform will be considered "Ready for UAT" when:

* 95% of test cases pass
* No open Critical or High defects
* All user roles validated end-to-end
* API success rate ≥ 99%
* Job marketplace stable under load
* Credential storage validated
* Notifications delivered successfully
* Accessibility score ≥ 85 (Lighthouse)

The platform will be considered "Ready for Production" when:

* All UAT feedback is addressed
* No open High/Medium defects
* Performance metrics achieved
* Security assessment passed
* Disaster recovery tested and successful

---

# **10. QA Reporting**

Weekly QA reports will include:

* Pass/fail summary
* Defect trends
* Stability index
* Deployment quality score
* Requirement coverage map
* Automation coverage percentage

Monthly QA reports will include:

* Performance comparison
* Security posture snapshot
* Accessibility score changes
* Release quality score

---

# **11. Automation Strategy**

### **Scope (MVP)**

Automate:

* Login
* Profile creation
* Job search
* Job application
* Facility staffing request submission
* Recruiter application review

### **Tools**

* Selenium WebDriver
* Cypress (preferred for web UI)
* Postman/Newman for API automation
* Jest for backend unit tests

### **Goals**

* Achieve **40% automation coverage** by MVP
* Achieve **75% automation coverage** by Phase 2

---

# **12. Risks & Mitigations**

| Risk                                    | Impact | Mitigation                               |
| --------------------------------------- | ------ | ---------------------------------------- |
| Large PDF uploads causing test failures | High   | Add file compression and size limits     |
| High data sensitivity (licenses)        | High   | Mask data and enforce strict role tests  |
| Unsupported browsers                    | Medium | Enforce Chrome/Edge support for MVP      |
| Test environment instability            | High   | Automated environment health checks      |
| Manual verification slowing QA          | Medium | Introduce mock services for license APIs |

---

# **13. Future QA Enhancements**

* Contract testing using Pact
* AI-powered test case generation
* Full mobile testing suite
* Chaos engineering for resiliency testing
* Automated accessibility remediation suggestions

---

# **14. Appendices**

### **A. Test Case Categories**

* Authentication
* Candidate workflows
* Facility workflows
* Assignment workflows
* Credentialing
* Admin controls
* Notifications
* API response validation
* Error handling
* Performance

### **B. Related Documents**

* PRD
* Technical Architecture Document
* API Specification (next document if you want)
* Data Security Policy
* Release Management Plan