
---

# **Intuitive Health Services Locum Staffing Platform**

## **Overview**

The **Intuitive Health Services Locum Staffing Platform** is a modern, cloud-native system designed to streamline the full lifecycle of locum tenens staffing across the United States — with a primary focus on California and New York.

It enables:

* Healthcare professionals to **discover, apply for, and manage locum opportunities**
* Facilities (hospitals, clinics, correctional centers) to **submit staffing requests**
* Internal teams to handle **recruitment, credentialing, matching, and assignments**
* Secure storage and verification of **licenses, certifications, and compliance documents**

The platform aims to eliminate manual workflows and replace them with a **unified, secure, data-driven staffing ecosystem**.

---

## **Key Features**

### **For Healthcare Professionals**

* Create and manage professional profiles
* Upload licenses, certifications, and compliance docs
* Browse and apply for open jobs
* Track application and assignment status
* Receive notifications for interviews, offers, and renewals

### **For Facilities**

* Submit locum staffing requests
* Track request progress
* Communicate with recruiters
* Receive assignment confirmations

### **For Recruiters & Internal Teams**

* Review candidate applications
* Validate credentials
* Manage staffing requests
* Create and manage assignments
* Internal dashboards for tracking workload

### **Platform-Level Features**

* Role-based access control (RBAC)
* Secure document storage (HIPAA-aligned)
* Notifications: email, SMS, in-app
* Scalable microservice-friendly backend
* Centralized analytics and reporting

---

## **Tech Stack**

### **Frontend**

* React.js / Next.js
* Tailwind or Material UI
* JWT-based authentication

### **Backend**

* Node.js (Express or NestJS)
* RESTful API architecture
* Microservices for Users, Jobs, Credentials, Assignments, Notifications

### **Database & Storage**

* PostgreSQL (AWS RDS)
* Amazon S3 for file storage
* Redis for caching

### **Infrastructure**

* AWS (EC2 / ECS / Elastic Beanstalk)
* CI/CD via GitHub Actions or GitLab CI
* CloudWatch for logs and monitoring

---

## **Architecture**

A high-level architecture flow:

```
Client Browser
      |
      v
   Frontend
      |
      v
REST API Layer (Node.js)
      |
      +--------+
      |        |
Microservices  |
      |        |
      v        v
PostgreSQL   AWS S3
```

More detailed diagrams (ERD, sequence, activity) are available in the `/docs/uml/` folder.

---

## **Core Modules**

| Module                   | Description                                      |
| ------------------------ | ------------------------------------------------ |
| **Auth Service**         | JWT login, registration, role management         |
| **User Service**         | Candidate and facility profile management        |
| **Job Service**          | Job posting, filtering, and application workflow |
| **Application Service**  | Candidate-job communication and status tracking  |
| **Credential Service**   | Secure upload, verification, and tracking        |
| **Assignment Service**   | Scheduling and end-to-end assignment workflow    |
| **Notification Service** | Email, SMS, and in-app alerts                    |

---

## **API Documentation**

All API endpoints follow this base:

```
/api/v1/
```

API specification is available in the **API Specification Document** under `/docs/api/`.

---

## **Environment Setup**

### **Prerequisites**

* Node.js 18+
* PostgreSQL 14+
* AWS account with S3 access (optional for local dev)
* Git

### **Setup Steps**

1. Clone the repository:

```bash
git clone https://github.com/intuitive-health-services/platform.git
cd platform
```

2. Install dependencies:

```bash
npm install
```

3. Create environment variables:

```bash
cp .env.example .env
```

4. Run database migrations:

```bash
npm run migrate
```

5. Start development server:

```bash
npm run dev
```

---

## **Project Structure**

```
/src
  /api
  /services
  /models
  /controllers
  /middleware
  /utils

/docs
  PRD.pdf
  Technical_Architecture.pdf
  QA_Strategy.pdf
  API_Specification.pdf
  UML_Diagrams/
    ERD.md
    Sequence.md
    Activity.md

/frontend
  /components
  /pages
  /hooks
  /styles
```

---

## **Testing**

Quality Assurance is implemented based on the **QA Test Strategy Document**.

### **Testing Tools**

* Jest (unit tests)
* Supertest (API tests)
* Cypress (UI tests)
* JMeter / Locust (performance tests)

Run automated tests:

```bash
npm run test
```

---

## **Security**

The platform enforces:

* Encrypted storage (AES-256)
* TLS 1.2+ HTTPS
* Strict RBAC
* Secure AWS S3 policies
* Audit logs for credential access
* HIPAA-aligned workflows

---

## **Deployment**

Deploy using:

```bash
npm run build
npm run start
```

Or using automated CI/CD pipelines on:

* GitHub Actions
* GitLab CI
* AWS CodePipeline

---

## **Planned Enhancements**

* AI-based job matching
* Real-time recruiter–candidate chat
* Mobile app (iOS/Android)
* Automated license verification via state APIs
* Advanced analytics dashboard
* Facility staffing forecast engine

---

## **Support**

For technical support, contact:

**Email:** [jobs@intuitivehealthservices.com](mailto:jobs@intuitivehealthservices.com)
**Phone:** (559) 796-5853
**Phone:** (805) 703-3729

For platform issues: open a ticket under **/support** in this repository.

---

## **License**

© 2025 Intuitive Health Services. All rights reserved.
Unauthorized copying, distribution, or modification is prohibited.