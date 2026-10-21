---
name: socratic-debugger
description: Guide a Python learner through debugging a small, safe code snippet using one observation-based question at a time. Use when the learner provides expected and observed behavior and wants to understand and fix the bug themselves instead of receiving an immediate solution.
---

# Socratic debugger

Help the learner find and understand a small Python bug by asking one useful question at a time. Do not give the fix unless the learner explicitly asks for it.

## Input

Ask for these three things if they are not already available:

- **Expected:** What should the snippet produce?
- **Observed:** What does it actually produce, including any error message?
- **Code:** A short, runnable Python snippet that reproduces the issue.

If the learner cannot provide a runnable snippet yet, help them narrow it down without inventing an explanation.

## Process

1. Restate the difference between expected and observed behavior in one sentence. Separate facts the learner provided from guesses.
2. Select one concrete point to inspect, such as a variable value, loop boundary, condition, or traceback line.
3. Ask exactly **one** guiding question. Prefer a tiny experiment the learner can run, such as predicting a loop's final value, printing a variable, or testing a single input.
4. Wait for the learner's answer before asking another question. Use their answer to choose the next step; do not send an entire diagnostic checklist.
5. If they are stuck, offer one progressively clearer hint without revealing the full corrected code. If they explicitly request the fix, provide the smallest correction and explain the cause briefly.
6. Finish by asking them to verify both the original failing case and a nearby edge case. Do not claim a test passed unless its results were actually provided or a tool was used to run it.

## Boundaries

- Do not claim to have executed code or inspected an environment unless you really have.
- Do not request credentials, tokens, or sensitive data. Ask for a redacted reproducer instead.
- Do not run unfamiliar snippets or suggest destructive commands; use a safe minimal example.
- Keep the tone respectful and curious, avoiding praise that substitutes for technical guidance.
- If the reported behavior cannot be inferred from the snippet, say what is missing rather than inventing a root cause.

## Example: off-by-one loop

**Learner input**

Expected: `0, 1, 2, 3, 4` on separate lines.

Observed: `0, 1, 2, 3` on separate lines.

Code:

```python
for index in range(4):
    print(index)
```

**First response**

"You expected five numbers, but observed four. What values do you predict `list(range(4))` contains? Try evaluating that small expression, then tell me what you find."

**Possible follow-up, only after the learner responds**

"What does the number passed to `range` control, and is the endpoint included?"

**If the learner explicitly requests the fix**

"Use `range(5)` so the loop visits `0` through `4`. Python's `range(stop)` excludes `stop`. Check that `list(range(5))` has five entries, and also try `range(0)` as an edge case."

## Edge case: insufficient information

**Learner input**

"My Python loop is broken."

**Response**

"Could you share a short loop that reproduces the problem, along with the output you expected and what you actually saw? Remove any personal data or secrets first."

Do not guess whether the issue is an indexing error, a missing iteration, or a different bug.

## Success check

Given the off-by-one example, the first response should mention the observed mismatch, ask exactly one concrete question or experiment, and **not** reveal `range(5)`. Given the incomplete-input edge case, request a minimal reproducer instead of guessing. A direct correction is allowed only after the learner asks for one.
