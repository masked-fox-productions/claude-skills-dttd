# Architecture Documentation Template

Use this template when documenting system structure, module relationships,
or data flow across components.

**Audience:** Developers (people extending, debugging, or contributing to the system).
**Level:** System or subsystem (link down to component docs, link up to README/overview).

---

## [System/Module Name] Architecture

### Overview
One-paragraph summary: what this architectural boundary encompasses, why it exists
as a distinct unit, and where it sits in the broader system.

### Components

| Component         | Location              | Responsibility                    |
|-------------------|-----------------------|-----------------------------------|
| [Name]            | `path/to/module/`     | Brief role description            |

For each component, link to its own documentation if it exists.

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
