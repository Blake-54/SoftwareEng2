# Feature Specification: Game Scheduling

**Feature Branch**: `soccer-league`
**Created**: 2026-10-21
**Status**: Draft
**Input**: The league needs a schedule of games showing which teams play, when, and where.

---

## User Stories

### User Story 1 - Schedule a Game (Priority: P1)
As a **League Admin**, I want to schedule a game between two teams with a date, time, and field, so that everyone knows when and where to play.

### User Story 2 - View the Schedule (Priority: P1)
As a **Parent, Coach, or Player**, I want to see the game schedule for my team, so that I know when and where the games are.

### User Story 3 - Reschedule or Cancel a Game (Priority: P2)
As a **League Admin**, I want to change or cancel a game, so that I can handle bad weather or field problems.

---

## Functional Requirements

- **FR-001**: System MUST allow a League Admin to create a game with a home team, away team, date, time, and field.
- **FR-002**: System MUST only allow games between two teams in the same division.
- **FR-003**: System MUST NOT allow a team to play against itself.
- **FR-004**: System MUST NOT allow two games on the same field at the same date and time.
- **FR-005**: System MUST NOT allow a team to have two games at the same date and time.
- **FR-006**: System MUST allow anyone logged in to view the schedule and filter it by team or division.
- **FR-007**: System MUST allow a League Admin to reschedule or cancel a game.
- **FR-008**: System MUST show cancelled games as "Cancelled" on the schedule.

---

## Key Entities

- **Game**: A match between a home team and an away team at a set date, time, and field.
- **Field**: A place where games are played.
- **Season**: The time period the games are played in (for example, Fall 2026).

---

## Initial Data Model

**Game**
| Field | Type | Notes |
|---|---|---|
| GameId | int | Primary key |
| SeasonId | int | Foreign key to Season |
| HomeTeamId | int | Foreign key to Team |
| AwayTeamId | int | Foreign key to Team |
| FieldId | int | Foreign key to Field |
| GameDateTime | datetime | Required |
| Status | string | Scheduled, Completed, or Cancelled |

**Field**
| Field | Type | Notes |
|---|---|---|
| FieldId | int | Primary key |
| FieldName | string | Example: Field 1 |
| Address | string | |

**Season**
| Field | Type | Notes |
|---|---|---|
| SeasonId | int | Primary key |
| SeasonName | string | Example: Fall 2026 |
| StartDate | date | |
| EndDate | date | |

---

## Gherkin AC

```gherkin
Feature: Game Scheduling

  Scenario: Admin schedules a game
    Given I am logged in as a League Admin
    When I pick home team "Tigers", away team "Sharks", a date, a time, and "Field 1"
    And I click "Save"
    Then the game shows up on the schedule

  Scenario: Field is already booked
    Given a game is scheduled on "Field 1" on Saturday at 9:00 AM
    When I try to schedule another game on "Field 1" on Saturday at 9:00 AM
    Then I see the error "This field is already booked at that time"

  Scenario: Team picked twice
    When I pick "Tigers" as both the home and away team
    Then I see the error "A team cannot play itself"

  Scenario: Admin cancels a game
    Given a game is scheduled between the Tigers and the Sharks
    When I click "Cancel Game" and confirm
    Then the game shows as "Cancelled" on the schedule

  Scenario: Parent views their team's schedule
    Given I am logged in as a Parent of a Tigers player
    When I filter the schedule by "Tigers"
    Then I only see games where the Tigers are playing
```

---

## Screen Requirements

- **Schedule Screen**: Table or calendar list of games (Date, Time, Home Team, Away Team, Field, Status). Has filters for Division and Team. Admins see an "Add Game" button and Edit/Cancel buttons on each game.
- **Add/Edit Game Screen**: Form with Home Team (dropdown), Away Team (dropdown), Date (date picker), Time (time picker), and Field (dropdown). Has "Save" and "Cancel" buttons.
- **Cancel Game Popup**: Asks "Are you sure you want to cancel this game?" with "Yes" and "No" buttons.
