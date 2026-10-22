# Feature Specification: Team Management

**Feature Branch**: `soccer-league`
**Created**: 2026-10-21
**Status**: Draft
**Input**: League admins need to create and manage the teams in the soccer league.

---

## User Stories

### User Story 1 - Create a Team (Priority: P1)
As a **League Admin**, I want to create a new team with a name, age division, and coach, so that the team can join the league.

### User Story 2 - Edit a Team (Priority: P2)
As a **League Admin**, I want to edit a team's information, so that I can fix mistakes or change the coach.

### User Story 3 - View All Teams (Priority: P1)
As a **League Admin, Coach, or Parent**, I want to see a list of all teams in the league, so that I know who is playing this season.

### User Story 4 - Remove a Team (Priority: P3)
As a **League Admin**, I want to remove a team that dropped out, so that it does not show up in the schedule or standings.

---

## Functional Requirements

- **FR-001**: System MUST allow a League Admin to create a team with a name, division, and coach.
- **FR-002**: System MUST NOT allow two teams with the same name in the same division.
- **FR-003**: System MUST allow a League Admin to edit a team's name, division, and coach.
- **FR-004**: System MUST show a list of all teams that can be filtered by division.
- **FR-005**: System MUST allow a League Admin to remove a team only if it has no games already played.
- **FR-006**: System MUST only allow League Admins to create, edit, or remove teams.

---

## Key Entities

- **Team**: A group of players that plays in the league. Has a name, division, and one coach.
- **Division**: An age group the team plays in (for example U10, U12, U14).
- **Coach**: The adult in charge of the team.

---

## Initial Data Model

**Team**
| Field | Type | Notes |
|---|---|---|
| TeamId | int | Primary key |
| TeamName | string | Required, unique within division |
| DivisionId | int | Foreign key to Division |
| CoachId | int | Foreign key to Coach |
| IsActive | bool | False if team is removed |

**Division**
| Field | Type | Notes |
|---|---|---|
| DivisionId | int | Primary key |
| DivisionName | string | Example: U10 |
| MinAge | int | |
| MaxAge | int | |

**Coach**
| Field | Type | Notes |
|---|---|---|
| CoachId | int | Primary key |
| FirstName | string | Required |
| LastName | string | Required |
| Email | string | Required |
| Phone | string | |

---

## Gherkin AC

```gherkin
Feature: Team Management

  Scenario: Admin creates a new team
    Given I am logged in as a League Admin
    When I enter the team name "Tigers", choose division "U12", and choose a coach
    And I click "Save"
    Then the team "Tigers" is added to the U12 division

  Scenario: Admin tries to create a duplicate team
    Given a team named "Tigers" already exists in U12
    When I try to create another team named "Tigers" in U12
    Then I see the error "A team with this name already exists in this division"

  Scenario: Admin edits a team's coach
    Given the team "Tigers" exists
    When I change the coach and click "Save"
    Then the team shows the new coach

  Scenario: Parent views the team list
    Given I am logged in as a Parent
    When I go to the Teams page
    Then I see a list of all active teams
    And I do not see Add, Edit, or Remove buttons
```

---

## Screen Requirements

- **Team List Screen**: Shows all teams in a table (Name, Division, Coach). Has a division filter dropdown. Admins see an "Add Team" button and Edit/Remove buttons on each row.
- **Add/Edit Team Screen**: Form with Team Name (text box), Division (dropdown), and Coach (dropdown). Has "Save" and "Cancel" buttons. Shows error messages under the field that has a problem.
- **Remove Confirmation Popup**: Asks "Are you sure you want to remove this team?" with "Yes" and "No" buttons.
