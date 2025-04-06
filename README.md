# Win-Stay, Lose-Shift (Pavlov)

### Description

1. **Initial Setup**  
   - The function signature is `strategy(my_history: list[int], opponent_history: list[int], rounds: int | None) -> int`.  
   - `my_history` and `opponent_history` represent all past moves for the player and the opponent, respectively (`1` for cooperate, `0` for defect).  
   - The parameter `rounds` is not strictly used here, but it remains part of the signature.

2. **First Move**  
   - If no rounds have been played yet (`len(my_history) == 0`), the strategy **cooperates** (returns `1`).

3. **Identifying the Previous Payoff**  
   - Pavlov (Win-Stay, Lose-Shift) is based on whether you “did well” in the previous round. We figure out the payoff from the last moves of both players:
     - (D, D) ⇒ payoff = 1 (lose)
     - (D, C) ⇒ payoff = 5 (win)
     - (C, D) ⇒ payoff = 0 (lose)
     - (C, C) ⇒ payoff = 3 (win)

4. **Decision Logic**  
   - **Win-Stay**: If your last payoff was “good” (3 or 5), **repeat** your last move.  
   - **Lose-Shift**: If your last payoff was “bad” (0 or 1), **switch** your move (from cooperate to defect or vice versa).

5. **Move Return**  
   - Based on the computed payoff, return `1` (cooperate) or `0` (defect).

### Analysis

- **Immediate Recovery**  
  If you and your opponent ever both defect (payoff = 1), Pavlov sees this as a “lose” and switches to cooperation next round. This can quickly break a chain of mutual defection.

- **Exploiting Cooperation**  
  If mutual cooperation yields a “win” (3 points each), Pavlov will stay cooperating. This locks in a high payoff when facing a cooperative strategy.

- **Punishing Defection**  
  Pavlov also automatically exploits a perpetually cooperating opponent, but if the opponent defects, Pavlov eventually defects back—unless mutual defection triggers it to revert to cooperation.

- **Simplicity & Effectiveness**  
  With only a single round of memory (the last moves), Pavlov can effectively respond to the opponent’s behavior and adapt. It often performs strongly in tournaments similar to Generous Tit for Tat.

In many tournaments, **Pavlov** finds a balance between retaliation and cooperation, allowing it to do well against a wide variety of opposing strategies.

---