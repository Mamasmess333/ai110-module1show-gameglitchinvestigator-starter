# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

When I first ran the game, it was completely unplayable — even when I typed the exact secret number shown in the Developer Debug Info, I still couldn't win. Two bugs stood out immediately: the hints were completely backwards ("Go HIGHER!" appeared when my guess was too high, and "Go LOWER!" when it was too low), and the secret number seemed to change every time I clicked Submit. I also noticed the game would sometimes compare my guess to a string version of the secret on even-numbered attempts, which made winning on those turns impossible. The New Game button also didn't fully reset the game — old history and score carried over into the next round.

---

## 2. How did you use AI as a teammate?

I used Claude (Claude Code) and ChatGPT throughout this project to help me understand bugs and look up Streamlit-specific behavior. One example where AI was correct: I asked ChatGPT "How do I keep a variable from resetting in Streamlit when I click a button?" and it correctly explained that I needed to use `st.session_state` with an `if "key" not in st.session_state` guard — I verified this by adding the guard for `secret` and confirming the number no longer changed on each submit. One example where AI was misleading: when I asked about fixing the score formula, an AI suggestion included resetting the score to zero on a wrong guess, which would have been way too harsh — I caught this by tracing through the logic manually and realized a simple `-5` deduction was the correct behavior.

---

## 3. Debugging and testing your fixes

I decided a bug was really fixed when both manual testing (playing the game and seeing expected behavior) and the automated `pytest` tests passed together. For example, after fixing the `check_guess` function, I ran `pytest` and confirmed the tests for "Too High" and "Too Low" outcomes both passed — before the fix, those tests were failing because the return values were swapped. AI helped me understand what the tests were asserting, especially how `pytest` compares return tuples, which made it easier to know exactly what the function needed to return.

---

## 4. What did you learn about Streamlit and state?

The secret number kept changing because Streamlit reruns the entire Python script from top to bottom every single time a user interacts with the page — including clicking a button. Without `st.session_state`, `random.randint()` was called fresh on every rerun, producing a new secret each time. I would explain it to a friend like this: imagine your browser refreshes the whole page every time you click anything — `session_state` is like a sticky notepad that survives those refreshes. The fix was wrapping the secret generation in `if "secret" not in st.session_state:` so it only runs once, on the very first load.

---

## 5. Looking ahead: your developer habits

One habit I want to carry forward is reading the tests before touching the code — the `pytest` tests acted like a spec that told me exactly what each function was supposed to do, which made debugging much faster. Next time I work with AI on a coding task, I would ask it to explain its reasoning step by step rather than just accepting the code it gives me, so I can catch mistakes before running anything. This project changed the way I think about AI-generated code: I used to assume that if it ran without errors it was probably correct, but now I know that code can be syntactically valid and logically broken at the same time — you still have to verify the behavior yourself.
