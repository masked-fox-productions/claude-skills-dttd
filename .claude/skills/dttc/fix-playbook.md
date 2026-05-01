# Fix Playbook

Project-specific rules of engagement for code changes. Customize this file
to match your project's conventions.

## Customize This File

Replace the sections below with your project's actual conventions.

## Commit Granularity

- [e.g., "One logical change per commit", "Squash into feature commits"]
- [Commit message conventions]

## Required Checks Before Submitting

- [ ] [Linter/formatter, e.g., `ruff check`, `prettier`, `rubocop`]
- [ ] [Type checker, e.g., `mypy`, `tsc --noEmit`]
- [ ] [Test suite passes]
- [ ] [Build succeeds]

## Change Boundaries

Rules about when a code change needs escalation or extra review:

- [e.g., "Changes to the public API require an ADR"]
- [e.g., "Database migrations need a separate review"]
- [e.g., "Changes to shared libraries must be verified in all consumers"]

## Safe Refactoring Boundaries

What's in scope for a fix session vs. what should be its own session:

- **In scope:** Bug fixes, making code match spec, small clarifying renames.
- **Out of scope:** Architectural changes, new abstractions, performance
  optimization (unless that's the stated goal), large-scale refactors.

## Project-Specific Gotchas

Known foot-guns or areas that need extra care:

- [e.g., "Modifying the JSON serialization affects all pipelines"]
- [e.g., "The auth module has implicit ordering dependencies"]
