---
name: practice-exam
description: Use when the user wants to take the CCAF Practice Exam — present the original questions from `Practice Exam/*.md` one at a time. Triggers on phrases like "模試/練習/模擬試験/Practice Exam を受けたい", "練習問題を出して", "Practice Exam をやろう", "出題して" referring to the Practice Exam. Randomizes scenario order, question order within a scenario, and choice order. Reveals the correct answer + explanation only after the user answers each question, then proceeds to the next.
---

# Practice Exam Mode

Drive the user through the four CCAF Practice Exam scenarios in `Practice Exam/`, presenting questions verbatim from the source files.

## Scenarios (source files)

- `Practice Exam/Claude Code for Continuous Integration.md`
- `Practice Exam/Code Generation with Claude Code.md`
- `Practice Exam/Customer Support Resolution Agent.md`
- `Practice Exam/Multi-Agent Research System.md`

Each file contains 15 questions in this exact format:

```
## Question N

<question body — may be multi-paragraph>

A) <choice A>
B) <choice B>
C) <choice C>
D) <choice D>

Correct Answer: <letter>
<explanation paragraph>
```

## Workflow

### 1. Initialize the session

At the start (or when the user wants to begin), read all four scenario files and **shuffle the scenario order using true randomness**. Use Bash, e.g.:

```bash
ls "Practice Exam/"*.md | shuf
```

Track session state in your working memory:
- `scenario_queue`: shuffled list of scenarios not yet attempted this session
- `current_scenario`: the active scenario
- `question_queue`: shuffled question indexes for the current scenario
- `score`: `{correct: N, total: N}` per scenario and overall

If the user says "skip to scenario X" or "do <scenario name> next", honor that and reorder the queue.

### 2. Start a scenario

Announce the scenario name and the scenario intro paragraph (the text immediately under the `# <Title>` heading, before `## Question 1`) verbatim. Then say how many questions there are (15) and present question 1.

### 3. Present a question

For each question:

1. Parse the source: question body, the four choices, the correct answer letter, and the explanation.
2. **Randomize the choice order** using true randomness (e.g. `shuf` in Bash, or another randomization mechanism — do NOT just rely on your own judgement). Re-label them A/B/C/D in the new order. Track the mapping so you know which new letter corresponds to the original correct answer.
3. Output the question **verbatim** from the source (do not paraphrase, translate, or shorten the question body), followed by the four shuffled choices labeled A) B) C) D).
4. Do **not** reveal the correct answer or any hint at this stage.
5. Wait for the user's answer.

Example output shape:

```
### Scenario: Claude Code for Continuous Integration — Question 3 of 15

<verbatim question body>

A) <shuffled choice>
B) <shuffled choice>
C) <shuffled choice>
D) <shuffled choice>
```

### 4. Handle the user's answer

The user will reply with a letter (A/B/C/D), or sometimes the choice text, or "skip" / "I don't know".

1. Map their reply back to the original choice (using your shuffle mapping).
2. Compare against the correct answer.
3. Reply with:
   - ✅ Correct / ❌ Incorrect (use these symbols even though emojis are normally avoided — they make scoring scannable)
   - "Correct answer: <letter in the shuffled labeling>) <choice text>"
   - The explanation **verbatim** from the source.
4. Update score.
5. Immediately move to the next question without waiting for further confirmation, unless the user asks to pause or discuss.

If the user wants to discuss before moving on (e.g. "why is C wrong?"), answer based on the explanation and the docs in `docs/`, then continue.

### 5. End of scenario

After question 15 of a scenario:

1. Show a scenario summary: `<scenario name>: X/15 correct (Y%)`.
2. Briefly call out any domains/topics where multiple questions were missed.
3. Ask the user: "次のシナリオに進みますか? (残り N シナリオ)" — if yes, dequeue the next scenario from `scenario_queue` and go to step 2 of the workflow. If no, go to step 6.

### 6. End of session

When the user stops or all 4 scenarios are done, show:

- Total score across attempted scenarios
- Per-scenario breakdown
- Topics/domains with the weakest performance, mapped to the 5 exam domains (Agentic Architecture & Orchestration / Tool Design & MCP / Claude Code Configuration / Prompt Engineering / Context Management & Reliability)
- Pointers into `docs/` and `Foundations Certification Exam Guide.md` for review

## Rules

- **Verbatim question text.** Never rewrite, translate, or summarize the question body or the choice text. The user is practicing for the real exam — they need to see exactly what the source says.
- **Verbatim explanation.** After answering, show the explanation exactly as written in the source. You may add supplementary notes after, clearly separated.
- **Randomize using a real RNG.** Use `shuf`, `python -c "import random,sys; ..."`, or similar via Bash. Don't rely on your own intuition for randomness — it's biased.
- **One question at a time.** Never list multiple questions in a single message.
- **No hints before the answer.** Do not signal the correct answer through formatting, ordering choice you put first, or wording.
- **Track scoring across the whole session,** not just the current scenario.
- The user may answer in Japanese or English. Treat "答えはB" / "B です" / "B" / "B)" all as choice B. If ambiguous, ask.
- If the user says "答え教えて" / "give up" / "skip" without answering, mark it as not-answered (don't count as wrong — track separately) and reveal the answer + explanation, then proceed.
