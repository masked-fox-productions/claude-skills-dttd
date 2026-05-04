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
6. Assess audience coverage: which audiences are served, which are neglected.
7. Assess hierarchical completeness: is there system-level, subsystem-level, and
   component-level documentation? Where are the gaps?
8. If the repo has an existing doc structure that's coherent, ask before reorganizing.
   If docs are scattered with no clear pattern, propose a structure (README + docs/ tree).
9. Offer to execute the reorganization, or hand off specific sections as standard-mode follow-ups.

Audit mode produces a **doc inventory and restructuring plan** rather than updated docs directly.
It's the reconnaissance pass that informs targeted `/dttd` sessions.

## Core Principles

### Audience Awareness

Documentation serves three distinct audiences. Every doc should know which
audience it's talking to, and the overall doc structure should serve all three.

| Audience       | What they need                                      | Where it lives            |
|----------------|-----------------------------------------------------|---------------------------|
| **Buyers**     | What is this? Why should I care? Is it for me?      | README.md                 |
| **Users**      | How do I install, configure, and use this?           | docs/guides/, tutorials/  |
| **Developers** | How does it work? How do I extend or contribute?     | docs/architecture, data-formats/, inline |

**README.md is a buyer document.** It should answer three questions fast:
1. What does this product do? (one sentence)
2. Why does it exist / what problem does it solve? (one paragraph)
3. Is it for me? (feature list, quick start, and a path to deeper docs)

The README should not bury the lede with architecture details or developer
concerns. Those belong in docs/ and are linked from the README.

**Guides and tutorials are user documents.** They assume the reader has decided
to use the product and wants to accomplish something specific. Task-oriented,
not system-oriented.

**Architecture, data formats, and API docs are developer documents.** They
explain how the system works, why it's structured the way it is, and how to
extend it. System-oriented, not task-oriented.

When auditing or producing docs, check that each document serves its intended
audience and doesn't drift into serving a different one. A README that reads
like an architecture doc has lost its buyer audience. A tutorial that explains
internal data structures has lost its user audience.

### Hierarchical Structure

Documentation should describe the system at three levels of granularity.
Missing levels create gaps where readers can't zoom in or out.

| Level          | Describes                                    | Example                          |
|----------------|----------------------------------------------|----------------------------------|
| **System**     | The product as a whole, its purpose and parts | README, architecture overview    |
| **Subsystem**  | A major component and its responsibilities    | Editor docs, rendering pipeline  |
| **Component**  | A specific module, format, or interface       | JSON format spec, API reference  |

Each level should:
- **Name what it covers** — clear scope boundary
- **State its purpose** — why this piece exists
- **Describe its interfaces** — what goes in, what comes out, what it connects to
- **Link down** to more detailed docs (system → subsystem → component)
- **Link up** to the broader context (component → subsystem → system)

When producing documentation, check for missing levels. A repo with great
component docs but no system overview forces readers to assemble the big
picture themselves. A repo with only a high-level README and no component
docs forces developers to read source code for every detail.

### Honesty Over Completeness

Documentation must reflect reality — or explicitly admit uncertainty.
It must never hide problems to look clean.

- **Current behavior** sections describe what the system *actually does*, verified
  by reading code or running tests. Not what someone wished it did.
- **Intended behavior** sections describe what the system *should* do, sourced
  from high-trust references or explicit user statements. Clearly separated from
  current behavior.
- **Every claim is tagged** with its finding classification.
- **Contradictions are stated explicitly**, not resolved silently.
  Format: "Docs say X. Code does Y. Tests assert Z."
- **Unknowns are admitted**, not papered over.

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
Write documentation that follows the core principles above: audience-aware,
hierarchically structured, and honest.

Pick the appropriate template from [doc-templates/](doc-templates/) if one fits.

When deciding where new content belongs:
- If it's about what the product is and why → README
- If it's about how to use it → guides/ or tutorials/
- If it's about how it works internally → architecture, data-formats/, or inline docs
- If it's a new subsystem or component → create a doc at the right level and link it
  from the parent level

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
- README changes should be evaluated against the buyer audience — does this help
  someone decide whether to adopt the product?

$ARGUMENTS
