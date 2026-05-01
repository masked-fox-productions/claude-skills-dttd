# Session Summary Template

Every DTT session ends with a structured summary. This is the format.

## Finding Classifications

Tag every claim produced during the session with one of these labels:

| Tag              | Meaning                                                         |
|------------------|-----------------------------------------------------------------|
| **VERIFIED**     | Directly supported by a high-trust reference or by execution    |
|                  | (a passing test, a grep result, running the code).              |
| **INFERRED**     | Derived from medium-trust references or reasoning across        |
|                  | multiple sources. Plausible, not proven.                        |
| **CONTRADICTED** | Two or more references disagree. The contradiction is surfaced, |
|                  | not silently resolved.                                          |
| **UNKNOWN**      | Could not determine from available evidence. Noted as a gap,    |
|                  | not invented.                                                   |

## Summary Structure

At the end of the session, produce:

### 1. Trust State (echo from interview)
Restate the trust levels that governed this session.

### 2. Scope
What was covered.

### 3. Changes Made
List every artifact created or modified, with file paths.

### 4. Findings

Group by classification:

**Verified:**
- [list of verified findings]

**Inferred:**
- [list of inferred findings, with reasoning]

**Contradicted:**
- [list of contradictions, with both sides stated]

**Unknown:**
- [list of gaps]

### 5. Unresolved Contradictions
Pull out any CONTRADICTED items that weren't resolved during the session.
These are the most important items — they represent known unknowns.

### 6. Suggested Follow-ups
Offer specific next sessions:
- If in `/dttd`: suggest `/dttt` to lock behavior, `/dttc` to fix gaps.
- If in `/dttt`: suggest `/dttc` with failing tests as definition of done,
  `/dttd` to update docs. Offer to open GitHub issues for discovered bugs.
- If in `/dttc`: suggest `/dttt` to add regression tests, `/dttd` to update docs.

For each suggestion, name the scope and briefly state why.

### 7. Trust Drift
Note if any trust levels should change for the next session based on what
was learned. Example:
> Tests were rated `medium` but every test I checked was accurate — consider
> `high` next time for this module.
