# Project Proposal & Development Plan (Sprint-Based with Per-Sprint Payment)

**Project:** Envivo Web-Based Scheduling System (MVP)  
**Client:** Envivo Dialysis Center – Bacolod City  
**Prepared For:** Carissa Dumancas, Chief Executive Officer  
**Prepared By:** John Rey Faciolan, Analytics and Automation Consultant  
**Date:** March 7, 2026  
**Version:** 3.0 – Sprint-Based with Per-Sprint Payment

---

## Executive Summary

This proposal outlines a sprint-based development plan for a custom web-based scheduling system for Envivo Dialysis Center. The current manual process using Google Calendar is no longer sustainable as operations scale to 30 machines.

The project will deliver a centralized, rules-based scheduling platform with a visual dashboard, patient check-in/sign-out, and automated patient removal based on attendance. Development will be organized into **five sprints** (four 2-week sprints + one 1-week buffer sprint), each delivering a working, demonstrable subset of features. This ensures you see tangible progress every sprint and can provide feedback early.

**Key Deliverables (MVP):**
- Full-stack web application with role-based access (Receptionist, Supervisor)
- Patient registration and profile management
- Machine management (CRUD) with status tracking
- Visual schedule dashboard (color-coded, interactive)
- Manual patient booking with availability filtering
- Daily check-in & sign-out system
- Automated missed-session detection and patient removal
- Manual removal and re-add flows
- Audit log and compliance features

**Timeline:**  
- Development: March 16 – May 15, 2026 (5 sprints)  
- Beta Testing: May 18 – May 29, 2026  
- Feedback & Production Deployment: June 1 – June 30, 2026  
- **Go-Live:** June 30, 2026

**Total Professional Fee:** PHP 125,000.00 (based on 125 hours @ PHP 1,000/hour)  
*(Consultant responsible for tax remittance)*

**Payment Terms (Per Sprint):**  
- Payment for each sprint is due **before the start of that sprint**.  
- Sprint costs are calculated based on the estimated hours for that sprint × PHP 1,000.  
- No upfront 50% downpayment – you only pay for the work scheduled in the upcoming sprint.

---

## Sprint-Based Development Approach

Each sprint is a timeboxed period (2 weeks or 1 week) with **3 hours of work per day**, for a total capacity of 30 hours per 2-week sprint and 15 hours for the 1-week buffer sprint. At the end of each sprint, you will receive a working increment of the system deployed on a staging server for review and feedback.

| Sprint | Duration | Dates | Capacity | Focus | Payment Due |
|--------|----------|-------|----------|-------|-------------|
| **Sprint 1** | 2 weeks | Mar 16 – Mar 27 | 30 hrs | Foundation, Patient & Machine Management | Before Mar 16 |
| **Sprint 2** | 2 weeks | Mar 30 – Apr 10 | 30 hrs | Patient Profiles & Visual Dashboard | Before Mar 30 |
| **Sprint 3** | 2 weeks | Apr 13 – Apr 24 | 30 hrs | Booking & Check-In | Before Apr 13 |
| **Sprint 4** | 2 weeks | Apr 27 – May 8 | 30 hrs | Removal Logic & Automation | Before Apr 27 |
| **Sprint 5 (Buffer)** | 1 week | May 11 – May 15 | 15 hrs | Audit Log & Polishing | Before May 11 |
| **Total** | **9 weeks** | | **125 hrs** | | |

---

## Detailed Work Items by Sprint

All tasks are strictly sequential within each sprint to respect single-developer constraints. Each task includes the associated User Story ID from the requirements document.

### Sprint 1: Foundation & Core Management (Mar 16 – Mar 27)
*Capacity: 30 hours*

| Week | ID | User Story | Task Description | Hours | Start Date | End Date |
|:----:|:--:|------------|-------------------|:-----:|:----------:|:--------:|
| 1 | 1.1 | N/A | Project Foundation: VPS (Docker/MySQL), Django project, auth & RBAC | 6 | Mar 16 | Mar 18 |
| 1 | 1.2 | PROJ-02-01, PROJ-03-01, PROJ-04-01 | Core Data Models: Patient, Machine, Schedule, CheckIn, AuditLog | 10 | Mar 18 | Mar 21 |
| 2 | 1.3 | PROJ-02-01 | Patient Registration: form with 6 fields + privacy consent, PII encryption | 6 | Mar 21 | Mar 24 |
| 2 | 1.4 | PROJ-03-01, PROJ-03-02, PROJ-03-03 | Machine Management: full CRUD, status update with reason, delete protection | 8 | Mar 24 | Mar 27 |
| | | | **Sprint 1 Total** | **30** | | |

**Sprint 1 Cost: 30 hrs × PHP 1,000 = PHP 30,000**  
**Sprint 1 Demo:** You can log in, register a new patient, and add/edit/delete machines. All data is persisted.

---

### Sprint 2: Patient Profiles & Dashboard (Mar 30 – Apr 10)
*Capacity: 30 hours*

| Week | ID | User Story | Task Description | Hours | Start Date | End Date |
|:----:|:--:|------------|-------------------|:-----:|:----------:|:--------:|
| 3 | 2.1 | PROJ-02-02, PROJ-02-03 | Patient Profile View: Overview tab, Check-In History (30 days), Schedule History, "Remove" button | 10 | Mar 30 | Apr 2 |
| 3-4 | 2.2 | PROJ-01-01 | Visual Dashboard – Schedule Grid: 7-day × machines × shifts grid with color coding, hover details, click to profile | 15 | Apr 2 | Apr 9 |
| 4 | | | *Slack for early feedback* | 5 | Apr 9 | Apr 10 |
| | | | **Sprint 2 Total** | **30** | | |

**Sprint 2 Cost: 30 hrs × PHP 1,000 = PHP 30,000**  
**Sprint 2 Demo:** You can view a patient’s full profile and see a visual schedule grid showing available, booked, and unavailable machines.

---

### Sprint 3: Booking & Check-In (Apr 13 – Apr 24)
*Capacity: 30 hours*

| Week | ID | User Story | Task Description | Hours | Start Date | End Date |
|:----:|:--:|------------|-------------------|:-----:|:----------:|:--------:|
| 5 | 3.1 | PROJ-04-01, PROJ-04-02 | Manual Booking Scheduler: multi-step modal with dynamic availability filtering | 12 | Apr 13 | Apr 17 |
| 5-6 | 3.2 | PROJ-01-04, PROJ-01-05 | Daily Check-In Interface: table of today’s patients with Check In / Sign Out buttons, status tracking | 10 | Apr 17 | Apr 21 |
| 6 | 3.3 | PROJ-01-06, PROJ-01-07, PROJ-01-08 | Late Check-In & Edge Cases: late logic (15+ min), post-shift handling, unscheduled quick-booking | 8 | Apr 21 | Apr 24 |
| | | | **Sprint 3 Total** | **30** | | |

**Sprint 3 Cost: 30 hrs × PHP 1,000 = PHP 30,000**  
**Sprint 3 Demo:** You can book a patient into an available slot, then check them in upon arrival (including late scenarios). Sign-out records completion.

---

### Sprint 4: Removal Logic & Automation (Apr 27 – May 8)
*Capacity: 30 hours*

| Week | ID | User Story | Task Description | Hours | Start Date | End Date |
|:----:|:--:|------------|-------------------|:-----:|:----------:|:--------:|
| 7 | 4.1 | PROJ-01-09 | Missed Session Detection: cron job at shift end to mark no-shows and increment counter | 4 | Apr 27 | Apr 28 |
| 7 | 4.2 | PROJ-01-10 | Auto-Removal System: Sunday midnight job to remove patients with 3 missed sessions, toast notification | 6 | Apr 28 | Apr 30 |
| 7 | 4.3 | PROJ-01-11, PROJ-01-12 | Manual Removal Flow: "Remove from Schedule" button on profile, modal with reason dropdown, prevent if checked in | 4 | Apr 30 | May 1 |
| 7-8 | 4.4 | PROJ-01-02, PROJ-01-03 | Removal Lists (Auto & Manual): dashboard widgets with "Re-add" buttons | 4 | May 1 | May 4 |
| 8 | 4.5 | PROJ-01-13 | Re-add Removed Patient: two-option flow (re-add only, re-add with scheduling) | 4 | May 4 | May 5 |
| 8 | 4.6 | PROJ-05-02, PROJ-05-03, PROJ-06-02 | System Resilience: branded error pages (404,403,500), network error handling, privacy compliance | 8 | May 5 | May 8 |
| | | | **Sprint 4 Total** | **30** | | |

**Sprint 4 Cost: 30 hrs × PHP 1,000 = PHP 30,000**  
**Sprint 4 Demo:** Full attendance-driven auto-removal works; you can manually remove patients (with reasons) and re-add them. Dashboard shows removal lists.

---

### Sprint 5 (Buffer): Audit Log & Polishing (May 11 – May 15)
*Capacity: 15 hours*

| Week | ID | User Story | Task Description | Hours | Start Date | End Date |
|:----:|:--:|------------|-------------------|:-----:|:----------:|:--------:|
| 9 | 5.1 | PROJ-05-01 | Audit Log: filterable table of all user actions (check-ins, removals, bookings, etc.) | 8 | May 11 | May 13 |
| 9 | 5.2 | All stories | Polishing & Integration Testing: end-to-end testing, bug fixes, UI polish | 6 | May 13 | May 15 |
| | | | **Sprint 5 Total** | **14** | | |

**Sprint 5 Cost: 14 hrs × PHP 1,000 = PHP 14,000**  
**Sprint 5 Deliverable:** Complete system ready for Beta Testing, including full audit trail.

---

## Total Hours & Cost Summary

| Sprint | Hours | Cost (PHP) |
|--------|-------|------------|
| Sprint 1 | 30 | 30,000 |
| Sprint 2 | 30 | 30,000 |
| Sprint 3 | 30 | 30,000 |
| Sprint 4 | 30 | 30,000 |
| Sprint 5 (Buffer) | 14 | 14,000 |
| **Total** | **124*** | **124,000** |

* *Note: The earlier total was 125 hours; the adjusted sum here is 124 due to rounding in individual task estimates. The final cost is based on actual hours worked, but we cap at 125 hours maximum. For simplicity, we will use the originally agreed **125 hours / PHP 125,000** as the not‑to‑exceed total. Any hours under will be billed at actual; any overage within the buffer is covered. The per‑sprint payments are estimates and will be adjusted based on actual hours if they differ significantly, but the total will not exceed PHP 125,000.*

**Guaranteed Maximum Total: PHP 125,000**

---

## Payment Schedule (Per Sprint)

Payment for each sprint is due **before the start of that sprint**. This ensures that work can begin immediately and you only pay for the next increment of functionality.

| Sprint | Payment Due Date | Amount (PHP) |
|--------|------------------|--------------|
| Sprint 1 | Before Mar 16, 2026 | 30,000 |
| Sprint 2 | Before Mar 30, 2026 | 30,000 |
| Sprint 3 | Before Apr 13, 2026 | 30,000 |
| Sprint 4 | Before Apr 27, 2026 | 30,000 |
| Sprint 5 | Before May 11, 2026 | 14,000 |
| **Total** | | **124,000 – 125,000** |

*Actual final payment may be adjusted based on exact hours worked in Sprint 5, but will not exceed the guaranteed maximum.*

---

## Timeline Summary

| Phase | Dates | Duration |
|-------|-------|----------|
| **Sprint 1** | Mar 16 – Mar 27 | 2 weeks |
| **Sprint 2** | Mar 30 – Apr 10 | 2 weeks |
| **Sprint 3** | Apr 13 – Apr 24 | 2 weeks |
| **Sprint 4** | Apr 27 – May 8 | 2 weeks |
| **Sprint 5 (Buffer)** | May 11 – May 15 | 1 week |
| **Beta Testing** | May 18 – May 29 | 2 weeks |
| **Feedback & Production Deployment** | Jun 1 – Jun 30 | 4 weeks |
| **Go-Live** | **June 30, 2026** | |

---

## Success Criteria & Key Risks

**Go/No-Go Release Criteria:**
- All features demonstrated in sprints are fully functional
- Beta testing confirms auto-removal works based on check-in data
- Staff trained and comfortable with workflows
- Privacy compliance verified

**Key Risks & Mitigation:**

| Risk | Impact | Mitigation |
|------|--------|------------|
| Scope creep | High | Strict adherence to defined user stories; any new requests deferred to Phase 2 |
| Schedule slippage | Low | Built-in buffer sprint; slack within sprints |
| Staff adoption | Critical | UI designed for clarity; on-site training during Feedback phase |
| Data privacy | High | Encryption at rest; consent checkbox; role-based access |

---

## Sign-Off

By signing below, you approve the sprint-based MVP scope, timeline, and per-sprint payment terms. Payment for Sprint 1 is due before March 16, 2026 to commence work.

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Chief Executive Officer | Carissa Dumancas | _________________ | ___________ |

---

**Prepared by:**  
John Rey Faciolan  
Analytics and Automation Consultant