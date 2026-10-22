# Feature Specification: Referee Assignment

**Feature Branch**: `soccer-league`
**Created**: 2026-10-21
**Status**: Draft
**Input**: Every game needs a referee, and referees need to know which games they are working.

---

## User Stories

### User Story 1 - Assign a Referee (Priority: P1)
As a **League Admin**, I want to assign a referee to a game, so that every game has someone in charge.

### User Story 2 - View My Assigned Games (Priority: P1)
As a **Referee**, I want to see a list of the games I'm assigned to, so that I know where and when to show up.

### User Story 3 - Set My Availability (Priority: P3)
As a **Referee**, I want to mark the dates I can't work, so that I don't get assigned to games I can't make.

---

## Functional Requirements

- **FR-001**: System MUST allow a League Admin to add referees with a name, email, and phone number.
- **FR-002**: System MUST allow a League Admin to assign one referee to each game.
- **FR-003**: System MUST NOT allow a referee to be assigned to two games at the same date and time.
- **FR-004**: System MUST NOT allow a referee to be assigned on a date they marked as unavailable.
- **FR-005**: System MUST show a Referee only the games assigned to them on their My Games screen.
- **FR-006**: System MUST show games with no referee as "Needs Referee" to League Admins.
- **FR-007**: System MUST allow a Referee to add and remove dates they are unavailable.

---

## Key Entities

- **Referee**: A person who runs games and enters final scores.
- **Referee Assignment**: Links a referee to a game.
- **Unavailable Date**: A date a referee cannot work.

---

## Initial Data Model

**Referee**
| Field | Type | Notes |
|---|---|---|
| RefereeId | int | Primary key |
| FirstName | string | Required |
| LastName | string | Required |
| Email | string | Required |
| Phone | string | |

**RefereeAssignment**
| Field | Type | Notes |
|---|---|---|
| AssignmentId | int | Primary key |
| GameId | int | Foreign key to Game, one referee per game |
| RefereeId | int | Foreign key to Referee |

**RefereeUnavailableDate**
| Field | Type | Notes |
|---|---|---|
| UnavailableId | int | Primary key |
| RefereeId | int | Foreign key to Referee |
| UnavailableDate | date | |

---

## Gherkin AC

```gherkin
Feature: Referee Assignment

  Scenario: Admin assigns a referee to a game
    Given I am logged in as a League Admin
    And the Tigers vs Sharks game has no referee
    When I pick referee "John Smith" and click "Assign"
    Then John Smith is assigned to the game
    And the game no longer shows "Needs Referee"

  Scenario: Referee already has a game at that time
    Given John Smith is assigned to a game on Saturday at 9:00 AM
    When I try to assign him to another game on Saturday at 9:00 AM
    Then I see the error "This referee already has a game at this time"

  Scenario: Referee is unavailable
    Given John Smith marked Saturday as unavailable
    When I try to assign him to a game on Saturday
    Then I see the error "This referee is not available on this date"

  Scenario: Referee views their games
    Given I am logged in as referee John Smith
    When I go to My Games
    Then I only see the games I am assigned to
```

---

## Screen Requirements

- **Referee List Screen (Admin)**: Table of referees (Name, Email, Phone). Has an "Add Referee" button and an Edit button on each row.
- **Assign Referees Screen (Admin)**: List of upcoming games. Each game shows the current referee or "Needs Referee", with a referee dropdown and an "Assign" button. The dropdown only shows referees who are free at that time.
- **My Games Screen (Referee)**: List of the referee's upcoming games (Date, Time, Teams, Field).
- **My Availability Screen (Referee)**: Calendar where the referee can click dates to mark them unavailable. Has a "Save" button.
