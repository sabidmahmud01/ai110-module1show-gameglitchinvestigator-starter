# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?

The UI looked okay but immediately my eye went to the number of attempts which is mentioned in two different places but the number doesn't match. Upon clicking new game without inputting any guesses, the "Attempts left" becomes 8

- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

The New Game button doesn't work as you'd expect it to and you have to refresh the webpage to get to a new game

The hints were always backwards. The place where you'd expect the hint to be lower it shows higher.

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?

  I used ChatGPT (via this Copilot-style assistant) to help diagnose Streamlit state bugs and suggest code changes.

- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).

  The AI correctly suggested that the reason the "New Game" button felt broken was because Streamlit reruns the script and the session state still thought the game was over (status remained "won"/"lost"). It recommended resetting the full game state (status, score, attempts, history, and the secret) when New Game is pressed. I verified this by clicking New Game in the UI and observing that the game reset properly (attempts count went to 0, hints started over, and the game allowed new guesses without needing a browser refresh).

- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

  Early on, the AI suggested that the main bug might be due to the secret number being regenerated on every rerun (so the secret would change mid-game). While the secret was indeed being set on reruns, the real issue with the New Game button was that the code never reset `status` from "won"/"lost", so the UI immediately stopped. I verified this by printing and observing `st.session_state.status` and confirming that resetting it to "playing" fixed the New Game behavior without needing to change how the secret is generated.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?

  I decided the bug was fixed when the game behaved like a fresh session after clicking "New Game": the attempt counter reset, the secret number stayed stable, the hints started over, and I could play again without refreshing the browser.

- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.

  I manually clicked the New Game button several times and verified the in-UI debug info reset (attempts, score, history) and the game allowed new guesses. I also ran the existing pytest suite to confirm existing logic (hint direction, win/lose outcomes) still passed.

- Did AI help you design or understand any tests? How?

  AI helped by pointing out that the bug was in session state management. That gave me confidence to check `st.session_state.status` and manually verify that resetting it to "playing" fixed the issue, which is effectively a test of the state-reset logic.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- What change did you make that finally gave the game a stable secret number?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
