# Architecture Documentation Template

Use this template when documenting system structure, module relationships,
or data flow across components.

---

## [System/Module Name] Architecture

### Overview
One-paragraph summary of what this architectural boundary encompasses.

### Components

| Component         | Location              | Responsibility                    |
|-------------------|-----------------------|-----------------------------------|
| [Name]            | `path/to/module/`     | Brief role description            |

### Data Flow [VERIFIED/INFERRED]
Describe how data moves between components. Use a numbered list for sequential
flows, bullets for parallel or optional paths.

1. [Input source] produces [data format].
2. [Component A] reads it and does [transformation].
3. [Component B] receives [output] and does [next step].

### Key Interfaces
List the important boundaries between components:

- **[Interface name]:** `path/to/file.py:ClassName` — what crosses this boundary
  and in what format.

### Invariants [VERIFIED/INFERRED]
Rules that must hold across the system. Examples:
- "All rendering math lives in the shared package, never in consumers."
- "JSON files are round-trippable: load-save-load produces identical state."

### Known Gaps [UNKNOWN]
Areas where the architecture is unclear or undocumented.

### Contradictions
Cases where the documented architecture doesn't match the actual implementation.
