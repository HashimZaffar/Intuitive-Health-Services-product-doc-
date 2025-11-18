
---

# **Intuitive Health Services–Technical Architecture Document (TAD)**

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

# **1. Overview**

This Technical Architecture Document outlines the **end-to-end system design**, **platform components**, **data flow**, and **technology strategy** for the Intuitive Health Services Locum Staffing Platform.

The goal is to ensure:

* Scalability
* Security
* Maintainability
* High availability
* Compliance (HIPAA-aligned where applicable)

This TAD supports engineering planning, technical decision-making, and cross-team alignment for the platform’s development.

---

# **2. Architecture Goals**

### **2.1 Primary Goals**

* Build a **modular**, **service-oriented**, cloud-hosted staffing platform.
* Support **high-volume candidate onboarding**, **credential storage**, and **job-matching workflows**.
* Maintain strong **security and compliance controls** for sensitive documents.
* Enable smooth **scaling** across multiple states and facility types.
* Ensure **system observability** with monitoring and analytics.

### **2.2 Key Constraints**

* Must support HIPAA-aligned security for personal and medical documents.
* Must handle growth from ~1K to ~20K active professionals.
* Must support 50+ facility partners across different states.
* Should integrate with external license verification APIs (phase 2+).

---

# **3. System Architecture Summary**

The Intuitive Platform will use a **cloud-native, microservices-aligned architecture** with clear separation of concerns:

### **Core Layers**

1. **Frontend Layer**
2. **Backend API Layer**
3. **Microservices Layer**
4. **Database & Storage Layer**
5. **Notification Layer**
6. **Security & Compliance Layer**
7. **Admin & Analytics Layer**

---

# **4. High-Level Architecture Diagram**

```
+--------------------------------------------------------------------------+
|                             Client Devices                               |
|  (Web browsers, Tablets, Mobile—Responsive Web Only for MVP)            |
+--------------------------------------------------------------------------+
                   |              |                 |
                   v              v                 v
+---------------------------------------------------------------+
|                        Frontend Layer                         |
|  React.js / Next.js Web Portal (Candidates, Facilities, Admin)|
+---------------------------------------------------------------+
                   |                     |
                   v                     v
+---------------------------------------------------------------+
|                        Backend API Layer                      |
|   Node.js / Express REST APIs (Role-based, Secure, Versioned) |
+---------------------------------------------------------------+
                   |                     |                     |
                   v                     v                     v
+-------------+        +------------------+         +--------------------+
|  User Mgmt  |        |   Job Service    |         |  Assignment Service |
| Microservice|        | Microservice     |         |  Microservice       |
+-------------+        +------------------+         +--------------------+
         |                       |                            |
         v                       v                            v
+------------------------------------------------------------------------+
|                   Database & Document Storage Layer                    |
| RDS/PostgreSQL (Users, Jobs, Licenses, Requests)                       |
| S3 Bucket (Credential Files, Resumes, Compliance Docs)                 |
+------------------------------------------------------------------------+
         |                       |                            |
         v                       v                            v
+------------------+  +---------------------+  +---------------------------+
| Notification Svc |  | Analytics & Logging |  | Compliance & Security Svc |
+------------------+  +---------------------+  +---------------------------+
```

---

# **5. Detailed Component Breakdown**

## **5.1 Frontend Layer**

**Tech Recommendation:** React.js / Next.js
**Users Supported:** Candidates, Facility Admins, Recruiters, Internal Admins

### Features:

* Job browsing and filtering
* Profile creation and resume upload
* Staffing request submission
* Recruiter dashboards
* Credentialing workflows
* Admin analytics panel

### Requirements:

* Fully responsive
* WCAG 2.1 AA accessibility
* OAuth2 / JWT token authentication

---

## **5.2 Backend API Layer**

**Tech Recommendation:** Node.js + Express or NestJS
**Pattern:** RESTful API (GraphQL optional in future)

Responsibilities:

* Authentication & authorization
* User identity management
* Candidate-job matching APIs
* Facility request management
* CRUD for all resources
* Credential upload endpoints
* Assignment lifecycle management

### API Standards:

* Versioned endpoints
* Standardized error handling
* Role-based access filtering
* Encrypted payloads over HTTPS

---

## **5.3 Microservices Layer**

### **User Management Microservice**

* Authentication
* Roles & permissions (RBAC)
* Profile data & storage

### **Job Management Microservice**

* Job posting CRUD
* Job filtering
* Search ranking logic
* Candidate-application tracking

### **Credentialing Microservice**

* License uploads
* Verification tracking
* Expiration reminders
* Document access permissions

### **Assignment Management Microservice**

* Schedule handling
* Status updates
* Onboarding workflows

### **Notification Microservice**

* Email, SMS, in-app alerts
* Retry queues
* Template management

---

# **6. Data Architecture**

## **6.1 Database Strategy**

* **Primary Database:** PostgreSQL (AWS RDS)
* **Document Storage:** AWS S3 with restricted buckets
* **Search Indexing:** Elasticsearch (Phase 2)

### **Key Entities**

* Users
* Profiles
* Jobs
* Applications
* Facilities
* Staffing Requests
* Credentials
* Assignments
* Notifications

---

## **6.2 Sample Entity-Relationship Overview**

```
Users 1—1 Profiles
Users 1—Many Credentials
Users 1—Many Applications
Jobs 1—Many Applications
Facilities 1—Many Staffing Requests
Staffing Requests 1—Many Assignments
```

---

# **7. Security & Compliance Architecture**

### **Security Controls**

* JWT authentication
* Role-Based Access Control
* OAuth2 for admin tools
* Encrypted S3 document storage
* AES-256 encryption at rest
* TLS 1.2+ encryption in transit
* Audit logs for all credential access

### **Compliance Requirements**

* HIPAA-aligned document handling
* Multi-state medical licensing rules
* Data retention policies
* Periodic penetration testing

---

# **8. Integration Architecture**

### **Internal Integrations**

* Candidate-job matching engine
* License verification modules
* Automated email/SMS notifications
* Admin reporting dashboards

### **External Integrations (Future Phases)**

* State medical license APIs
* Background check providers
* Scheduling systems (correctional facilities)
* Billing platforms (phase 3)

---

# **9. Infrastructure Architecture**

### **Hosting**

* AWS EC2 or AWS Elastic Beanstalk
* Auto-scaling groups for backend APIs
* Load balancers for traffic routing

### **Storage**

* S3 for document storage
* RDS PostgreSQL for relational data

### **Caching**

* Redis for session and query caching

### **CI/CD**

* GitHub Actions or GitLab CI
* Automated test & deployment pipelines

---

# **10. Observability & Monitoring**

### **Logging**

* CloudWatch
* Centralized log aggregation

### **Metrics**

* API performance
* DB connection usage
* Job posting activity
* Candidate onboarding rate
* Assignment fill rates

### **Alerting**

* PagerDuty or Opsgenie
* High error rate alerts
* Credential expiration warnings

---

# **11. Performance & Scalability Requirements**

| Requirement                 | Target              |
| --------------------------- | ------------------- |
| Job search response         | ≤ 1.5 seconds       |
| Candidate upload throughput | 50 uploads/min      |
| Concurrent users            | 1,000+              |
| API uptime                  | 99.9%               |
| Notifications               | 5,000/hour capacity |

---

# **12. Risks & Mitigation**

| Risk                     | Impact                   | Mitigation               |
| ------------------------ | ------------------------ | ------------------------ |
| Large file uploads       | High storage + bandwidth | Size limits, compression |
| License data sensitivity | Compliance violations    | Strict ACLs, encryption  |
| High request volume      | API strain               | Auto-scaling, caching    |
| Manual verification lag  | Delayed placement        | Automated reminders      |

---

# **13. Future Architectural Enhancements**

* AI-driven job matching engine
* Real-time chat between recruiters and candidates
* Full mobile app architecture
* Automated payroll & timesheet module
* Multi-region AWS deployment
* Advanced analytics with Redshift or BigQuery

---

# **14. Appendices**

### **A. Glossary**

| Term | Meaning                       |
| ---- | ----------------------------- |
| RBAC | Role-Based Access Control     |
| S3   | Amazon Simple Storage Service |
| RDS  | Relational Database Service   |
| ATS  | Applicant Tracking System     |

### **B. Related Documents**

* Product Requirements Document (PRD)
* QA Test Plan (to be created)
* API Specification Document
* Data Privacy Policy
* User Manual & Admin Guide
