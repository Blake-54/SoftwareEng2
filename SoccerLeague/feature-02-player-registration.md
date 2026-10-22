# Feature Specification: Player Registration

**Feature Branch**: `soccer-league`
**Created**: 2026-10-21
**Status**: Draft
**Input**: Parents need to sign their kids up for the league, and admins need to put players on team rosters.

---

## User Stories

### User Story 1 - Register a Player (Priority: P1)
As a **Parent**, I want to register my child for the season, so that they can play in the league.

### User Story 2 - Assign Player to a Team (Priority: P1)
As a **League Admin**, I want to assign registered players to a team in their age division, so that every team has a full roster.

### User Story 3 - View Team Roster (Priority: P2)
As a **Coach**, I want to see the list of players on my team, so that I know who is on my roster and how to contact their parents.

### User Story 4 - Update Player Info (Priority: P3)
As a **Parent**, I want to update my child's info, so that the league has the correct contact and medical details.

---

## Functional Requirements

- **FR-001**: System MUST allow a Parent to register a player with first name, last name, birth date, and emergency contact.
- **FR-002**: System MUST automatically place the player in the correct division based on their birth date.
- **FR-003**: System MUST allow a League Admin to assign a player to one team in their division.
- **FR-004**: System MUST NOT allow a player to be on more than one team in the same season.
- **FR-005**: System MUST allow a Coach to view the roster for their own team only.
- **FR-006**: System MUST allow a Parent to edit their own child's information.
- **FR-007**: System MUST set a maximum roster size per division (for example, 15 players).

---

## Key Entities

- **Player**: A kid who plays in the league. Belongs to one team per season.
- **Parent**: The adult who registers the player and is the main contact.
- **Roster**: The list of players on a team for a season.

---

## Initial Data Model

**Player**
| Field | Type | Notes |
|---|---|---|
| PlayerId | int | Primary key |
| FirstName | string | Required |
| LastName | string | Required |
| BirthDate | date | Required, used to pick division |
| ParentId | int | Foreign key to Parent |
| TeamId | int | Foreign key to Team, can be empty until assigned |
| MedicalNotes | string | Optional |

**Parent**
| Field | Type | Notes |
|---|---|---|
| ParentId | int | Primary key |
| FirstName | string | Required |
| LastName | string | Required |
| Email | string | Required |
| Phone | string | Required |

---

## Gherkin AC

```gherkin
Feature: Player Registration

  Scenario: Parent registers a player
    Given I am logged in as a Parent
    When I fill out my child's name, birth date, and emergency contact
    And I click "Register"
    Then my child is registered for the season
    And my child is placed in the correct division

  Scenario: Parent leaves a required field blank
    Given I am on the Player Registration form
    When I leave "Birth Date" blank and click "Register"
    Then I see the error "Birth Date is required"

  Scenario: Admin assigns a player to a team
    Given a registered player in division U12 with no team
    When I assign the player to the team "Tigers"
    Then the player shows up on the Tigers roster

  Scenario: Team roster is full
    Given the team "Tigers" already has 15 players
    When I try to add another player to the Tigers
    Then I see the error "This team's roster is full"

  Scenario: Coach views roster
    Given I am logged in as the coach of the Tigers
    When I go to My Roster
    Then I see all players on the Tigers and their parent contact info
```

---

## Screen Requirements

- **Player Registration Screen**: Form with First Name, Last Name, Birth Date (date picker), Emergency Contact, and Medical Notes. Has "Register" and "Cancel" buttons.
- **Unassigned Players Screen (Admin)**: Table of registered players with no team, filtered by division. Each row has a "Team" dropdown and an "Assign" button.
- **Team Roster Screen**: Shows the team name and a table of players (Name, Age, Parent Name, Parent Phone). Shows roster count like "12 / 15".
- **My Players Screen (Parent)**: Shows the parent's registered children with an "Edit" button for each.
