
---
# **Intuitive Health Services – API Specification Document (APISD)**

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

This API Specification Document details the **RESTful API architecture** for the Intuitive Health Services Locum Staffing Platform. It defines:

* Endpoints
* Request/response structures
* Authentication and authorization rules
* Data models
* Error codes
* Versioning strategy
* Best practices for integration

This specification ensures the platform remains **scalable**, **secure**, **consistent**, and **easy to integrate** across multiple services and clients.

---

# **2. API Design Principles**

### **2.1 RESTful Architecture**

All APIs follow RESTful principles:

* Resource-based endpoints
* Stateless communication
* Standard HTTP methods

### **2.2 JSON Format**

Request and response bodies use JSON.

### **2.3 Standard HTTP Status Codes**

* 200: Success
* 201: Created
* 400: Bad Request
* 401: Unauthorized
* 403: Forbidden
* 404: Not Found
* 500: Server Error

### **2.4 Pagination Defaults**

Endpoints returning lists use:

```
?limit=20&offset=0
```

### **2.5 URL Structure**

All endpoints start with:

```
/api/v1/
```

### **2.6 Authentication**

* JWT-based authentication
* OAuth2 (Admin tools) – Phase 2
* All admin endpoints require RBAC

---

# **3. Authentication & Authorization**

## **3.1 Login**

**POST /api/v1/auth/login**

### Request

```json
{
  "email": "candidate@example.com",
  "password": "Password123"
}
```

### Response

```json
{
  "token": "jwt-token-string",
  "expires_in": 3600,
  "role": "candidate"
}
```

## **3.2 Access Control**

Roles include:

* candidate
* facility_admin
* recruiter
* credential_specialist
* admin

APIs must validate role-based access using:

```
X-User-Role
X-User-ID
Authorization: Bearer <JWT>
```

---

# **4. Users & Profiles API**

## **4.1 Create User**

**POST /api/v1/users**

### Request

```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "Secret123",
  "role": "candidate"
}
```

### Response

```json
{
  "id": "u_12345",
  "email": "john@example.com",
  "role": "candidate",
  "created_at": "2025-11-01T10:00:00Z"
}
```

---

## **4.2 Get User Profile**

**GET /api/v1/users/{user_id}**

### Response

```json
{
  "id": "u_12345",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "role": "candidate",
  "discipline": "Psychiatrist",
  "location": "California",
  "license_status": "Pending Verification"
}
```

---

## **4.3 Update Profile**

**PUT /api/v1/users/{user_id}**

```json
{
  "discipline": "Registered Nurse",
  "phone": "555-123-4567"
}
```

---

# **5. Jobs API**

## **5.1 List All Jobs**

**GET /api/v1/jobs**

### Query Parameters

```
?discipline=RN
?location=California
?sort=posted_at
?limit=20&offset=0
```

### Response

```json
{
  "total": 128,
  "jobs": [
    {
      "id": "job_987",
      "title": "Registered Nurse - State Hospital",
      "discipline": "RN",
      "location": "Fresno, CA",
      "pay_rate": "Hourly",
      "status": "Open"
    }
  ]
}
```

---

## **5.2 Get Job Details**

**GET /api/v1/jobs/{job_id}**

```json
{
  "id": "job_987",
  "title": "Registered Nurse",
  "facility_type": "State Hospital",
  "description": "Provide patient care...",
  "schedule": "M–F, 8am–4pm",
  "qualifications": ["Active RN license", "1+ year experience"],
  "posted_at": "2025-10-20T12:00:00Z"
}
```

---

## **5.3 Apply for a Job**

**POST /api/v1/jobs/{job_id}/apply**

### Request

```json
{
  "candidate_id": "u_12345"
}
```

### Response

```json
{
  "application_id": "app_5566",
  "status": "Submitted"
}
```

---

# **6. Facility Requests API**

## **6.1 Submit Staffing Request**

**POST /api/v1/facilities/{facility_id}/requests**

```json
{
  "discipline": "Psychiatrist",
  "location": "Los Angeles, CA",
  "shift_type": "Full-time",
  "start_date": "2025-12-01",
  "notes": "Correctional facility experience preferred"
}
```

---

## **6.2 List Requests**

**GET /api/v1/facilities/{facility_id}/requests**

---

## **6.3 Update Request Status**

**PATCH /api/v1/requests/{request_id}**

```json
{
  "status": "In Progress"
}
```

---

# **7. Credential Management API**

## **7.1 Upload Credential Document**

**POST /api/v1/credentials**

Multipart Form Data:

```
file: <pdf/jpeg>
type: "RN License"
user_id: "u_12345"
```

### Response

```json
{
  "id": "cred_8787",
  "type": "RN License",
  "status": "Pending Review"
}
```

---

## **7.2 Approve or Reject Credential**

**PATCH /api/v1/credentials/{credential_id}**

```json
{
  "status": "Verified"
}
```

---

# **8. Assignment API**

## **8.1 Create Assignment**

**POST /api/v1/assignments**

```json
{
  "candidate_id": "u_12345",
  "job_id": "job_987",
  "start_date": "2025-12-05",
  "end_date": "2026-01-30"
}
```

---

## **8.2 Update Assignment Status**

**PATCH /api/v1/assignments/{assignment_id}**

```json
{
  "status": "Active"
}
```

---

# **9. Notifications API**

## **9.1 Send Notification**

**POST /api/v1/notifications**

```json
{
  "recipient_id": "u_12345",
  "type": "email",
  "subject": "Assignment Update",
  "message": "Your assignment has been approved."
}
```

---

# **10. Error Codes**

| Code    | Meaning           | Description               |
| ------- | ----------------- | ------------------------- |
| ERR-100 | Validation Error  | Missing or invalid field  |
| ERR-101 | Unauthorized      | Invalid token             |
| ERR-102 | Forbidden         | Role not allowed          |
| ERR-200 | Not Found         | Resource missing          |
| ERR-300 | File Upload Error | Invalid or oversized file |
| ERR-500 | Server Error      | Unexpected exception      |

### Standard Error Response

```json
{
  "error_code": "ERR-100",
  "message": "Discipline is required."
}
```

---

# **11. API Rate Limits**

| User Type       | Requests Per Minute |
| --------------- | ------------------- |
| Candidate       | 100                 |
| Facility Admin  | 120                 |
| Recruiter/Admin | 300                 |
| System/Service  | 500                 |

429 errors must include `Retry-After` header.

---

# **12. Future Enhancements**

* AI-based candidate-job matching API
* License verification API integrations
* Telehealth scheduling API
* Webhooks for job updates
* Multi-language responses (Phase 3)

---

# **13. Appendices**

### **A. Resource Naming Conventions**

* Use snake_case IDs
* Use plural nouns for endpoints
* Use `/sub-resources` for related entities

### **B. Related Documents**

* PRD
* Technical Architecture Document
* QA Test Strategy Document
* Data Privacy Policy