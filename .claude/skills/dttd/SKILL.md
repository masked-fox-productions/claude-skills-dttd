---
name: dttd
description: "Don't Trust the Documents — audit, repair, or create documentation by treating existing docs as incomplete or incorrect. Triangulates from tests and code to produce honest, classified documentation."
when_to_use: "When the user wants to audit docs, document a feature, reconcile docs with reality, onboard to a codebase, or design future behavior. Also when docs seem stale, incomplete, or contradictory."
argument-hint: "[scope: module path, feature name, or 'full repo']"
effort: high
---

# /dttd — Don't Trust the Documents

You are entering a structured documentation session. Your job is to produce or
repair documentation that honestly reflects reality — not to rubber-stamp
existing docs.

**Primary artifact (untrusted):** Documentation
**Secondary evidence:** Tests + Code (trust levels set during interview)

## Session Modes

At the start of the session, ask the user which mode they want:

### Standard Mode
Focused documentation work on a specific scope. Proceeds to the trust interview.

### Audit Mode
Full-repo documentation assessment and reorganization. Use when:
- Onboarding to a codebase with unknown doc quality
- Docs are scattered, disorganized, or of unknown accuracy
- The user wants a comprehensive doc inventory before targeted work

In audit mode:
1. Set tests and code trust to `unknown` (will be assessed during the audit).
2. Set scope to full repo.
3. Survey all existing documentation: README, docs/, inline comments, docstrings.
4. Survey the codebase structure to understand what *should* be documented.
5. Produce an inventory: what exists, what's missing, what's contradicted, what's stale.
6. If the repo has an existing doc structure that's coherent, ask before reorganizing.
   If docs are scattered with no clear pattern, propose a structure (README + docs/ tree).
7. Offer to execute the reorganization, or hand off specific sections as standard-mode follow-ups.

Audit mode produces a **doc inventory and restructuring plan** rather than updated docs directly.
It's the reconnaissance pass that informs targeted `/dttd` sessions.

## Session Protocol (Standard Mode)

### 1. Trust Interview
Follow the trust interview protocol exactly.
See [trust-interview.md](../_dtt-shared/trust-interview.md).

### 2. Gather Evidence
Before writing anything, investigate the scope:

- Read existing documentation for the scoped area.
- Read the implementation code. Trace key paths.
- Read existing tests — they are executable documentation of actual behavior.
- Check git log for recent changes that may not be reflected in docs.

Use the trust levels from the interview to weight what you find.
See [trust-matrix.md](../_dtt-shared/trust-matrix.md) for how to act on each level.

### 3. Audit Existing Documentation
Work through the [doc-audit-checklist.md](doc-audit-checklist.md). For each item:

- Is it present?
- Is it accurate (per the trust-weighted evidence)?
- Tag it: VERIFIED / INFERRED / CONTRADICTED / UNKNOWN.

### 4. Produce or Update Documentation
Write documentation that follows these rules:

- **Current behavior** sections describe what the system *actually does*, verified
  by reading code or running tests. Not what someone wished it did.
- **Intended behavior** sections describe what the system *should* do, sourced
  from high-trust references or explicit user statements. Clearly separated from
  current behavior.
- **Every claim is tagged** with its finding classification.
- **Contradictions are stated explicitly**, not resolved silently.
  Format: "Docs say X. Code does Y. Tests assert Z."
- **Unknowns are admitted**, not papered over.

Pick the appropriate template from [doc-templates/](doc-templates/) if one fits.

### 5. Session Summary
Produce the structured summary.
See [session-summary.md](../_dtt-shared/session-summary.md).

## Mode Discipline

**Do not fix code or tests in this mode.** If you discover bugs or test gaps:

- Note them in the documentation as known issues.
- Include them in the session summary under Contradictions or Unknowns.
- Suggest `/dttt` or `/dttc` follow-ups with specific scope.

Documentation must reflect reality — or explicitly admit uncertainty.
It must never hide problems to look clean.

## Output Conventions

- In standard mode: prefer updating existing doc files over creating new ones.
- In audit mode: restructuring (creating new files, archiving old ones) is expected.
- Use the repo's existing doc conventions (format, location, naming) when they exist.
- If no conventions exist, establish a structure: README.md at root with links into a docs/ tree.
- When archiving old docs, move them to docs/archive/ — don't delete until their content
  has been absorbed into the new structure.
- Keep docs scannable: headers, short paragraphs, code references with file paths.

$ARGUMENTS
