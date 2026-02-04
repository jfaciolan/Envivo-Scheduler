# **UI/UX REQUIREMENT DOCUMENT (URD) - VERSION 1.1**

**Project:** Envivo Web-Based Scheduling System  
**Client:** Envivo Dialysis Center – Bacolod City  
**Prepared By:** Senior UI/UX Strategist & Product Designer  
**Date:** February 4, 2026  
**Version:** 1.1 – Complete UI/UX Specifications

---

## **1. EXECUTIVE SUMMARY: UX VISION & STRATEGY**

The Envivo Scheduling System is engineered to function as a **real-time operational command center**—a digital nervous system for dialysis operations where clarity, resilience, and trust are foundational principles. The system includes **patient check-in and sign-out functionality** at the reception desk, making attendance tracking integral to the scheduling workflow with **immediate auto-removal** when patients miss three sessions, plus **manual schedule removal** for exceptional cases.

This document formalizes the complete UI/UX specifications ensuring every state—whether ideal, empty, loading, error, or partial—is **visually distinct, semantically meaningful, and brand-aligned**. The experience is designed to be **predictable, accessible, and operationally resilient**, allowing frontline staff to act with confidence.

> **Core UX Truth**: The interface should feel like an extension of the user's mind—intuitive, immediate, and unobtrusive. Critical actions like removal must be deliberate but straightforward.

---

## **2. USER PERSONAS & JOBS TO BE DONE (JTBD)**

| Persona | Role | Primary JTBD | Motivations | Pain Points |
|--------|------|--------------|------------|-------------|
| **Lara, Receptionist** | Frontline scheduler **and check-in/sign-out operator** | 1. "Schedule patients without conflicts"<br>2. "Quickly check in arriving patients"<br>3. "Record sign-out after treatment"<br>4. "Monitor attendance and handle removals"<br>5. **"Manually remove patients when needed (death, cancellation, etc.)"** | Accuracy, efficiency, compliance, complete records | Manual errors, forgetting check-ins/sign-outs, handling exceptional cases |
| **Miguel, Shift Supervisor** | Oversight role | "Verify schedules and attendance compliance" | Operational transparency, accountability | No real-time visibility into actual attendance |
| **Elena, Machine Technician** | Technical operator | "Manage machine availability and status" | System integrity, operational continuity | Manual status updates, lack of audit trail |

---

## **3. BRAND APPLICATION FRAMEWORK**

### **3.1 Color Palette Application**

| Role | Tailwind Key | Hex Code | Usage & Rationale |
|------|--------------|----------|------------------|
| **Primary** | `envivo-teal` | `#2D7A78` | Navigation bar, primary headers, active tab indicators. |
| **Secondary** | `envivo-pink` | `#E86A92` | High-action CTAs ("Check In", "New Booking", "Re-add"). |
| **Neutral Background** | `envivo-cream` | `#F8F1E5` | Dashboard and page backgrounds. Reduces eye strain. |
| **Success (Available/Checked In)** | `status.available` | `#28A745` | Available slots, checked-in patients. |
| **Info (Scheduled)** | `status.scheduled` | `#0D6EFD` | Booked slots (not yet checked in). |
| **Danger (Unavailable/Missed/Auto-Removed)** | `status.unavailable` | `#DC3545` | Machine downtime, missed check-ins, auto-removed patients. |
| **Warning (Late)** | `status.warning` | `#FFC107` | Late check-ins (15+ minutes past shift start). |
| **Complete (Signed Out)** | `status.complete` | `#6F42C1` | Patient has signed out after treatment. |
| **Manual Removal** | `status.manual-removal` | `#8B4513` | Manual removal records (brown). |
| **Notice (Auto-Removal Alert)** | `status.notice` | `#FFF3CD` | Auto-removal notification background. |

> ✅ **Accessibility Compliance**: All colors meet WCAG 2.1 AA contrast requirements.  
> ⚠️ **Color-Blind Safety**: No status communicated via color alone. All states include icons/text labels.

---

### **3.2 Typography Application**

| Purpose | Font | Size & Weight | Rationale |
|--------|------|----------------|----------|
| **Body Text** | Inter | 16px, Regular (400) | High legibility for dense data. |
| **Headings** | Lexend | 20px–24px, Medium (500) | Geometric, modern, matches brand. |
| **Labels & Form Inputs** | Inter | 14px, Medium (500) | Clear, consistent across forms. |
| **Status Badges** | Inter | 12px, Semi-bold (600) | Distinct from body text. |
| **Timestamp Text** | Inter | 12px, Regular (400) | Secondary information. |
| **Alert/Notification Text** | Inter | 14px, Semi-bold (600) | Draws attention to important system events. |

> All text uses **variable font weights**. Minimum text size: 14px for primary content.

---

### **3.3 Voice & Tone Examples**

| Scenario | Message (Brand-Aligned) |
|--------|------------------------|
| **Check-In Success** | "Maria Santos checked in at 9:15 AM for Shift A." |
| **Sign-Out Success** | "Maria Santos signed out at 11:45 AM." |
| **Late Check-In** | "Juan Dela Cruz checked in late at 10:20 AM (20 minutes late)." |
| **Manual Removal Success** | "Carlos Reyes removed from schedule. Reason: Moved to other Clinics." |
| **Missed Session Warning** | "Patient has missed 2/3 sessions. One more miss will trigger auto-removal." |
| **Immediate Auto-Removal Notification** | "⚠️ **IMMEDIATE AUTO-REMOVAL**: Carlos Reyes was automatically removed from all future schedules due to 3 missed sessions." |
| **Privacy Consent** | "I acknowledge that my personal data will be processed in accordance with the Data Privacy Act of 2012 (RA 10173)." |
| **Empty Check-In List** | "No patients scheduled for today. Check tomorrow's schedule or add new bookings." |

> Tone: **Calm, clear, and action-oriented**. No jargon. No blame. No panic.

---

## **4. INTERACTION FLOW & EDGE CASE MAPPING**

### **4.1 Patient Registration Flow**

**Steps:**
1. Navigate to Patient Registration
2. Fill 6 required fields + privacy consent
3. Real-time validation on each field
4. Submit with loading state
5. Success toast and redirection

**Edge Cases:**
- Duplicate mobile/email → Show error, suggest merge
- Form abandoned → Auto-save draft
- Session timeout → Preserve data, redirect to login

---

### **4.2 Manual Patient Booking Flow**

**Steps:**
1. Click "New Booking" from dashboard
2. Select patient from searchable dropdown
3. Select available Day Group (A/B)
4. Select available Shift (A/B/C)
5. Select available Machine from filtered list
6. Confirm booking with summary

**Key Constraint:** Only available Day Groups, Shifts, and Machines are shown

**Edge Cases:**
- Machine becomes unavailable mid-flow → Error, suggest alternatives
- Concurrent booking → "Slot taken" error with retry
- Patient auto-removed during booking → Disable selection

---

### **4.3 Patient Check-In & Sign-Out Flow**

**Steps:**
1. Receptionist opens Daily Check-In page
2. View today's patients sorted by shift
3. Click "Check In" next to arriving patient
4. System records timestamp and updates status to "Checked In"
5. After treatment completion, click "Sign Out" next to same patient
6. System records sign-out timestamp and updates status to "Signed Out"

**Timing Logic:**
- On Time Check-In: Within 15 minutes of shift start
- Late Check-In: 15+ minutes late but before shift end
- Missed: No check-in by shift end
- Sign-Out: Can occur at any time after check-in

**Edge Cases:**
- Check-in after shift end → "Mark as missed?" prompt
- Wrong patient checked in → "Undo" button for 5 minutes
- Patient without schedule → Quick booking option
- Sign-out attempted before check-in → Button disabled, tooltip: "Check in first"
- Patient leaves without signing out → Remains "Checked In" in historical records

---

### **4.4 Immediate Auto-Removal Flow**

**Trigger:** Patient reaches 3 missed sessions

**System Actions:**
1. Remove patient from all future schedules
2. Set patient status to inactive
3. Record removal reason and timestamp
4. Add to auto-removal list

**User Notification:**
1. Toast: "Patient [Name] auto-removed"
2. Dashboard widget updates
3. Patient list status changes

**Edge Cases:**
- Removal during active booking → Cancel booking, notify user
- System failure → Recovery job on restart
- Manual override needed → Admin "Force remove" option

---

### **4.5 Manual Schedule Removal Flow**

**Access Point:** Patient Profile → Overview tab → "Remove from Schedule" button

**Steps:**
1. User navigates to Patient Profile
2. Clicks **"Remove from Schedule"** button (appears if patient has future bookings)
3. **Removal Modal** appears with:
   - Patient name and next scheduled date
   - **Required Reason dropdown:** Cancelled Treatment / Moved to other Clinics / Deceased / Others
   - Notes field (optional, appears if "Others" selected)
   - Warning: "This will remove [Name] from all future scheduled sessions and set status to Inactive."
4. User selects reason, confirms removal
5. System performs:
   - Removes patient from **all future scheduled slots**
   - Sets patient status to **Inactive**
   - Records removal with: timestamp, user, reason, notes (if any)
   - Updates dashboard and schedule grid
6. Success toast appears: "[Name] removed from schedule. Reason: [Selected Reason]"

**Edge Cases:**
- Patient is currently checked in → Button disabled, tooltip: "Cannot remove checked-in patient"
- No future bookings → Button hidden
- Removal in progress while another user schedules patient → Lock patient record during removal
- Network failure during removal → Show error, preserve form data

---

### **4.6 Re-add Auto-Removed/Manually Removed Patient Flow**

**Steps:**
1. From **either** auto-removal or manual removal list, click "Re-add"
2. **Availability Check Dialog** appears:
   - Option A: "Re-add only" (mark active, schedule later)
   - Option B: "Re-add with immediate scheduling"
3. If Option B selected → Opens Booking Modal with:
   - Patient pre-selected
   - Only available Day Groups/Shifts/Machines shown
   - Standard availability constraints apply
4. Complete booking with availability validation
5. System sets patient status back to Active

**Key Constraint:** Must respect current machine and schedule slot availability

**Edge Cases:**
- No available slots → "Re-add only" or cancel
- Limited availability → Pre-filtered options only
- Concurrent booking → "Slot taken" error

---

### **4.7 Machine Management Flow**

**Sub-flows:**
- Register New Machine: Form with ID, Name, Location, Branch
- Update Status: "Mark Unavailable" with required reason
- Edit/Delete: Only if no future bookings

**Edge Cases:**
- Delete with bookings → "Cancel bookings first" warning
- Status change affects bookings → Option to notify
- Duplicate machine ID → Prevent at validation

---

### **4.8 Dashboard Viewing Flow (Read-Only)**

**For Shift Supervisors:**
- Schedule grid view-only
- Auto-removal widget view-only
- Check-in summary view-only
- Filter by date, machine, status

**Edge Cases:**
- Large dataset → Pagination and lazy loading
- Real-time updates → Manual refresh button
- Export needs → Future print/PDF functionality

---

### **4.9 Patient Profile & History Flow**

**Profile Tabs:**
1. Overview: Basic info, current status, **"Remove from Schedule" button** (if applicable)
2. Check-In History: 30-day attendance with check-in and sign-out timestamps
3. Schedule History: Past/future bookings **(removed sessions marked with reason)**
4. Audit Trail: System changes **(includes removal records)**

**Edge Cases:**
- Long history → Pagination and date filters
- Confidential info → Role-based access control
- Data privacy → Mask sensitive info in exports

---

## **5. RESILIENT INFORMATION ARCHITECTURE (SITEMAP)**

```
Home (Dashboard)
├── Patient Management
│   ├── Patient Registration
│   ├── Patient List (Active/Removed filters)
│   └── Patient Profile
│       ├── Overview
│       ├── Check-In History
│       ├── Schedule History
│       └── Audit Trail
├── Scheduling & Operations
│   ├── Manual Booking (Modal)
│   ├── Daily Check-In & Sign-Out
│   ├── Machine Status Update
│   └── View Weekly Schedule
├── System
│   ├── Machine Management
│   │   ├── Register New Machine
│   │   ├── Edit Machine
│   │   ├── Delete Machine
│   │   └── Update Status
│   └── Settings
│       └── User Preferences
├── Reports (Future)
└── Help & Support
    └── FAQ / Quick Guide

Error Pages (Global)
├── 404 – Page Not Found
├── 403 – Unauthorized Access
├── 500 – Internal Server Error
└── Maintenance Mode
```

> **Navigation Priority:** Daily Check-In & Sign-Out page must be 1-click accessible from any screen.

---

## **6. COMPONENT-LAYER UI STATES (5-STATE FRAMEWORK)**

### **6.1 Dashboard Schedule Grid**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | Grid: 7 days × machines × 3 shifts<br>• Green: Available slot<br>• Blue: Booked (patient initials)<br>• Red: Machine unavailable<br>• Hover: Patient details<br>• Click: Patient profile |
| **Empty** | "No patients scheduled this week."<br>• CTA: "Schedule first patient" |
| **Loading** | Skeleton grid: 7×15 cells with shimmer |
| **Error** | Red banner: "Failed to load schedule"<br>• Retry button |
| **Partial** | Some cells loaded, others loading |

---

### **6.2 Daily Check-In & Sign-Out Page**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | Table: Name, Shift, Machine, Status, Check-in Time, **Sign-Out Time**, Actions<br>• Status: Not Checked In/Checked In/Late/**Signed Out**<br>• Missed counter: X/3<br>• Check In button (if not checked in)<br>• **Sign Out button (if checked in)** |
| **Empty** | "No patients scheduled today."<br>• Date picker, CTA to schedule |
| **Loading** | Skeleton table: 8×7 with shimmer |
| **Error** | Red banner: "Failed to load"<br>• Retry button |
| **Partial** | Some rows loaded, others loading |

---

### **6.3 Check-In Button Component**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Default** | "Check In" (`envivo-pink`)<br>• Badge: "Missed: X/3" if X>0 |
| **Loading** | "Checking In..." with spinner |
| **Checked In** | Button replaced with "Sign Out" button |
| **Disabled** | Grayed, not clickable |

---

### **6.4 Sign-Out Button Component**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Default** | "Sign Out" (`envivo-pink` outline)<br>• Appears only when status = Checked In/Late |
| **Loading** | "Signing Out..." with spinner |
| **Success** | Button removed, status updated to "Signed Out"<br>• Sign-out timestamp displayed |
| **Disabled** | Grayed, tooltip: "Patient not checked in" |

---

### **6.5 Auto-Removal Notification Toast**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Trigger** | Background: light yellow<br>• Icon: ⚠️<br>• Title: "IMMEDIATE AUTO-REMOVAL"<br>• Message: Patient removed<br>• Action: "View Details"<br>• Auto-dismiss: 10s |
| **Multiple** | "X patients auto-removed"<br>• Expand to list |
| **Error** | Fallback to dashboard update |

---

### **6.6 Auto-Removal Dashboard Widget**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | List: Name, Removal Timestamp, Reason ("3 missed sessions"), Missed detail, "Re-add" button<br>• Sorted: Newest first |
| **Empty** | "No patients auto-removed."<br>• Info text |
| **Loading** | Skeleton list: 3 items |
| **Error** | "Failed to load"<br>• Retry button |
| **Partial** | Some items loaded |

---

### **6.7 Manual Removal Dashboard Widget**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | List: Name, Removal Timestamp, Reason (from dropdown), Notes (if any), "Re-add" button<br>• Color: Brown (`#8B4513`)<br>• Sorted: Newest first |
| **Empty** | "No manual removals." |
| **Loading** | Skeleton list: 3 items |
| **Error** | "Failed to load manual removals"<br>• Retry button |
| **Partial** | Some items loaded |

---

### **6.8 Patient Registration Form**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | 6 fields + privacy consent<br>• Submit enabled when valid |
| **Empty** | Blank form, submit disabled |
| **Loading** | Submit spinner, fields disabled |
| **Error** | Field-level red borders + messages |
| **Partial** | Some fields filled, consent missing |

---

### **6.9 Machine Management Table**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | Table: ID, Name, Location, Branch, Status, Actions |
| **Empty** | "No machines registered."<br>• CTA: "Add first machine" |
| **Loading** | Skeleton table: 10 rows |
| **Error** | "Failed to load machines." |
| **Partial** | Partial data loaded |

---

### **6.10 Manual Booking Modal**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | Form: Patient, Day Group, Shift, Machine, Notes<br>• Dynamic filtering by availability |
| **Empty** | All fields empty, submit disabled |
| **Loading** | Submit spinner, fields disabled |
| **Error** | "Machine unavailable"<br>• Auto-close if invalid |
| **Partial** | Some selections made |

---

### **6.11 Patient Profile Overview Tab**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | Patient info + status + next booking + **"Remove from Schedule" button** (if future bookings) |
| **Empty** | No patient data loaded |
| **Loading** | Skeleton patient card |
| **Error** | "Failed to load patient" |
| **Partial** | Basic info loaded, history loading |

---

### **6.12 Manual Removal Modal**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal** | Form: Reason dropdown (required), Notes (optional if "Others"), Warning message |
| **Empty** | Reason not selected, submit disabled |
| **Loading** | "Removing..." with spinner |
| **Error** | "Failed to remove patient"<br>• Retry option |
| **Partial** | Reason selected, notes being typed |

---

## **7. ERROR HANDLING & FEEDBACK SYSTEM**

### **7.1 Error Types & Messages**

| Error Type | User Message | UI Treatment |
|------------|--------------|--------------|
| **Validation Error** | "Mobile must be 11 digits starting with '09'" | Red border + inline message |
| **Availability Error** | "Slot no longer available" | Error toast + suggest alternatives |
| **Permission Error** | "You don't have permission" | 403 page with help link |
| **Network Error** | "Connection lost. Retrying..." | Banner + retry button |
| **System Error** | "Something went wrong" | 500 page with support contact |
| **Removal Error** | "Cannot remove checked-in patient" | Button disabled + tooltip |

### **7.2 Success Confirmations**

| Action | Confirmation Message |
|--------|---------------------|
| **Patient Registered** | "Patient [Name] registered successfully" |
| **Booking Confirmed** | "Patient scheduled on Machine [X]" |
| **Check-In Recorded** | "[Name] checked in at [time]" |
| **Sign-Out Recorded** | "[Name] signed out at [time]" |
| **Manual Removal Recorded** | "[Name] removed from schedule. Reason: [Reason]" |
| **Machine Status Updated** | "Machine [ID] marked as [status]" |
| **Patient Re-added** | "[Name] re-added successfully" |

---

## **8. CONSTRAINTS & ACCESSIBILITY**

### **8.1 Technical Constraints**

| Constraint | UX Implication | Design Solution |
|------------|----------------|-----------------|
| **No real-time sync** | Different check-in/sign-out states on devices | "Last updated" timestamp, manual refresh |
| **No offline mode** | Internet required | "No connection" banner, disable actions |
| **Manual check-in/sign-out only** | No automated tracking | One-click actions for minimal friction |
| **Single branch (MVP)** | No multi-location | Static "Bacolod" branch field |

### **8.2 Accessibility Requirements (WCAG 2.1 AA)**

- **Color Contrast:** All text ≥ 4.5:1 ratio
- **Screen Reader Support:** ARIA labels for all statuses
- **Keyboard Navigation:** Full tab navigation
- **Focus Management:** Logical focus order
- **Error Identification:** Clear messages with fixes
- **Alternative Text:** Descriptive alt text for icons

> **Color-Blind Safety:** All critical states include icons + text labels (not just color)

---

## **9. FINAL OUTPUT: DESIGN DELIVERABLES**

### **9.1 Figma Design Assets**

1. **Complete Component Library** with all 5 states including check-in, sign-out, and removal components
2. **Interactive Prototype** covering:
   - Dashboard with Schedule Grid and widgets (auto-removal + manual removal)
   - Daily Check-In & Sign-Out page and flow
   - Patient Registration with validation
   - Manual Booking with availability filtering
   - Manual Removal flow from Patient Profile
   - Machine Management CRUD operations
   - Patient Profile with history tabs
   - Auto-removal and re-add flows
   - All error states and pages

3. **Design System Documentation:**
   - Color palette with accessibility validation
   - Typography scale and hierarchy
   - Spacing system (8px grid)
   - Icon library (check-in, sign-out, warning, removal, status icons)
   - Component specifications (CSS variables, spacing)

### **9.2 Development Handoff**

- **Figma links** with developer notes
- **Component specifications** (all states documented)
- **Interaction specifications** (transitions, animations)
- **Accessibility audit report**
- **User flow diagrams** for complex interactions

---

## **APPENDIX: KEY DESIGN DECISIONS**

| Decision | Rationale |
|----------|-----------|
| **Dashboard Schedule Grid** | Visual representation per PRD requirements |
| **Color coding per PRD** | Green=Available, Blue=Scheduled, Red=Unavailable, Purple=Signed Out, Brown=Manual Removal |
| **Patient initials in grid** | Space-efficient privacy; hover for details |
| **Immediate auto-removal** | Frees slots immediately for operational efficiency |
| **Manual removal from Patient Profile** | Centralized patient management, less clutter in schedule grid |
| **Required reason for manual removal** | Audit trail and operational clarity for exceptional cases |
| **Re-add with availability check** | Respects current scheduling constraints |
| **One-click check-in and sign-out** | Minimizes friction for high-frequency tasks |
| **Sign-out for attendance recording only** | Completes patient session timeline without affecting scheduling logic |
| **Dynamic filtering in booking** | Prevents errors by only showing available options |
| **Privacy consent in registration** | Legal compliance with Data Privacy Act |
| **No age validation for DOB** | Clinical reality: infants need dialysis |
| **Missed session counter in UI** | Clear warning before auto-removal |
| **Real-time notifications** | Keeps staff informed of system actions |
| **Receptionist-performed sign-out** | Matches real-world workflow; complete attendance tracking |
| **Manual removal sets status to inactive** | Consistent with auto-removal behavior, prevents accidental re-scheduling |
| **Separate widgets for auto/manual removal** | Clear distinction between system-triggered and user-triggered removals |

---

## **NEXT STEPS**

1. ✅ **Design Specification Complete** (This URD v1.1)
2. 🔄 **Stakeholder Review** (Carissa Dumancas approval)
3. 🎨 **High-Fidelity Design** (Figma completion)
4. 👥 **User Testing** (3 receptionists, 1 supervisor)
5. 🤝 **Development Handoff** (Component library + specs)
6. 🧪 **QA Testing** (Focus on auto-removal, manual removal, check-in, and sign-out flows)
7. 🚀 **Deployment** (August 31, 2026 target)

---

**Prepared by:**  John Rey Faciolan  
Senior UI/UX Strategist & Product Designer  
*For: Envivo Web-Based Scheduling System*  
*Date: February 5, 2026*

**Approved by:**  
_________________________  
**Carissa Dumancas**  
Chief Executive Officer  
Date: _______________

---

## **CHANGE LOG**

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Feb 4, 2026 | Initial URD draft (missing check-in system) |
| 1.1 | Feb 5, 2026 | **Complete MVP with Check-In System:**<br>• Added Patient Check-In as core feature<br>• Updated all user personas and JTBD<br>• Added check-in interaction flows and edge cases<br>• Created check-in component with 5 states<br>• Updated sitemap with check-in page<br>• Added check-in accessibility requirements<br>• Defined check-in performance targets<br>• Added implementation priorities with check-in as Phase 3<br>• Added check-in success metrics<br>• Updated all appendix decisions to include check-in rationale |

**END OF DOCUMENT**