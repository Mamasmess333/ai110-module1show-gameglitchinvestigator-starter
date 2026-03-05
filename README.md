# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

**Game Purpose:** A number guessing game built with Streamlit where players guess a secret number within a difficulty-based range and attempt limit, earning score points for winning quickly.

**Bugs Found:**
1. Hint direction was backwards — "Go HIGHER!" showed when the guess was too high, and "Go LOWER!" when too low.
2. Hard difficulty (range 1–50) was actually easier than Normal (1–100) — the ranges were illogical.
3. On every even-numbered attempt, the secret was converted to a string, making it impossible to win on those turns.
4. The win score formula used `attempt_number + 1`, penalizing the player an extra 10 points they shouldn't lose.
5. Wrong guesses on even attempts awarded +5 points instead of deducting them.
6. The attempt counter started at 1 before any guess was made, causing an off-by-one error throughout.
7. The UI info banner always said "1 to 100" regardless of the selected difficulty.
8. Clicking "New Game" did not reset `status`, `history`, or `score`, so the game could not actually restart.

**Fixes Applied:**
- Fixed hint logic in `check_guess`: `guess > secret` now correctly returns "Too High" / "Go LOWER!"
- Updated Hard difficulty range to 1–500 (larger than Normal).
- Removed the even/odd attempt secret-string-casting code — secret is always compared as an int.
- Fixed score formula to `100 - 10 * attempt_number` (removed the `+ 1`).
- Removed points reward for wrong guesses — both "Too High" and "Too Low" now always deduct 5.
- Changed initial `attempts` to start at `0` so the first submit correctly counts as attempt 1.
- Updated the info banner to use `{low}` and `{high}` variables from the difficulty setting.
- New Game now resets `status`, `history`, `score`, and uses the correct difficulty range for the new secret.
- Refactored all game logic (`get_range_for_difficulty`, `parse_guess`, `check_guess`, `update_score`) into `logic_utils.py`.

## 📸 Demo

- [ ] [Insert a screenshot of your fixed, winning game here]

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
