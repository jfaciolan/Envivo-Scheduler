# Envivo Web-Based Scheduling System

A comprehensive web-based scheduling and patient management system designed for Envivo Dialysis Center in Bacolod City. This system replaces manual Google Calendar scheduling with an automated, rules-based platform to streamline dialysis patient scheduling, improve operational visibility, and support future expansion.

## 📋 Project Overview

**Client:** Envivo Dialysis Center - Bacolod City  
**Status:** MVP Development  
**Target Launch:** August 31, 2026  
**Version:** 1.1 (Complete MVP with Check-In System)

## 📁 Documentation Structure

This repository contains all product documentation for the Envivo Scheduling System:

### [`docs/Product Requirement Document.md`](docs/Product%20Requirement%20Document.md)
The master requirements document defining the complete MVP scope including:
- Project introduction and purpose
- Target audience and user personas
- MoSCoW prioritized features
- System and technical requirements
- Assumptions, constraints, and risks
- Success metrics and release criteria
- Timeline and stakeholder information

### [`docs/UI-UX Requirement Document.md`](docs/UI-UX%20Requirement%20Document.md)
Comprehensive design specifications covering:
- UX vision and strategy
- User personas and Jobs to Be Done (JTBD)
- Brand application framework (colors, typography, voice)
- Detailed interaction flows and edge cases
- Component-layer UI states (5-state framework)
- Error handling and accessibility requirements
- Complete Figma design system references

### [`docs/User Story Collection.md`](docs/User%20Story%20Collection.md)
Complete development backlog with:
- 72 user stories across 7 epics
- Detailed acceptance criteria for each feature
- Story mapping by release (MVP: August 31, 2026)
- Dependency matrix and validation checklist
- Definition of Ready (DoR) and Definition of Done (DoD)
- Supplemental artifacts for development planning

## 🎯 Core Features (MVP)

### Must-Have Features
1. **Dashboard – Visual Schedule Overview** - Color-coded grid showing patient schedules across machines, days, and shifts
2. **Patient Check-In System** - Daily interface for marking patient attendance with timestamps
3. **Manual Patient Booking** - Rules-based scheduling with availability constraints
4. **Patient Registration** - Form with 6 fields including privacy consent for Data Privacy Act compliance
5. **Patient Profile View** - Read-only view with check-in history and attendance statistics
6. **Machine Management** - Full CRUD operations for dialysis machines
7. **Auto-Patient Removal** - Automatic removal after 3 missed sessions (based on check-in records)
8. **Manual Schedule Removal** - For exceptional cases (death, transfers, cancellations)
9. **Re-add Removed Patient** - Manual re-addition with availability checking
10. **Error Pages & States** - Comprehensive error handling with branded pages

### Should-Have Features
- **Audit Log** - System-wide logging of all user actions

## 👥 Target Users

| Role | Permissions | Key Responsibilities |
|------|-------------|---------------------|
| **Receptionist/Scheduler** | Full CRUD on schedules, check-ins, patients | Daily scheduling, patient check-in/sign-out, manual removals |
| **Shift Supervisor** | Read-only access | Schedule verification, compliance monitoring |
| **Machine Technician** | Full CRUD on machines | Machine status management, downtime tracking |

## 🛠 Technical Stack

- **Database:** PostgreSQL (with JSON support, triggers, audit logging)
- **Hosting:** Cloud VPS/Infrastructure (AWS/Azure/Google Cloud)
- **Security:** Role-based access control (RBAC), AES-256 encryption for PII
- **Performance:** Supports 30 machines × 6 days × 3 shifts (540 weekly slots)
- **Compliance:** Philippine Data Privacy Act (RA 10173)

## 📅 Timeline

| Milestone | Target Date | Status |
|-----------|-------------|--------|
| MVP Development Complete | July 31, 2026 | Planned |
| Internal Testing & QA | August 1–15, 2026 | Planned |
| Staff Training | August 16–25, 2026 | Planned |
| Data Migration | August 26–28, 2026 | Planned |
| **Soft Launch (Bacolod Branch)** | **August 31, 2026** | **Target** |
| Stakeholder Sign-Off | September 5, 2026 | Planned |

## 🚀 Success Metrics

- ✅ No scheduling conflicts in first 30 days post-launch
- ✅ Zero critical bugs in acceptance testing
- ✅ 90% of staff trained and confident using the system
- ✅ Auto-removal list accurately generated from check-in data
- ✅ Check-in system used for 95% of patient visits in first week
- ✅ 100% correlation between check-ins and auto-removals

## 📄 License & Compliance

This system complies with the Philippine Data Privacy Act (RA 10173). All patient data includes privacy consent collection and AES-256 encryption at rest.

## 🤝 Stakeholders

- **Carissa Dumancas** - Chief Executive Officer (Final Approver)
- **John Rey Faciolan** - Analytics and Automation Consultant (Project Lead)
- **Envivo Operations Manager** - Operations Lead
- **Envivo IT Coordinator** - IT Infrastructure

---

**Last Updated:** February 4, 2026  
**Document Version:** 1.0
**Prepared By:** John Rey Faciolan, Analytics and Automation Consultant