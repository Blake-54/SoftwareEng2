# Feature: Attendance Tracking

**Feature ID:** 3
**Branch pattern:** `feature/3-attendance-tracking`
**Status:** Ready
**Created:** 2026-09-11
**Input:** Staff need to record which members attended each service or event so leadership can see who attends regularly.
**Depends on:** [Feature 2 — Member Profile Management](feature-2-member-profile-management.md)

---

## User Stories

### US-3.1: Create a service to track
**As a** signed-in staff member
**I want to** create a service or event record with a name and date
**So that** attendance can be logged against a specific gathering
**Priority:** P1
**Independent test:** Create a service with a name and date; it appears in the service list
**Acceptance scenarios:** see ### US-3.1 under Acceptance Criteria

### US-3.2: Record attendance for members
**As a** signed-in staff member
**I want to** mark which members attended a given service
**So that** I can track regular attendance for the congregation
**Priority:** P1
**Independent test:** Mark a member present for a service and confirm the record is saved
**Acceptance scenarios:** see ### US-3.2 under Acceptance Criteria

### US-3.3: View attendance for a service
**As a** signed-in staff member
**I want to** view the list of members marked present for a specific service
**So that** I know who showed up on a given date
**Priority:** P1
**Independent test:** Open a service and see the list of members marked present
**Acceptance scenarios:** see ### US-3.3 under Acceptance Criteria

### US-3.4: View a member's attendance history
**As a** signed-in staff member
**I want to** view a single member's attendance history
**So that** I can identify regular attenders and members who have stopped attending
**Priority:** P2
**Independent test:** Open a member's profile and see what they have attended
**Acceptance scenarios:** see ### US-3.4 under Acceptance Criteria

### US-3.5: Correct an attendance record
**As a** signed-in staff member
**I want to** remove an attendance record entered by mistake
**So that** attendance data stays accurate
**Priority:** P3
**Independent test:** Remove a mistakenly added attendance record and confirm it no longer appears
**Acceptance scenarios:** see ### US-3.5 under Acceptance Criteria

---

## Requirements

### Functional Requirements

- **FR-001**: All service and attendance endpoints MUST require a valid staff session.
- **FR-002**: A service record MUST include a name and a date; both are required.
- **FR-003**: An attendance record MUST link exactly one member to exactly one service.
- **FR-004**: The system MUST NOT allow more than one attendance record for the same member and the same service.
- **FR-005**: Any member, regardless of `active` or `inactive` status, MAY be marked present for a service (e.g. an inactive member visiting occasionally).
- **FR-006**: Absence MUST be represented by the lack of an attendance record; the system MUST NOT store an explicit "absent" row for every member who did not attend.

---

## Initial Data Model

### Key Entities

- **Service**: a specific church gathering (e.g. "Sunday Morning Service", "Wednesday Night") with a name and date.
- **AttendanceRecord**: links one Member to one Service, indicating they were present.

### Data Model Requirements

#### `services` table

| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `name` | STRING(100) | Required (e.g. "Sunday Morning Service") |
| `date` | DATE | Required |
| `createdAt` | DATE | Timestamp |
| `updatedAt` | DATE | Timestamp |

#### `attendance_records` table

| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `memberId` | INTEGER FK | Required; references `members.id` |
| `serviceId` | INTEGER FK | Required; references `services.id` |
| `present` | BOOLEAN | Default `true` |
| `createdAt` | DATE | Timestamp |
| `updatedAt` | DATE | Timestamp; unique constraint on (`memberId`, `serviceId`) |

---

## Acceptance Criteria

### US-3.1 — Create a service to track

#### Scenario: Staff creates a new service
* **Given** I am signed in
* **When** I create a service with name `Sunday Morning Service` and date `2026-09-13`
* **Then** the API returns `201` with a service object containing `id`, `name`, and `date`
* **And** `Sunday Morning Service` appears in the service list

#### Scenario: Staff attempts to create a service without a date
* **Given** I am signed in
* **When** I submit a new service with a name but no date
* **Then** the API returns `400` with `{ "message": "Date is required." }`
* **And** no service record is created

---

### US-3.2 — Record attendance for members

#### Scenario: Staff marks a member present for a service
* **Given** I am signed in
* **And** service `Sunday Morning Service` exists
* **And** member `David North` exists
* **When** I mark `David North` present for `Sunday Morning Service`
* **Then** the API returns `201` with an attendance record linking his `memberId` and the service's `serviceId`

#### Scenario: Staff attempts to mark the same member present twice for the same service
* **Given** I am signed in
* **And** `David North` is already marked present for `Sunday Morning Service`
* **When** I attempt to mark him present for that same service again
* **Then** the API returns `400` with `{ "message": "Attendance already recorded for this member and service." }`

---

### US-3.3 — View attendance for a service

#### Scenario: Staff views the attendance list for a specific service
* **Given** I am signed in
* **And** `David North` and `John Smith` are marked present for `Sunday Morning Service`
* **When** I open the attendance view for `Sunday Morning Service`
* **Then** I see both `David North` and `John Smith` listed as present

---

### US-3.4 — View a member's attendance history

#### Scenario: Staff views a member's attendance across multiple services
* **Given** I am signed in
* **And** `David north` is marked present for two different services
* **When** I open `David North`'s profile
* **Then** I see both services listed in his attendance history

---

### US-3.5 — Correct an attendance record

#### Scenario: Staff removes an incorrectly recorded attendance entry
* **Given** I am signed in
* **And** `David North` was mistakenly marked present for `Sunday Morning Service`
* **When** I remove that attendance record
* **Then** the API returns `200` confirming deletion
* **And** `David North` no longer appears in the attendance list for that service
