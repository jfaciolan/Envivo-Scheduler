# **PRODUCT REQUIREMENTS DOCUMENT**

**Project:** Envivo Web-Based Scheduling System  
**Client:** Envivo Dialysis Center - Bacolod City  
**Prepared For:** Carissa Dumancas, Chief Executive Officer  
**Prepared By:** John Rey Faciolan, Analytics and Automation Consultant  
**Date:** February 2, 2026  
**Version:** 1.0 - Final for Stakeholder Approval  

---

## **1. INTRODUCTION & PURPOSE**

Envivo is the premier dialysis provider in Bacolod City, currently operating 15 dialysis machines and planning to expand to 30 machines by August 2026. Patient scheduling is currently managed manually using Google Calendar, which has become unsustainable and error-prone as operations scale.

This document defines the requirements for a web-based scheduling system designed to streamline patient scheduling, improve operational visibility, and support future expansion. The system will replace the current manual process with a centralized, rules-based scheduling platform accessible to receptionists and scheduling staff.

---

## **2. TARGET AUDIENCE & USER PERSONAS**

| Role | Permissions | Access Scope |
|------|-------------|--------------|
| **Scheduler** | Create, read, update, and delete schedules; update machine status | Bacolod Branch (MVP only) |
| **Schedule Viewer** | Read-only access to schedules | Bacolod Branch (MVP only) |

> All users access a unified system. Data isolation between future branches will be managed via a `branch_id` database field.

---

## **3. FEATURES & FUNCTIONALITY (MoSCoW PRIORITIZATION)**

| Feature                                  | Priority        | Description                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dashboard – Visual Schedule Overview** | **Must Have**   | A visual grid interface displaying scheduled patients across machines, days, and shifts using color coding. Colors represent: <br>• **Green**: Available slot <br>• **Blue**: Scheduled patient <br>• **Red**: Machine marked as "Not Available" <br>Each scheduled slot displays patient initials or abbreviated name. Hover reveals full patient details.                                                       |
| **Scheduler – Manual Patient Booking**   | **Must Have**   | Interface to schedule a patient by selecting: patient → day group (A/B) → shift (A/B/C) → machine. <br><br>**Availability Logic:** <br>• Only day group + shift + machine combinations with available slots are shown. <br>• Fully booked machines are excluded from selection. <br>• Selection of already-booked slots is prevented. <br>• Availability is based on existing scheduled bookings (not real-time). |
| **Patient Registration**                 | **Must Have**   | Form to register patients with fields: First Name, Last Name, Mobile Number, Email, Date of Birth. All patient records are retained indefinitely with an `is_scheduled` flag to indicate active scheduling status.                                                                                                                                                                                                |
| **Machine Status Management**            | **Must Have**   | Allows staff to update machine status to either `Available` or `Not Available`. When status is `Not Available`, a **downtime reason** must be provided.                                                                                                                                                                                                                                                           |
| **Auto-Patient Removal**                 | **Must Have**   | Automatically removes a patient from the schedule if they miss all three scheduled sessions in a week (based on sign-in status).                                                                                                                                                                                                                                                                                  |
| **Auto-Removal Notification**            | **Must Have**   | Displays a list of auto-removed patients (Name + Date of Birth) on the dashboard, sorted in descending order by removal date.                                                                                                                                                                                                                                                                                     |
| **Re-add Removed Patient**               | **Must Have**   | Allows manual re-addition of an auto-removed patient via the scheduler interface.                                                                                                                                                                                                                                                                                                                                 |
| **Audit Log**                            | **Should Have** | Logs all user actions (e.g., schedule changes, status updates) with timestamp and user identifier. Data retained for 3 years.                                                                                                                                                                                                                                                                                     |
| **Offline Functionality**                | **Could Have**  | Not required for MVP.                                                                                                                                                                                                                                                                                                                                                                                             |
| **Multi-Branch Dashboard**               | **Won't Have**  | Not required for MVP; future branch data will be filtered via `branch_id`.                                                                                                                                                                                                                                                                                                                                        |

---

## **4. SYSTEM & TECHNICAL REQUIREMENTS**

| Category | Specification |
|----------|---------------|
| **Performance** | No performance metric tied to internet connection. System responsiveness based on typical clinic broadband. |
| **Scalability** | Designed to support 30 machines × 6 days × 3 shifts (540 weekly slots). |
| **Security** | Role-based access control (RBAC). Personally identifiable information (PII) stored encrypted at rest using AES-256. Names are decrypted for display purposes only. Secure authentication required. |
| **Compliance** | Must comply with the Philippine Data Privacy Act (RA 10173). |
| **Data Retention** | Patient records and audit logs retained for **3 years**. |
| **Hosting** | Cloud VPS or Cloud Infrastructure (AWS, Azure, or Google Cloud). |
| **Database** | PostgreSQL (supports JSON, triggers, and audit logging). |
| **Authentication** | Email and password. Two-factor authentication (2FA) is optional. |

---

## **5. ASSUMPTIONS & CONSTRAINTS**

| Assumption | Description |
|------------|-------------|
| **User Devices** | Staff will use desktop computers or tablets located within the clinic. |
| **Status Updates** | Machine status updates are performed manually by scheduling staff. |
| **Missed Session Definition** | A "missed" session is defined as a scheduled slot with no recorded sign-in. |
| **Dashboard Content** | The dashboard displays only scheduled patients—no treatment completion or usage analytics. |
| **Data Sync** | The system does not require real-time synchronization or live updates. |

> **Constraints:**
> - No biometric or mobile check-in functionality.
> - No offline operation required for MVP.
> - No integration with existing electronic health records (EHR) or PhilHealth verification.
> - No automated machine status updates (e.g., based on sensor data).

---

## **6. RISKS & DEPENDENCIES**

| Risk | Impact | Mitigation |
|------|--------|------------|
| Staff forget to update machine status. | High | Required downtime reason field when status is set to `Not Available`. |
| Auto-removal list overlooked by staff. | Medium | Prominent placement on dashboard with clear heading. |
| Incomplete downtime reason entry. | Medium | Form validation requires reason when status is `Not Available`. |

| Dependency | Owner |
|------------|-------|
| Clinic hardware (desktops/tablets) | Envivo IT |
| Staff training and onboarding | Envivo Operations |
| Cloud VPS/Infrastructure setup | John Rey Faciolan |
| Final validation and approval | Carissa Dumancas (CEO) |

---

## **7. SUCCESS METRICS & RELEASE CRITERIA**

| Metric | Target |
|--------|--------|
| No scheduling conflicts in the first 30 days post-launch. | ✅ |
| Zero critical bugs in acceptance testing. | ✅ |
| 90% of scheduling staff trained and confident using the system. | ✅ |
| Auto-removal list accurately generated and visibly displayed. | ✅ |

> **Go/No-Go Release Criteria:**
> - All defined test cases pass with zero critical defects.
> - 90% of relevant staff complete training.
> - Formal sign-off from the CEO (Carissa Dumancas).

---

## **8. TIMELINE & RELEASE PLAN**

| Milestone | Target Date | Owner |
|-----------|-------------|-------|
| MVP Development Complete | July 31, 2026 | John Rey Faciolan |
| Internal Testing & QA | August 1–15, 2026 | John Rey Faciolan |
| **Soft Launch (Bacolod Branch)** | **August 31, 2026** | Envivo Operations |
| **Stakeholder Sign-Off** | **September 5, 2026** | **Carissa Dumancas** |

> The Talisay branch launch is not part of the MVP and has no defined timeline in this document.

---

## **9. STAKEHOLDER REVIEW & APPROVAL**

| Stakeholder | Role | Approval Responsibility |
|-------------|------|-------------------------|
| **Carissa Dumancas** | Chief Executive Officer | Final approval of MVP and authorization for operational release. |
| **John Rey Faciolan** | Analytics and Automation Consultant | Confirmation of delivery completion, testing, and technical handover. |

---

## **APPROVAL**

This document signifies mutual understanding and agreement of the product requirements for the Envivo Web-Based Scheduling System.

**Prepared by:**  
_________________________  
**John Rey Faciolan**  
Analytics and Automation Consultant  
February 5, 2026

**Approved by:**  
_________________________  
**Carissa Dumancas**  
Chief Executive Officer  
Date: _______________