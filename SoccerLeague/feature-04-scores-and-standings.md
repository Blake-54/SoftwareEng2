# Feature Specification: Scores and Standings

**Feature Branch**: `soccer-league`
**Created**: 2026-10-21
**Status**: Draft
**Input**: After games are played, the scores need to be entered and the league standings need to update.

---

## User Stories

### User Story 1 - Enter a Game Score (Priority: P1)
As a **Referee or League Admin**, I want to enter the final score of a game, so that the result is saved.

### User Story 2 - View Standings (Priority: P1)
As a **Parent, Coach, or Player**, I want to see the standings for my division, so that I know how my team is doing.

### User Story 3 - Fix a Wrong Score (Priority: P2)
As a **League Admin**, I want to fix a score that was entered wrong, so that the standings are correct.

---

## Functional Requirements

- **FR-001**: System MUST allow the assigned Referee or a League Admin to enter a final score for a game.
- **FR-002**: System MUST only allow scores to be entered for games that are not cancelled and whose date has passed.
- **FR-003**: System MUST NOT allow negative scores.
- **FR-004**: System MUST mark the game as "Completed" once a score is entered.
- **FR-005**: System MUST update the standings automatically when a score is entered or changed.
- **FR-006**: System MUST give 3 points for a win, 1 point for a tie, and 0 points for a loss.
- **FR-007**: System MUST sort the standings by points, then goal difference, then goals scored.
- **FR-008**: System MUST only allow a League Admin to change a score after it is saved.

---

## Key Entities

- **Game Result**: The final score of a completed game.
- **Standings**: A ranking of the teams in a division based on their results.

---

## Initial Data Model

**GameResult**
| Field | Type | Notes |
|---|---|---|
| ResultId | int | Primary key |
| GameId | int | Foreign key to Game |
| HomeScore | int | 0 or higher |
| AwayScore | int | 0 or higher |
| EnteredBy | int | Foreign key to User |
| EnteredDate | datetime | |

**Standings** (calculated from GameResult, not stored)
| Field | Type | Notes |
|---|---|---|
| TeamId | int | |
| GamesPlayed | int | |
| Wins | int | |
| Losses | int | |
| Ties | int | |
| GoalsFor | int | |
| GoalsAgainst | int | |
| GoalDifference | int | GoalsFor minus GoalsAgainst |
| Points | int | (Wins x 3) + Ties |

---

## Gherkin AC

```gherkin
Feature: Scores and Standings

  Scenario: Referee enters a score
    Given I am the referee assigned to the Tigers vs Sharks game
    And the game has been played
    When I enter Tigers 2, Sharks 1 and click "Submit"
    Then the game is marked "Completed"
    And the Tigers get 3 points in the standings

  Scenario: Game ends in a tie
    When the final score Tigers 1, Sharks 1 is entered
    Then both teams get 1 point in the standings

  Scenario: Negative score entered
    When I enter a score of -1
    Then I see the error "Score cannot be negative"

  Scenario: Score entered for a future game
    Given a game is scheduled for next week
    When I try to enter a score for that game
    Then I see the error "You cannot enter a score for a game that has not been played"

  Scenario: Admin fixes a score
    Given the game Tigers vs Sharks has a saved score of 2-1
    When a League Admin changes the score to 2-2
    Then the standings update to show a tie for both teams
```

---

## Screen Requirements

- **Enter Score Screen**: Shows the game info (teams, date, field) and two number boxes for Home Score and Away Score. Has "Submit" and "Cancel" buttons.
- **My Games Screen (Referee)**: List of the referee's past games that still need a score, each with an "Enter Score" button.
- **Standings Screen**: Division dropdown at the top. Table showing Rank, Team, GP, W, L, T, GF, GA, GD, and Pts, sorted by rank.
- **Game Results Screen**: List of completed games with final scores. Admins see an "Edit Score" button on each.
