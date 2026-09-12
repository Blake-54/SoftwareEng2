# Feature: Visitor Tracking

**Feature ID:** 4
**Branch pattern:** `feature/4-visitor-tracking`
**Status:** Ready
**Created:** 2026-09-11
**Input:** Staff need to log visitors who attend a service so leadership knows who is new and can follow up, separately from regular members.
**Depends on:** [Feature 1 — User Authentication & Session Management](feature-1-user-authentication.md)

---

## User Stories

### US-4.1: Log a new visitor
**As a** signed-in staff member
**I want to** record a visitor's name, contact info, and the date they visited
**So that** the church knows who came and can follow up with them
**Priority:** P1
**Independent test:** Submit a new visitor with a name and visit date; the visitor appears in the visitor list
**Acceptance scenarios:** see ### US-4.1 under Acceptance Criteria

### US-4.2: View visitor list
**As a** signed-in staff member
**I want to** see a list of all visitors and when they visited
**So that** I can track newcomers over time
**Priority:** P1
**Independent test:** Load the visitor list and see all logged visitors with their visit dates
**Acceptance scenarios:** see ### US-4.2 under Acceptance Criteria

### US-4.3: Record a repeat visit
**As a** signed-in staff member
**I want to** log another visit date for a person who has already visited before
**So that** I can see how many times they've come before joining as a member
**Priority:** P2
**Independent test:** Add a second visit date to an existing visitor without creating a duplicate visitor record
**Acceptance scenarios:** see ### US-4.3 under Acceptance Criteria

### US-4.4: Convert a visitor to a member
**As a** signed-in staff member
**I want to** convert a visitor's record into a full member record
**So that** I don't have to re-enter their information when they decide to join the church
**Priority:** P3
**Independent test:** Convert an existing visitor and confirm a new member record is created using their stored name and contact info
**Acceptance scenarios:** see ### US-4.4 under Acceptance Criteria

---

## Requirements

### Functional Requirements

- **FR-001**: All visitor endpoints MUST require a valid staff session.
- **FR-002**: A visitor record MUST include a first name, last name, and at least one visit date; all are required.
- **FR-003**: System MUST support optional contact fields: phone, email, and how they heard about the church.
- **FR-004**: A returning visitor MUST be recorded as an additional visit date on their existing visitor record, not as a new duplicate visitor. Staff MUST identify the existing record by matching first name and last name.
- **FR-005**: Converting a visitor to a member MUST create a new member record pre-filled with the visitor's stored name and contact info, without requiring re-entry.
- **FR-006**: A visitor that has been converted MUST be linked to the resulting member record so staff can see the connection.

---

## Initial Data Model

### Key Entities

- **Visitor**: a person who attended a service but is not yet a member; has a name, contact info, and one or more visit dates.
- **VisitorVisit**: a record of one specific date a visitor attended, allowing repeat visits to be tracked under a single visitor.

### Data Model Requirements

#### `visitors` table

| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `firstName` | STRING(100) | Required |
| `lastName` | STRING(100) | Required |
| `phone` | STRING(20) | Optional |
| `email` | STRING(255) | Optional |
| `howHeard` | STRING(255) | Optional |
| `convertedToMemberId` | INTEGER FK | Optional; references `members.id`; set when converted |
| `createdAt` | DATE | Timestamp |
| `updatedAt` | DATE | Timestamp |

#### `visitor_visits` table

| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `visitorId` | INTEGER FK | Required; references `visitors.id` |
| `visitDate` | DATE | Required |
| `createdAt` | DATE | Timestamp |
| `updatedAt` | DATE | Timestamp |

---

## Acceptance Criteria

### US-4.1 — Log a new visitor

#### Scenario: Staff logs a new visitor with required fields
* **Given** I am signed in
* **When** I submit a new visitor with first name `David`, last name `North`, and visit date `2026-09-13`
* **Then** the API returns `201` with a visitor object containing `id`, `firstName`, `lastName`, and a linked visit date
* **And** `David North` appears in the visitor list

#### Scenario: Staff attempts to log a visitor without a visit date
* **Given** I am signed in
* **When** I submit a new visitor with a name but no visit date
* **Then** the API returns `400` with `{ "message": "Visit date is required." }`
* **And** no visitor record is created

---

### US-4.2 — View visitor list

#### Scenario: Staff views the list of all visitors
* **Given** I am signed in
* **And** visitors exist in the system
* **When** I load the visitor list page
* **Then** I see each visitor's name and their most recent visit date

---

### US-4.3 — Record a repeat visit

#### Scenario: Staff logs a second visit for an existing visitor
* **Given** I am signed in
* **And** visitor `David North` already has one recorded visit
* **When** I log a new visit date for `David North`
* **Then** the API returns `201` with a new visit date linked to the existing `David North` visitor record
* **And** `David North` now shows two visit dates
* **And** no duplicate `David North` visitor record is created

---

### US-4.4 — Convert a visitor to a member

#### Scenario: Staff converts a returning visitor into a member
* **Given** I am signed in
* **And** visitor `David North` exists with a phone number and email on file
* **When** I convert `David North` to a member
* **Then** the API returns `201` with a new member record for `David North` pre-filled with his phone and email
* **And** the visitor record for `David North` is linked to the new member record
