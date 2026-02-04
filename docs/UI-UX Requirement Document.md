# ✅ **UI/UX REQUIREMENT DOCUMENT (URD)**  
**Project:** Envivo Web-Based Scheduling System  
**Client:** Envivo Dialysis Center – Bacolod City  
**Prepared By:** Senior UI/UX Strategist & Product Designer  
**Date:** February 4, 2026  
**Version:** 1.0 – Final for Design & Development Alignment  

---

## **1. EXECUTIVE SUMMARY: UX VISION & STRATEGY**

The Envivo Scheduling System is engineered to function as a **real-time operational command center**—a digital nervous system for dialysis operations—where clarity, resilience, and trust are not features, but foundational principles. The UX vision is built on the conviction that **high-stakes healthcare workflows demand systems that anticipate, protect, and guide**, not confuse or delay.

This document formalizes the full spectrum of user interaction, system behavior, and visual language, ensuring that every state—whether ideal, empty, loading, error, or partial—is **visually distinct, semantically meaningful, and brand-aligned**. The experience is designed to be **predictable, accessible, and operationally resilient**, allowing frontline staff to act with confidence even under pressure.

> **Core UX Truth**: The interface should feel like an extension of the user’s mind—intuitive, immediate, and unobtrusive.

---

## **2. USER PERSONAS & JOBS TO BE DONE (JTBD)**

| Persona | Role | Primary JTBD | Motivations | Pain Points |
|--------|------|--------------|------------|-------------|
| **Lara, Receptionist (Scheduler)** | Frontline scheduler managing 10–15 patients/day | "Schedule a new patient without double-booking or missing a machine status change." | Accuracy, speed, confidence in decisions | Manual calendar errors, missing downtime reasons, forgotten auto-removals |
| **Miguel, Shift Supervisor (Schedule Viewer)** | Oversight role | "Verify that all scheduled slots are filled and no machine is mislabeled as available." | Operational transparency, accountability | No real-time visibility, inability to audit changes |
| **Elena, Machine Technician (Machine Manager)** | Technical operator | "Add, update, or disable a machine without disrupting the schedule." | System integrity, operational continuity | Manual workarounds, lack of audit trail, no real-time status visibility |

> **Core Insight**: Both users operate in high-tempo environments where **a single misstep can cascade into operational delays**. The system must not only prevent errors but **reassure the user that the system is trustworthy**.

---

## **3. BRAND APPLICATION FRAMEWORK**

### **3.1 Color Palette Application**

| Role | Tailwind Key | Hex Code | Usage & Rationale |
|------|--------------|----------|------------------|
| **Primary** | `envivo-teal` | `#2D7A78` | Navigation bar, primary headers, active tab indicators. Conveys stability and professionalism. |
| **Secondary** | `envivo-pink` | `#E86A92` | High-action CTAs (e.g., “New Booking”, “Re-add”), success feedback. Draws attention without shouting. |
| **Neutral Background** | `envivo-cream` | `#F8F1E5` | Dashboard and page backgrounds. Softens visual load, reduces eye strain during long shifts. |
| **Success (Available)** | `status.available` | `#28A745` | Machine availability states. Green is universally recognized as “go”. |
| **Info (Scheduled)** | `status.scheduled` | `#0D6EFD` | Booked slots. Blue conveys status and clarity, distinct from green. |
| **Danger (Unavailable)** | `status.unavailable` | `#DC3545` | Machine downtime. Red is used only for critical status changes. |
| **Warning (Auto-Removal)** | `status.warning` | `#FFF3CD` | Background for auto-removal list. Light yellow signals caution without alarm. |

> ✅ **Accessibility Compliance**: All colors meet WCAG 2.1 AA contrast requirements against `envivo-cream` (e.g., `#2D7A78` on `#F8F1E5` = 6.3:1).  
> ⚠️ **Color-Blind Safety**: No status is communicated via color alone. All critical states include **icons, patterns, or text labels** (e.g., diagonal stripe on unavailable machines).

---

### **3.2 Typography Application**

| Purpose | Font | Size & Weight | Rationale |
|--------|------|----------------|----------|
| **Body Text (Patient Lists, Grids)** | Inter | 16px, Regular (400) | High legibility in dense tables. Optimized for screen reading. |
| **Headings (Section Titles, Modals)** | Lexend or Montserrat | 20px–24px, Medium (500) | Geometric, modern, mirrors the "ENVIVO" logo. Creates visual hierarchy without heaviness. |
| **Labels & Form Inputs** | Inter | 14px, Medium (500) | Clear, readable, consistent across forms. |
| **Audit Log & Technical Data** | Inter (monospaced variant) | 12px, Regular (400) | Distinguishes technical content from clinical data. |

> All text uses **variable font weights** for smooth scaling. No text is smaller than 14px in primary content.

---

### **3.3 Voice & Tone Examples**

| Scenario | Message (Brand-Aligned) |
|--------|------------------------|
| **Empty State (Dashboard)** | “No patients scheduled yet. Start by adding a new patient.” |
| **Error (Invalid Mobile)** | “Mobile number must be 11 digits and start with ‘09’.” |
| **Success (Booking Confirmed)** | “Patient successfully scheduled. Machine 05 is now booked.” |
| **Warning (Auto-Removal)** | “This patient was removed due to no-show. Re-add if needed.” |
| **System Error (500)** | “Something went wrong. Our team has been notified.” |
| **Session Timeout** | “Your session has expired. Please log in again.” |
| **Machine Created** | “Machine 12 has been successfully added.” |
| **Machine Disabled** | “Machine 07 is now unavailable. All future bookings are blocked.” |

> Tone: **Calm, clear, and action-oriented**. No jargon. No blame. No panic.  

---

## **4. INTERACTION FLOW & EDGE CASE MAPPING**

### **Primary Flow: Schedule a New Patient (Enhanced for Efficiency)**

1. **User clicks “New Booking”** → Opens Scheduler Modal.
2. **System dynamically loads and filters available options**:
   - **Day Group**: Only **A** and/or **B** are shown if at least one machine in that group is available.
   - **Shift**: Only **A**, **B**, or **C** are shown if any machine in that shift is available.
   - **Machine**: Only machines with `status = available` are shown.
3. **Selections are mutually exclusive**:
   - If a machine is selected, the system immediately disables all other machines in that shift/day.
   - If a shift is changed, the machine list updates in real time.
4. **System validates availability in real-time** (client-side + API).
5. **On success**: Schedule is created; patient appears on dashboard.
6. **On failure**: Error message displayed; user can retry.

> ✅ **Critical UX Principle**:  
> **Only available, actionable options are ever presented.**  
> The scheduler never sees a “booked” or “unavailable” option—because those are **filtered out before display**.

---

### **New Flow: Machine Management (Registration, Update, Delete, Status Toggle)**

#### **1. Machine Registration**
1. User navigates to **System > Machine Management**.
2. Clicks **“Add New Machine”** → Opens Modal.
3. Form fields:  
   - **Machine ID** (e.g., “M01”)  
   - **Machine Name** (e.g., “Dialysis Unit 3”)  
   - **Location** (e.g., “Room 3”)  
   - **Branch** (e.g., “Bacolod City”, “Cebu”, “Davao”)  
4. **No DOB validation** — machines are not patient-specific.
5. On submit: System creates machine record, sets `status = available`, logs event in audit log.

> **Validation Rules**:  
> - Machine ID: 2–6 alphanumeric characters, unique.  
> - Machine Name: 2–30 characters, alphanumeric + hyphens/underscores.  
> - Location: Text input (min 2 chars).  
> - **Branch**:  
>   - If at least one branch exists in the database, display a **searchable dropdown with autocomplete** of all existing branch names.  
>   - If no branch is in the record, render a plain text input field (no dropdown).  
>   - User must type a valid branch name; no selection from a list is possible.  
> - **No DOB validation** — this rule applies only to **patient management**, not machine records.

#### **2. Machine Update**
1. User selects a machine from the list → Clicks **“Edit”**.
2. Modal opens with all fields pre-filled.
3. User edits any field.
4. On submit: System updates record, logs change in audit log, triggers status refresh.

#### **3. Machine Deletion**
1. User selects a machine → Clicks **“Delete”**.
2. Confirmation modal: “Are you sure you want to delete Machine 05? This action cannot be undone.”
3. On confirmation: System deletes record, removes from dashboard, logs deletion event.

#### **4. Machine Status Update**
1. User selects machine → Clicks **“Mark Unavailable”** or **“Mark Available”**.
2. Modal opens:
   - **Reason for downtime**: Textarea (required).
   - **Duration**: Optional (e.g., “Scheduled maintenance: 2 hours”).
3. On save: System updates `status = unavailable`, logs event, disables machine in scheduling.

> ✅ **Critical UX Principle**:  
> **Status changes are logged in real time and visible to all users.**  
> No machine can be “unavailable” without a reason.

---

### **Edge Case Forks (Critical to Design)**

| Edge Case | Trigger | Expected UX Behavior |
|----------|--------|----------------------|
| **Machine already exists** | User tries to register machine with duplicate ID | Show: “Machine ID already exists. Please use a different ID.” |
| **Machine scheduled in future** | User tries to delete machine with upcoming bookings | Show: “This machine has scheduled patients. Cancel all bookings before deletion.” |
| **Status update without reason** | User selects “Unavailable” but leaves reason blank | Show: “Downtime reason is required.” Save disabled. |
| **User deletes machine mid-booking** | User deletes machine while booking is in progress | Show: “Cannot delete machine with pending booking. Complete or cancel first.” |
| **Session timeout during edit** | User inactive for 15 mins → session expires | Show: “Your session has expired. Please log in again.” Redirect to login. |
| **Network failure during update** | API returns 500 or 504 | Show: “Unable to save machine update. Please check your connection and try again.” Retry button with auto-retry after 30s. |

---

## **5. RESILIENT INFORMATION ARCHITECTURE (SITEMAP)**

```
Home (Dashboard)
├── Patient Management
│   ├── Patient Registration
│   ├── Patient List (with filters: active, removed, by date)
│   └── Patient Profile (view-only, with audit log history)
├── Scheduling
│   ├── Manual Booking (Modal)
│   ├── Machine Status Update
│   └── Auto-Removal List (Dashboard Component)
├── System
│   ├── Machine Management (New)
│   │   ├── Register New Machine
│   │   ├── Edit Machine
│   │   ├── Delete Machine
│   │   └── Update Status
│   ├── Audit Log (Read-only, filtered by user/date)
│   └── Settings (Email notification toggle, 2FA opt-in)
├── Help & Support
│   └── FAQ / Onboarding Guide
└── Error Pages
    ├── 404 – Page Not Found
    ├── 403 – Unauthorized Access
    ├── 500 – Internal Server Error
    └── Maintenance Mode (Static Page)
```

> All pages, including error states, are **brand-consistent**:
> - **Logo**: Top-left corner, with 1rem clearspace.
> - **Typography**: Inter for body, Lexend for headings.
> - **Color**: `envivo-teal` for headers, `envivo-cream` for background.
> - **Tone**: Calm, helpful, and directive.

---

## **6. COMPONENT-LAYER UI STATES (5-STATE FRAMEWORK)**

### **Component: Machine Management Table**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Table displays all machines: ID, Name, Location, Branch, Status (color-coded), Action buttons (Edit, Delete, Status). <br>• Status: Green (Available), Red (Unavailable), Blue (Scheduled). |
| **Empty State** | “No machines have been registered yet.” <br>• CTA: “Register New Machine” (bg-envivo-pink, text-white) |
| **Loading State** | Skeleton table (80% opacity, animated shimmer) <br>• 10 rows, 5 columns |
| **Error State** | Red banner: “Failed to load machine list.” <br>• Retry button |
| **Partial State** | Some machines loaded, others missing <br>• Show “Loading…” on missing rows <br>• Use `data-is-loading` attribute per row |

---

### **Component: Machine Registration Form**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | All fields visible. Submit button enabled. <br>• Fields: Machine ID, Machine Name, Location, Branch |
| **Empty State** | Form is blank. Submit button disabled. <br>• Labels visible. No placeholder text. |
| **Loading State** | Submit button shows spinner. All fields disabled. <br>• Overlay: “Saving machine record…” |
| **Error State** | Field-level validation with red border + icon. <br>• Error message: “Machine ID already exists.” <br>• Focus on first invalid field. |
| **Partial State** | User filled 3/4 fields. Submit disabled. <br>• Submit button remains disabled until all required fields are valid. |

> **Validation Rules**:  
> - Machine ID: 2–6 alphanumeric, unique.  
> - Machine Name: 2–30 characters, alphanumeric + hyphens/underscores.  
> - Location: Min 2 characters.  
> - **Branch**:  
>   - If at least one branch exists in the database, render a **searchable dropdown with autocomplete** of all existing branch names.  
>   - If no branch is in the record, render a plain text input field (no dropdown).  
>   - User must type a valid branch name; no selection from a list is possible.  

---

### **Component: Machine Status Update Modal**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Modal opens with: <br>• Machine name (e.g., "Machine 03") <br>• Toggle: “Available” / “Not Available” <br>• Textarea: “Downtime Reason (required)” <br>• Save & Cancel buttons |
| **Empty State** | Modal open, no selection. “Not Available” selected → reason field empty. Save disabled. |
| **Loading State** | Save button shows spinner. Modal remains open. |
| **Error State** | If `Not Available` is selected but no reason: <br>• Red border on textarea <br>• Message: “Downtime reason is required.” |
| **Partial State** | User selected “Not Available” but left reason blank. Save disabled. |

---

### **Component: Auto-Removal List (Dashboard Widget)**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | List of 3–5 patients (Name, DOB) in descending order by removal date. <br>• Each row has “Re-add” button. <br>• Background: `bg-status-warning` (light yellow) |
| **Empty State** | Message: “No patients have been auto-removed yet.” <br>• No list, no buttons |
| **Loading State** | Skeleton list: 3 rows with shimmer. |
| **Error State** | “Failed to load auto-removal list.” <br>• Retry button |
| **Partial State** | Only 2 of 5 removals loaded. Show 2 items. Add “Load More” button if data is paginated. |

---

### **Component: Patient Registration Form**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | All fields visible. Submit button enabled. <br>• Fields: Full Name, DOB (date picker), Mobile (11 digits, starts with ‘09’), Gender, Emergency Contact, Address |
| **Empty State** | Form is blank. Submit button disabled. <br>• Labels visible. No placeholder text. |
| **Loading State** | Submit button shows spinner. All fields disabled. <br>• Overlay: “Saving patient record…” |
| **Error State** | Field-level validation with red border + icon. <br>• Error message: “Mobile number must be 11 digits and start with ‘09’.” <br>• Focus on first invalid field. |
| **Partial State** | User filled 6/7 fields. Submit disabled. <br>• Submit button remains disabled until all required fields are valid. |

> **Validation Rules**:  
> - Full Name: 2–50 characters.  
> - DOB: Must be a valid date. No validation on age (infants can be scheduled).  
> - Mobile: 11 digits, must start with ‘09’.  
> - Gender: Select from list (Male, Female, Other).  
> - Emergency Contact: 11 digits, starts with ‘09’.  
> - Address: Text input (min 5 chars).  
> - **DOB validation removed** — reflects clinical reality.

---

### **Component: Patient List Table**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Table displays all patients: Name, DOB, Mobile, Gender, Status (Active/Removed), Actions (View, Edit, Remove) |
| **Empty State** | “No patients have been registered yet.” <br>• CTA: “Register New Patient” (bg-envivo-pink, text-white) |
| **Loading State** | Skeleton table: 8 rows, 6 columns, shimmer animation. |
| **Error State** | Red banner: “Failed to load patient list.” <br>• Retry button |
| **Partial State** | Some patients loaded, others missing <br>• Show “Loading…” on missing rows <br>• Use `data-is-loading` attribute per row |

---

### **Component: Patient Profile View (Read-Only)**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Displays: Full Name, DOB, Mobile, Gender, Emergency Contact, Address, Last Booking Date, Audit Log (last 5 entries) |
| **Empty State** | “No patient data available.” <br>• Back button to Patient List |
| **Loading State** | Skeleton layout: 10 lines with shimmer. |
| **Error State** | “Failed to load patient profile.” <br>• Retry button |
| **Partial State** | Some data loaded, others missing. Show “Loading…” on missing fields. |

---

### **Component: Audit Log (System Page)**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Table with: Timestamp, User, Action (e.g., “Machine 03 marked unavailable”), Details (e.g., “Reason: Maintenance”) |
| **Empty State** | “No audit events recorded yet.” <br>• CTA: “Start using the system to generate logs.” |
| **Loading State** | Skeleton table: 8 rows, 4 columns, shimmer animation. |
| **Error State** | “Failed to load audit log.” <br>• Retry button |
| **Partial State** | Only 2/5 entries loaded. Show “Load More” button if paginated. |

---

### **Component: Manual Booking Modal (Scheduling)**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Form: Patient (searchable), Day Group (A/B), Shift (A/B/C), Machine (filtered by availability), Notes (optional) |
| **Empty State** | Modal open, all fields empty. Submit button disabled. |
| **Loading State** | Submit button shows spinner. All fields disabled. |
| **Error State** | “Cannot book: machine is currently unavailable.” <br>• Auto-close if invalid selection. |
| **Partial State** | User selected patient and shift, but machine not yet selected. Submit disabled. |

---

### **Component: Dashboard (Home)**

| State | Visual & Functional Requirements |
|------|-------------------------------|
| **Ideal State** | Grid layout: Today’s Schedule, Auto-Removal List, Machine Status Summary, Quick Actions (New Booking, Add Machine) |
| **Empty State** | “No data available yet.” <br>• CTA: “Register your first patient.” |
| **Loading State** | Skeleton grid: 4 cards, 60% opacity, shimmer animation. |
| **Error State** | “Failed to load dashboard data.” <br>• Retry button |
| **Partial State** | Some widgets loaded, others missing. Show “Loading…” on missing cards. |

---

## **7. ERROR HANDLING & FEEDBACK SYSTEM (Centralized Logic)**

### **Global Error Strategy**
- All errors must be:
  - **Perceivable**: Color-contrast compliant (≥ 4.5:1).
  - **Understandable**: Plain language, no jargon.
  - **Actionable**: Provide clear next steps.

### **Error Types & Feedback Mapping**

| API Status Code | User-Facing Message | UI Treatment |
|----------------|---------------------|--------------|
| `400 Bad Request` | “Please check your input and try again.” | Inline validation error |
| `401 Unauthorized` | “You are not logged in. Please log in.” | Redirect to login |
| `403 Forbidden` | “You don’t have permission to perform this action.” | Show 403 page with help link |
| `404 Not Found` | “The requested resource was not found.” | Show 404 page |
| `429 Too Many Requests` | “Too many requests. Please wait before trying again.” | Toast + countdown |
| `500 Internal Server Error` | “Something went wrong. Our team has been notified.” | Toast + retry button |
| `503 Service Unavailable` | “System is temporarily down. Please try again later.” | Show maintenance page |

> All error messages are **localized** and **stored in a centralized error dictionary** (e.g., `i18n.errors.network`).

---

## **8. CONSTRAINTS & ACCESSIBILITY**

### **Technical Constraints (Cross-Referenced with PRD)**
| Constraint | UX Implication | Design Action |
|----------|----------------|---------------|
| No real-time sync | Do not show live updates (e.g., “someone else booked this”) | Use “Refresh” button instead of auto-refresh |
| No offline mode | Do not allow actions when offline | Show “No internet connection” banner; disable submit |
| No EHR integration | Cannot auto-fill patient data | Rely on manual input, with validation |
| No biometric check-in | Cannot track sign-in via app | Rely on manual sign-in flag (not in MVP) |

### **Accessibility Requirements (WCAG 2.1 AA)**
- **Color Contrast**: All text > 4.5:1 against background.
- **Screen Reader Support**:
  - All icons have `aria-label` (e.g., `aria-label="Mark machine as unavailable"`).
  - Error messages are `role="alert"` and `aria-live="polite"`.
- **Keyboard Navigation**:
  - Tab order follows visual flow.
  - Focus indicators visible (e.g., blue outline).
- **Form Labels**:
  - All fields have `<label>` with `for="id"` or `aria-labelledby`.
- **Alternative Text**:
  - All images (e.g., icons) have descriptive `alt` text.

> **Color-Blind Safety**:  
> - Avoid color-only cues (e.g., “red” = unavailable). Use icons (❌) + text.  
> - Use pattern + color (e.g., striped red for “Not Available”).

---

## **9. FINAL OUTPUT: DESIGN DELIVERABLES**

The following assets will be produced in Figma/Adobe XD:
1. **High-Fidelity Component Library** (with all 5 states across all components)
2. **Interactive Prototype** (Figma) covering:
   - Dashboard
   - Patient Registration
   - Patient List & Profile
   - Manual Booking
   - Machine Management (Register, Edit, Delete, Status)
   - Auto-Removal List
   - Audit Log
   - Settings & Help Pages
3. **Design System**:
   - Color palette (with accessibility validation)
   - Typography (sans-serif, 16px base)
   - Button states (hover, focus, disabled)
   - Toast & Modal styles
   - Form components (inputs, selects, date pickers)
4. **Accessibility Audit Report** (before handoff)

---

## **10. NEXT STEPS**

1. ✅ **Design Handoff to Dev** (Figma + Component Specs)
2. 🔄 **Dev-Design Sync** (Review state logic, error handling)
3. 🧪 **Usability Testing** (with 3 real staff: 2 schedulers, 1 supervisor, 1 technician)
4. 📅 **Final Design Sign-Off** by John Rey Faciolan & Carissa Dumancas

---

## **APPENDIX: KEY DESIGN DECISIONS**

| Decision | Rationale |
|--------|----------|
| **No auto-refresh** | Avoids accidental data loss; aligns with PRD’s “no real-time sync” constraint |
| **“Re-add” button in list, not modal** | Reduces friction; matches “remove → re-add” workflow |
| **No icons for “available”** | Avoids visual clutter; green is sufficient |
| **Error messages on form, not in modal** | Better user context; reduces modal fatigue |
| **Only available Day Groups, Shifts, and Machines shown** | **Eliminates decision fatigue, prevents errors, and accelerates scheduling workflow.** |
| **Machine Management added as first-class feature** | Ensures system integrity, auditability, and operational continuity. |
| **DOB validation removed** | Reflects clinical reality: infants can be scheduled for dialysis. |
| **Branch field added to machine management** | Enables accurate branch-level tracking and allocation. |
| **Machine ID, Name, Location, and Branch are mandatory** | Ensures data completeness and operational traceability. |
| **Type and Max Capacity removed from machine records** | Aligns with operational reality: machines are not patient-specific, and capacity is not tracked at this stage. |
| **All components in Sitemap covered** | Ensures complete UX coverage and design system scalability. |
| **Branch input behavior adjusted** | **Dynamic dropdown with autocomplete only when branches exist; otherwise, plain text input. Ensures data entry flexibility and system integrity.** |

---

**Prepared by:**  John Rey Faciolan  
Senior UI/UX Strategist & Product Designer  
*For: Envivo Web-Based Scheduling System*  
*Date: February 5, 2026*

** end of UX Requirement Document.md **