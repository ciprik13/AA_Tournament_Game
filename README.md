# Win-Stay, Lose-Shift with Adaptive Opponent Selection

### Description

This strategy is an enhanced version of **Win-Stay, Lose-Shift (Pavlov)** adapted for the second round of the tournament, where opponent selection is allowed.

1. **Move Decision (Pavlov Logic)**  
   - If no rounds have been played yet (`len(my_history) == 0`), the strategy **cooperates** (`1`).
   - Otherwise, it analyzes the last move:
     - (Defect, Defect) ⇒ payoff = 1 (lose)
     - (Defect, Cooperate) ⇒ payoff = 5 (win)
     - (Cooperate, Defect) ⇒ payoff = 0 (lose)
     - (Cooperate, Cooperate) ⇒ payoff = 3 (win)
   - **Win-Stay**: If the payoff was 3 or 5, **repeat** the previous move.
   - **Lose-Shift**: If the payoff was 0 or 1, **switch** the move.

2. **Opponent Selection**  
   - After each move, the strategy selects the next opponent based on their behavior.
   - It chooses the opponent with the **highest cooperation rate** (percentage of cooperative moves against us).
   - If multiple opponents have the same cooperation rate, it selects the one with the smallest ID.
   - If the current opponent has recently (in the last 3 moves) **defected 2 or more times**, the strategy **avoids** continuing with them even if their overall cooperation rate is high.
   - Opponents with 200 rounds already played are excluded from selection.

3. **Function Signature**  
   ```python
   def strategy_round_2(opponent_id: int, my_history: dict[int, list[int]], opponents_history: dict[int, list[int]]) -> tuple[int, int]:
   move: 0 (defect) or 1 (cooperate).
   next_opponent: ID of the selected opponent for the next round.
    ```
### Analysis

#### Immediate Recovery
If mutual defection occurs (both defect), the strategy quickly switches to cooperation, breaking chains of low-payoff rounds.

#### Exploiting Cooperation
When facing cooperative opponents, it maintains cooperation to consistently achieve high scores (3 points per round).

#### Retaliating Against Betrayal
If an opponent defects, the strategy automatically retaliates by switching its move.  
If mutual defection continues, it attempts recovery by cooperating.

#### Dynamic Opponent Management
In addition to adapting moves, the strategy monitors opponents dynamically.  
If an opponent begins defecting too often, it proactively abandons them in favor of more reliable opponents.

#### Maximizing Long-Term Scores
By selectively playing against the most cooperative players and reacting to recent betrayal, the strategy aims to maximize the average score over all rounds.

#### Simplicity & Efficiency
- Uses only the last few moves to make decisions.
- Executes quickly within the time and memory constraints.
- No external libraries or random factors involved in the algorithm itself.

---

### Expected Behavior in Tournament

In tournaments where opponents are generally cooperative, this strategy is expected to maintain a score close to 3 points per round.  
Against aggressive or deceptive opponents, it dynamically adapts by changing partners to avoid prolonged losses.

This balance between adaptability, cooperation, and opponent selection makes it a highly effective choice for the second round format.
