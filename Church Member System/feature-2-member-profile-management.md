# Feature: Member Profile Management

**Feature ID:** 2
**Branch pattern:** `feature/2-member-profile-management`
**Status:** Ready
**Created:** 2026-09-11
**Input:** Staff need to keep an accurate roster of church members, including personal, contact, and household information.
**Depends on:** [Feature 1 — User Authentication & Session Management](feature-1-user-authentication.md)

---

## User Stories

### US-2.1: Add a new member
**As a** signed-in staff member
**I want to** add a new member record with their name, contact info, address, and household
**So that** I can maintain an accurate roster of the congregation
**Priority:** P1
**Independent test:** Submit a new member with a first and last name and the member appears in the roster
**Acceptance scenarios:** see ### US-2.1 under Acceptance Criteria

### US-2.2: View member list
**As a** signed-in staff member
**I want to** view a list of all members
**So that** I can see everyone who belongs to the church
**Priority:** P1
**Independent test:** Load the member roster and see all existing members
**Acceptance scenarios:** see ### US-2.2 under Acceptance Criteria

### US-2.3: View member details
**As a** signed-in staff member
**I want to** open a single member's full profile
**So that** I can see their contact info, address, and household at a glance
**Priority:** P2
**Independent test:** Click a member row and see their full stored details
**Acceptance scenarios:** see ### US-2.3 under Acceptance Criteria

### US-2.4: Edit a member's information
**As a** signed-in staff member
**I want to** update a member's information
**So that** records stay accurate when people move or their details change
**Priority:** P1
**Independent test:** Edit a member's address and confirm the change is saved and displayed
**Acceptance scenarios:** see ### US-2.4 under Acceptance Criteria

### US-2.5: Deactivate a member
**As a** signed-in staff member
**I want to** mark a member as inactive
**So that** the roster reflects who is currently part of the congregation without deleting their history
**Priority:** P2
**Independent test:** Mark a member inactive and confirm their status updates without removing their record
**Acceptance scenarios:** see ### US-2.5 under Acceptance Criteria

---

## Requirements

### Functional Requirements

- **FR-001**: All member endpoints MUST require a valid staff session.
- **FR-002**: A member record MUST include a first name and a last name; both are required.
- **FR-003**: First name and last name MUST NOT exceed 100 characters each.
- **FR-004**: System MUST support an optional household field to group related members (e.g. a shared family label).
- **FR-005**: System MUST support optional contact fields: address, phone, and email.
- **FR-006**: Every member MUST have a status of either `active` or `inactive`, defaulting to `active` on creation.
- **FR-007**: Marking a member `inactive` MUST NOT delete their record or attendance history.

---

## Initial Data Model

### Key Entities

- **Member**: a person who is part of the congregation; has personal details, contact info, a household label, and an active/inactive status.

### Data Model Requirements

#### `members` table

| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `firstName` | STRING(100) | Required |
| `lastName` | STRING(100) | Required |
| `household` | STRING(100) | Optional; free-text family/household label |
| `address` | STRING(255) | Optional |
| `phone` | STRING(20) | Optional |
| `email` | STRING(255) | Optional |
| `status` | ENUM('active','inactive') | Required; default `active` |
| `notes` | TEXT | Optional |
| `createdAt` | DATE | Timestamp |
| `updatedAt` | DATE | Timestamp |

---

## Acceptance Criteria

### US-2.1 — Add a new member

#### Scenario: Staff adds a new member with required fields
* **Given** I am signed in
* **When** I submit a new member with first name `David` and last name `North`
* **Then** the API returns `201` with a member object containing `id`, `firstName`, `lastName`, and `status: "active"`
* **And** `David North` appears in the member roster

#### Scenario: Staff attempts to add a member with a missing last name
* **Given** I am signed in
* **When** I submit a new member with a first name but no last name
* **Then** the API returns `400` with `{ "message": "Last name is required." }`
* **And** no member record is created

---

### US-2.2 — View member list

#### Scenario: Staff views the full member roster
* **Given** I am signed in
* **And** members exist in the system
* **When** I load the member roster page
* **Then** I see a list of all members with their first and last names

---

### US-2.3 — View member details

#### Scenario: Staff opens a member's profile
* **Given** I am signed in
* **And** member `David North` exists with an address, phone, and household set
* **When** I open `David North`'s profile
* **Then** I see his full name, address, phone, email, household, and status

---

### US-2.4 — Edit a member's information

#### Scenario: Staff updates a member's address
* **Given** I am signed in
* **And** member `David North` exists
* **When** I update his address and save
* **Then** the API returns `200` with the updated address
* **And** the new address is displayed on his profile

#### Scenario: Staff attempts to save an edit with an empty first name
* **Given** I am signed in
* **And** member `David North` exists
* **When** I clear the first name field and attempt to save
* **Then** the API returns `400` with `{ "message": "First name is required." }`
* **And** the member's stored first name is unchanged

---

### US-2.5 — Deactivate a member

#### Scenario: Staff marks a member inactive
* **Given** I am signed in
* **And** member `David North` has status `active`
* **When** I mark him as inactive
* **Then** the API returns `200` with `status: "inactive"`
* **And** `David North`'s record and any past attendance history still exist
