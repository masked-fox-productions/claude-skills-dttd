---
name: dttt
description: "Don't Trust the Tests — build or repair a trustworthy test suite by treating existing tests as incomplete or incorrect. Captures bugs as failing tests without fixing the code."
when_to_use: "When the user wants to add test coverage, audit existing tests, prepare for a refactor, reproduce bugs, or validate system behavior. Also when test suite is sparse, outdated, or gives false confidence."
argument-hint: "[scope: module path, feature name, or 'full repo']"
effort: high
---

# /dttt — Don't Trust the Tests

You are entering a structured testing session. Your job is to build a test suite
that honestly captures what the system does — and exposes where it doesn't do
what it should.

**Primary artifact (untrusted):** Tests
**Secondary evidence:** Docs + Code (trust levels set during interview)

## Session Modes

At the start of the session, ask the user which mode they want:

### Standard Mode
Focused test-writing work on a specific scope. Proceeds to the trust interview.

### Audit Mode
Full-repo test infrastructure assessment. Use when:
- Onboarding to a codebase with unknown test quality
- Test suite is sparse or nonexistent
- Preparing for a major refactor and need to understand coverage baseline

In audit mode:
1. Set docs and code trust to `unknown` (will be assessed during the audit).
2. Set scope to full repo.
3. Check if a test framework is configured. If not:
   - Detect the project's language(s) and recommend an appropriate framework.
   - Offer to set up the framework (conftest.py, test runner config, CI integration).
   - Ask the user before installing anything.
4. Run the existing test suite. Note pass/fail/error counts.
5. Measure coverage if tooling is available or can be quickly set up.
6. Survey the codebase: list every module, identify which have tests and which don't.
7. Produce a **coverage inventory**: tested modules, untested modules, coverage percentage
   per module (if measurable), and test quality assessment (meaningful assertions vs. smoke tests).
8. Recommend **immediate coverage improvements** prioritized by:
   - Critical path modules with zero coverage
   - Modules with complex logic but no tests
   - Modules that other modules depend on heavily
9. Offer to start writing tests for the highest-priority gaps, or hand off as
   standard-mode follow-ups with specific scope.

Audit mode produces a **test inventory and coverage plan** rather than a full test suite.
It's the reconnaissance pass that informs targeted `/dttt` sessions.

## Session Protocol (Standard Mode)

### 1. Trust Interview
Follow the trust interview protocol exactly.
See [trust-interview.md](../_dtt-shared/trust-interview.md).

### 2. Survey Existing Tests
Before writing tests, understand the current state:

- Find all test files in the scoped area.
- Run the existing test suite (or the scoped subset). Note what passes, fails, errors.
- Check coverage if tooling is available.
- Read test code critically: are assertions meaningful or do they just check
  that code runs without crashing?
- Identify: tested paths, untested paths, tests that pass for the wrong reasons.

### 3. Identify Coverage Gaps
Using the trust-weighted secondary evidence, build a list of what should be
tested but isn't:

- From **code** (trust-weighted): trace public APIs, branch points, error paths,
  edge cases. Each is a potential test.
- From **docs** (trust-weighted): documented behaviors that have no corresponding
  test. Each is a potential test.
- From **existing tests**: look for patterns — if similar functions are tested but
  one is missing, that's a gap.

Prioritize: critical paths > documented behavior > edge cases > error paths.

See [coverage-playbook.md](coverage-playbook.md) for project-specific guidance.

### 4. Write Tests
Follow these principles:

- **Tests document behavior.** A test name should read as a specification.
  `test_blend_two_poses_weights_sum_to_one` not `test_blend`.
- **One behavior per test.** Don't combine unrelated assertions.
- **Tests are evidence.** Tag test comments with finding classifications where
  the expected behavior came from a specific trust source.
- **Match project idioms.** See [test-patterns.md](test-patterns.md) for this
  project's conventions (fixtures, mocking, test structure).

### 5. Handling Discovered Bugs

**Do NOT fix code in this mode.** When you find code that doesn't match
trusted docs or expected behavior:

1. Write a test that captures the **expected** behavior (it will fail).
2. Mark it appropriately for the test framework (e.g., `xfail`, `skip`,
   `@pytest.mark.xfail(reason="...")`, or a clear comment).
3. Note the bug in the session log with:
   - What the code does (actual behavior).
   - What it should do (expected behavior, with trust source).
   - The failing test that captures the gap.
4. Continue building the test suite. Don't get pulled into fixing.

The failing tests become the definition of done for a future `/dttc` session.

### 6. Session Summary
Produce the structured summary.
See [session-summary.md](../_dtt-shared/session-summary.md).

In addition to the standard summary, include:

- **Coverage before/after** (if measurable).
- **New tests written** (list with brief descriptions).
- **Discovered bugs** (list with failing test references).
- **Offer to open GitHub issues** for each discovered bug using `gh issue create`.

## Mode Discipline

- Do not fix implementation code. Write tests that expose the problem.
- Do not update documentation. Note doc inaccuracies for a `/dttd` follow-up.
- If existing tests are wrong (assert incorrect behavior), fix the test — that's
  your primary artifact. But flag it: the "passing" test was giving false confidence.

## Suggested Follow-ups

At the end of the session, suggest:
- `/dttc [scope]` — with the failing tests as the definition of done.
- `/dttd [scope]` — if doc inaccuracies were found.
- Offer to create GitHub issues for discovered bugs.

$ARGUMENTS
