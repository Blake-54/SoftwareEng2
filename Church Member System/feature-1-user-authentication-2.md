# Feature: User Authentication & Session Management

**Feature ID:** 1
**Branch pattern:** `feature/1-user-authentication`
**Status:** Ready
**Created:** 2026-09-11
**Input:** Church staff need to sign in securely before they can view or manage member, attendance, and visitor records.
**Depends on:** none

---

## User Stories

### US-1.1: Sign in
**As a** church staff member
**I want to** sign in with a username and password
**So that** I can access member and attendance records securely
**Priority:** P1
**Independent test:** Sign in with known credentials and receive a session token, then land on the dashboard
**Acceptance scenarios:** see ### US-1.1 under Acceptance Criteria

### US-1.2: Stay signed in across page loads
**As a** signed-in staff member
**I want** my session to persist while I'm using the app
**So that** I don't have to log in again every time I load a page
**Priority:** P2
**Independent test:** Refresh the page while signed in and remain authenticated without re-entering credentials
**Acceptance scenarios:** see ### US-1.2 under Acceptance Criteria

### US-1.3: Sign out
**As a** signed-in staff member
**I want to** sign out
**So that** I can end my session, especially on a shared computer
**Priority:** P1
**Independent test:** Click sign out and confirm the session token is cleared and I'm redirected to the login page
**Acceptance scenarios:** see ### US-1.3 under Acceptance Criteria

### US-1.4: Block unauthenticated access
**As the** application
**I want to** block requests that don't include a valid session
**So that** member data stays private and only staff can see it
**Priority:** P1
**Independent test:** Request a protected route/endpoint with no session token and receive a `401`
**Acceptance scenarios:** see ### US-1.4 under Acceptance Criteria

---

## Requirements

### Functional Requirements

- **FR-001**: System MUST require a username and password to create a session.
- **FR-002**: Passwords MUST be stored only as hashed values (e.g. bcrypt); the system MUST NOT store plain-text passwords.
- **FR-003**: All member, attendance, and visitor endpoints MUST require a valid session (authenticate middleware).
- **FR-004**: Unauthenticated requests to any protected endpoint MUST receive `401`.
- **FR-005**: Usernames MUST be unique across staff accounts.
- **FR-006**: Session tokens MUST expire after 24 hours of inactivity, requiring the staff member to sign in again.

---

## Initial Data Model

### Key Entities

- **StaffUser**: a church staff member's login account; used to authenticate and gain access to member, attendance, and visitor data.

### Data Model Requirements

#### `staff_users` table

| Field | Type | Rules |
|-------|------|-------|
| `id` | INTEGER PK | Auto-increment |
| `username` | STRING(100) | Required, unique |
| `password` | STRING(255) | Required; stored as hash only |
| `createdAt` | DATE | Timestamp |
| `updatedAt` | DATE | Timestamp |

---

## Acceptance Criteria

### US-1.1 — Sign in

#### Scenario: Staff member signs in with valid credentials
* **Given** I am on the login page
* **And** a staff account exists with username `pnorth` and a known password
* **When** I enter username `pnorth` and the correct password
* **And** I click **Sign in**
* **Then** the API returns `200` with a payload containing a session token
* **And** I am redirected to the dashboard

#### Scenario: Staff member signs in with an invalid password
* **Given** I am on the login page
* **And** a staff account exists with username `pnorth`
* **When** I enter username `pnorth` and an incorrect password
* **And** I click **Sign in**
* **Then** the API returns `401` with `{ "message": "Invalid username or password." }`
* **And** I remain on the login page

---

### US-1.2 — Stay signed in across page loads

#### Scenario: Session persists after a page refresh
* **Given** I have signed in successfully
* **When** I refresh the page
* **Then** I remain signed in
* **And** I am not redirected to the login page

---

### US-1.3 — Sign out

#### Scenario: Staff member signs out
* **Given** I am signed in
* **When** I click **Sign out**
* **Then** my session token is cleared
* **And** I am redirected to the login page
* **And** subsequent requests to protected endpoints return `401`

---

### US-1.4 — Block unauthenticated access

#### Scenario: Unauthenticated request to a protected endpoint
* **Given** I have no valid session token
* **When** I request a protected endpoint (e.g. `GET /members`)
* **Then** the API returns `401` with an unauthorized message

#### Scenario: Unauthenticated user is redirected from the dashboard
* **Given** I have no session in `localStorage`
* **When** I navigate to the dashboard
* **Then** I am redirected to the login page
