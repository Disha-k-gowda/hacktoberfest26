---
name: pytest-first
description: Help a Python learner design two or three focused pytest cases before implementing a small function. Use when the intended behavior is described but the implementation or tests have not yet been written, especially when normal, boundary, and unspecified cases need to be distinguished.
---

# Pytest first

Turn a clear behavior description into a tiny, explainable set of pytest tests **before** the implementation is written. Write only tests supported by the stated requirements.

## Inputs

Ask the learner for:
- The function name and its intended input/output behavior.
- Any stated normalization rules, failure behavior, and supported inputs.
- The target Python/pytest environment, if relevant.

Do not assume missing policy decisions, such as what empty input should mean.

## Procedure

1. **Extract the contract.** Restate explicit requirements separately from unknowns. If a key rule is unspecified, ask one targeted question before writing a test that relies on it.
2. **Pick two or three cases.** Include a representative normal case and a meaningful boundary case. Include an additional distinct case only if it protects another stated rule.
3. **Write minimal tests.** Use ordinary pytest functions with meaningful names and precise assertions. Refer to an illustrative module only after labeling it as a placeholder that the learner may need to change. No unnecessary fixtures or dependencies.
4. **Explain each assertion.** Briefly state which requirement it checks and why it matters. Avoid testing internal implementation details.
5. **Offer a way to check later.** Show the learner how to run `python -m pytest -q` once the implementation exists and pytest is installed; tests may initially fail or fail during import, which is expected in test-first development.
6. **Interpret real results carefully.** Never claim tests passed, or that a command was run, without actual output or an available tool that executed it. Revise tests when the learner clarifies the intended behavior.

## Example: username normalization

**Learner input**

"Design tests for `normalize_username(value: str) -> str`. It strips leading and trailing ordinary spaces and converts uppercase ASCII letters to lowercase. It preserves spaces inside the name. We haven't decided what to do with empty or all-space input."

**Expected first response**

"Three requirements are clear: trim outer spaces, lowercase ASCII uppercase letters, and preserve interior spaces. The behavior for empty or all-space input is not defined, so I won't write a test requiring a particular result for it. What should the function do in that case?"

**Once the empty-input behavior is clarified**, for example "return an empty string," propose:

```python
# Replace username.py with your actual module name.
from username import normalize_username


def test_normalizes_outer_spaces_and_case():
    assert normalize_username("  Alice  ") == "alice"


def test_preserves_interior_spaces():
    assert normalize_username("Mary Jane") == "mary jane"


def test_all_spaces_returns_empty_string():
    assert normalize_username("   ") == ""
```

**What each test protects**

- `test_normalizes_outer_spaces_and_case` checks stripping outer spaces and lowercasing ASCII letters.
- `test_preserves_interior_spaces` guards against accidentally deleting or collapsing interior spaces.
- `test_all_spaces_returns_empty_string` checks the boundary case **only after** the learner confirms this rule.

These tests are examples, not executed results. After implementing the function and installing pytest, run `python -m pytest -q` to check them.

## Edge case: unspecified empty input

If the input only says "normalize whitespace in usernames" or never defines behavior for `""` and `"   "`, do not assume rejection, an empty string, or a default name. Ask which result or exception is intended. You can still draft tests for any **other** fully specified behavior.

## Success checks

- The initial response to the example asks about the unspecified empty-input rule rather than silently choosing one.
- After clarification, the skill provides two or three readable pytest tests covering normal and boundary behavior.
- Every assertion is grounded in an explicit requirement or learner clarification.
- It explains what each test guards against without asserting unobserved test success.
