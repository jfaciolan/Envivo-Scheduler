# **UPDATED PRODUCT REQUIREMENTS DOCUMENT - VERSION 1.1**

**Project:** Envivo Web-Based Scheduling System  
**Client:** Envivo Dialysis Center - Bacolod City  
**Prepared For:** Carissa Dumancas, Chief Executive Officer  
**Prepared By:** John Rey Faciolan, Analytics and Automation Consultant  
**Date:** February 5, 2026  
**Version:** 1.1 - Complete MVP with Check-In System  

---

## **1. INTRODUCTION & PURPOSE**

Envivo is the premier dialysis provider in Bacolod City, currently operating 15 dialysis machines and planning to expand to 30 machines by August 2026. Patient scheduling is currently managed manually using Google Calendar, which has become unsustainable and error-prone as operations scale.

This document defines the requirements for a web-based scheduling system designed to streamline patient scheduling, improve operational visibility, and support future expansion. The system will replace the current manual process with a centralized, rules-based scheduling platform accessible to receptionists and scheduling staff.

---

## **2. TARGET AUDIENCE & USER PERSONAS**

| Role | Permissions | Access Scope |
|------|-------------|--------------|
| **Receptionist/Scheduler** | Create, read, update, delete schedules; check in patients; update machine status; manage machines | Bacolod Branch (MVP only) |
| **Shift Supervisor (Schedule Viewer)** | Read-only access to schedules and check-in records | Bacolod Branch (MVP only) |
| **Machine Technician** | Manage machine registration, updates, and status changes | Bacolod Branch (MVP only) |

> All users access a unified system. Data isolation between future branches will be managed via a `branch_id` database field.

---

## **3. FEATURES & FUNCTIONALITY (MoSCoW PRIORITIZATION)**

| Feature                                  | Priority        | Description                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dashboard – Visual Schedule Overview** | **Must Have**   | A visual grid interface displaying scheduled patients across machines, days, and shifts using color coding. Colors represent: <br>• **Green**: Available slot <br>• **Blue**: Scheduled patient <br>• **Red**: Machine marked as "Not Available" <br>Each scheduled slot displays patient initials or abbreviated name. Hover reveals full patient details. Includes auto-removal list sorted by removal date. |
| **Patient Check-In System**              | **Must Have**   | Daily interface for receptionists to mark patients as present for their scheduled sessions. Displays today's scheduled patients with: patient name, scheduled shift, machine number, check-in status (Not Checked In/Checked In/Late). Allows single-click check-in with timestamp recording. Data drives auto-removal logic.                                                                                     |
| **Scheduler – Manual Patient Booking**   | **Must Have**   | Interface to schedule a patient by selecting: patient → day group (A/B) → shift (A/B/C) → machine. <br><br>**Availability Logic:** <br>• Only day group + shift + machine combinations with available slots are shown. <br>• Fully booked machines are excluded from selection. <br>• Selection of already-booked slots is prevented. <br>• Availability is based on existing scheduled bookings (not real-time). |
| **Patient Registration**                 | **Must Have**   | Form to register patients with fields: **First Name, Last Name, Mobile Number, Email, Date of Birth, Gender**. Includes **privacy consent checkbox** for Data Privacy Act compliance. All patient records are retained indefinitely with an `is_scheduled` flag to indicate active scheduling status.                                                                                                            |
| **Patient Profile View**                 | **Must Have**   | Read-only view of patient information including: Full Name, DOB, Mobile, Gender, Last Booking Date, Check-in History (last 30 days), and attendance statistics.                                                                                                                                                                                                                                                   |
| **Machine Management**                   | **Must Have**   | Full CRUD operations for machines: Create, Read, Update, Delete. Includes fields: Machine ID, Machine Name, Location, Branch (default: Bacolod). Allows staff to update machine status to either `Available` or `Not Available`. When status is `Not Available`, a **downtime reason** must be provided.                                                                                                          |
| **Auto-Patient Removal**                 | **Must Have**   | Automatically removes a patient from the schedule if they miss all three scheduled sessions in a week **based on check-in records**. System tracks attendance via manual check-in at reception. Removal occurs automatically at midnight Sunday/Monday.                                                                                                                                                           |
| **Auto-Removal Notification**            | **Must Have**   | Displays a list of auto-removed patients (Name + Date of Birth) on the dashboard, sorted in descending order by removal date with reason: "3 missed sessions".                                                                                                                                                                                                                                                    |
| **Re-add Removed Patient**               | **Must Have**   | Allows manual re-addition of an auto-removed patient via the scheduler interface.                                                                                                                                                                                                                                                                                                                                 |
| **Error Pages & States**                 | **Must Have**   | Comprehensive error handling with branded error pages (404, 403, 500) and maintenance mode page. All components handle ideal, empty, loading, error, and partial states.                                                                                                                                                                                                                                          |
| **Audit Log**                            | **Should Have** | Logs all user actions (e.g., schedule changes, check-ins, status updates, machine management) with timestamp and user identifier. Data retained for 3 years. UI for viewing logs with filtering capabilities.                                                                                                                                                                                                     |
| **Help & Support Section**               | **Could Have**  | Basic help documentation and FAQ for staff onboarding and troubleshooting.                                                                                                                                                                                                                                                                                                                                        |
| **Offline Functionality**                | **Won't Have**  | Not required for MVP.                                                                                                                                                                                                                                                                                                                                                                                             |
| **Multi-Branch Dashboard**               | **Won't Have**  | Not required for MVP; future branch data will be filtered via `branch_id`.                                                                                                                                                                                                                                                                                                                                        |
| **2FA & Email Settings**                 | **Won't Have**  | Not required for MVP.                                                                                                                                                                                                                                                                                                                                                                                             |

---

## **4. SYSTEM & TECHNICAL REQUIREMENTS**

| Category | Specification |
|----------|---------------|
| **Performance** | No performance metric tied to internet connection. System responsiveness based on typical clinic broadband. Must handle concurrent check-ins during peak hours (8-10AM). |
| **Scalability** | Designed to support 30 machines × 6 days × 3 shifts (540 weekly slots) plus check-in records. |
| **Security** | Role-based access control (RBAC). Personally identifiable information (PII) stored encrypted at rest using AES-256. Names are decrypted for display purposes only. Secure authentication required (email/password). |
| **Compliance** | Must comply with the Philippine Data Privacy Act (RA 10173). UI must include privacy notices and consent mechanisms. Check-in records are considered medical attendance data. |
| **Data Retention** | Patient records and audit logs retained for **3 years**. Check-in records retained for 3 years for compliance and reporting. |
| **Hosting** | Cloud VPS or Cloud Infrastructure (AWS, Azure, or Google Cloud). |
| **Database** | PostgreSQL (supports JSON, triggers, and audit logging). Required tables: patients, schedules, machines, patient_checkins, audit_log. |
| **Authentication** | Email and password authentication only for MVP. |
| **Error Handling** | Comprehensive error page system (404, 403, 500, maintenance mode) with brand-consistent design. |

---

## **5. ASSUMPTIONS & CONSTRAINTS**

| Assumption | Description |
|------------|-------------|
| **User Devices** | Staff will use desktop computers or tablets located within the clinic. Reception desk requires dedicated terminal for check-ins. |
| **Check-In Process** | Patients check in at reception desk upon arrival. Receptionist manually records attendance via the system. |
| **Check-In Timing** | Shift end times define missed sessions: Shift A (10AM), B (2PM), C (6PM). No check-in by shift end = missed session. |
| **Machine Setup** | Machine creation and deletion capabilities required for initial system setup and maintenance. |
| **Patient Data** | Patient records include gender but do NOT include emergency contact or address information for MVP. |
| **Age Requirements** | No minimum age limit for patients. Infants can be scheduled for dialysis. Date of Birth field has no validation restrictions. |
| **Branch Management** | MVP includes branch field in machine management for future expansion, but only Bacolod branch will be used initially. |
| **Missed Session Definition** | A "missed" session is defined as a scheduled slot with **no check-in recorded by the receptionist by shift end time**. |
| **Dashboard Content** | The dashboard displays scheduled patients and check-in status—no treatment completion or usage analytics. |
| **Data Sync** | The system does not require real-time synchronization or live updates between multiple devices. |

> **Constraints:**
> - No biometric or mobile check-in functionality.
> - No offline operation required for MVP.
> - No integration with existing electronic health records (EHR) or PhilHealth verification.
> - No automated machine status updates (e.g., based on sensor data).
> - No 2-factor authentication or email notification settings for MVP.
> - Check-in must be manual (no automated attendance tracking).

---

## **6. RISKS & DEPENDENCIES**

| Risk | Impact | Mitigation |
|------|--------|------------|
| **Receptionist forgets to check in patients** | High | Dashboard prominently displays unchecked patients. Training emphasizes check-in importance for auto-removal accuracy. |
| **Staff forget to update machine status** | High | Required downtime reason field when status is set to `Not Available`. Dashboard highlights unavailable machines. |
| **Auto-removal list overlooked by staff** | Medium | Prominent placement on dashboard with clear heading and sorting by date. |
| **Incomplete downtime reason entry** | Medium | Form validation requires reason when status is `Not Available`. |
| **Data privacy compliance gaps** | High | Include privacy consent in patient registration, ensure PII encryption, and restrict data access by role. |
| **Check-in system not used consistently** | Critical | Integrate check-in into daily workflow. Supervisor oversight of check-in compliance. |

| Dependency | Owner |
|------------|-------|
| **Clinic hardware (desktops/tablets)** | Envivo IT |
| **Staff training and onboarding** | Envivo Operations |
| **Cloud VPS/Infrastructure setup** | John Rey Faciolan |
| **Final validation and approval** | Carissa Dumancas (CEO) |
| **Privacy compliance review** | Envivo Legal/Compliance |
| **Check-in process integration** | Envivo Reception Staff |

---

## **7. SUCCESS METRICS & RELEASE CRITERIA**

| Metric | Target |
|--------|--------|
| **No scheduling conflicts** in the first 30 days post-launch. | ✅ |
| **Zero critical bugs** in acceptance testing. | ✅ |
| **90% of scheduling/reception staff** trained and confident using the system. | ✅ |
| **Auto-removal list** accurately generated based on check-in data. | ✅ |
| **Machine management (CRUD)** functioning without errors. | ✅ |
| **Privacy consent** implemented in patient registration. | ✅ |
| **Check-in system** used for 95% of patient visits in first week. | ✅ |
| **Auto-removal accuracy** - 100% correlation between check-ins and removals. | ✅ |

> **Go/No-Go Release Criteria:**
> - All defined test cases pass with zero critical defects.
> - 90% of relevant staff complete training, including check-in procedures.
> - Privacy compliance measures implemented and verified.
> - Check-in system tested with mock patient flow.
> - Auto-removal logic validated with test data.
> - Formal sign-off from the CEO (Carissa Dumancas).

---

## **8. TIMELINE & RELEASE PLAN**

| Milestone | Target Date | Owner | Notes |
|-----------|-------------|-------|-------|
| **MVP Development Complete** | July 31, 2026 | John Rey Faciolan | Includes check-in system development |
| **Internal Testing & QA** | August 1–15, 2026 | John Rey Faciolan | Focus on check-in → auto-removal flow |
| **Staff Training** | August 16–25, 2026 | Envivo Operations | Emphasize check-in procedures |
| **Data Migration** | August 26–28, 2026 | John Rey Faciolan | Migrate existing patient data |
| **Soft Launch (Bacolod Branch)** | **August 31, 2026** | Envivo Operations | Go-live with parallel monitoring |
| **Stakeholder Sign-Off** | **September 5, 2026** | **Carissa Dumancas** | Post-launch review and approval |

> The Talisay branch launch is not part of the MVP and has no defined timeline in this document.

---

## **9. STAKEHOLDER REVIEW & APPROVAL**

| Stakeholder | Role | Approval Responsibility |
|-------------|------|-------------------------|
| **Carissa Dumancas** | Chief Executive Officer | Final approval of MVP and authorization for operational release. |
| **John Rey Faciolan** | Analytics and Automation Consultant | Confirmation of delivery completion, testing, and technical handover. |
| **Envivo Operations Manager** | Operations Lead | Validation of workflow integration and staff readiness. |
| **Envivo IT Coordinator** | IT Infrastructure | Verification of system deployment and hardware readiness. |

---

## **APPROVAL**

This document signifies mutual understanding and agreement of the product requirements for the Envivo Web-Based Scheduling System, version 1.1, including the critical patient check-in system.

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

---

## **CHANGE LOG**

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Feb 2, 2026 | Initial PRD draft |
| 1.1 | Feb 4, 2026 | **Major update to align with URD MVP scope:**<br>• Added Machine Management (CRUD) as Must Have<br>• Updated Patient Registration to 6 fields (added Gender)<br>• Added Patient Profile View as Must Have<br>• Added Error Pages & States as Must Have<br>• Added Help & Support as Could Have<br>• Updated Assumptions: machine setup, patient data fields, no age limit<br>• Added privacy compliance requirements<br>• Removed 2FA/email settings from MVP<br>• Clarified audit log priority as Should Have<br>**Critical update adding Patient Check-In System:**<br>• Added "Patient Check-In System" as Must Have feature<br>• Updated auto-removal description to specify check-in dependency<br>• Added check-in timing assumptions and constraints<br>• Updated success metrics for check-in usage<br>• Added check-in-related risks and mitigations<br>• Updated timeline to include check-in development<br>• All features now align with URD version 1.2 |

---