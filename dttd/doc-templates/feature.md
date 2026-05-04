# Feature Documentation Template

Use this template when documenting a specific feature or capability.

**Audience:** Choose based on the feature scope:
- User-facing feature → write for **users** (how to use it, what it does for them)
- Internal mechanism → write for **developers** (how it works, how to extend it)
- Both → split into a user section (usage, examples) and a developer section (internals)

**Level:** Subsystem or component. Link up to the parent architecture doc and
down to related component docs or format specs.

---

## [Feature Name]

### Overview
One-paragraph summary of what this feature does and why it exists.
For user-facing features, lead with the user benefit, not the implementation.

### Current Behavior [VERIFIED/INFERRED]
What the feature actually does today, based on code reading and test evidence.

- **Entry point:** `path/to/file.py:function_name`
- **Data flow:** Brief description of how data moves through the system.
- **Configuration:** What controls this feature's behavior.

### Intended Behavior [VERIFIED/INFERRED/UNKNOWN]
What the feature is supposed to do, based on trusted documentation or explicit
user statements. If this differs from current behavior, say so clearly.

### Usage
```
# Minimal usage example, verified against actual code
```

### Edge Cases and Limitations
- Known limitations (tagged with classification).
- Known bugs (reference failing tests or issue numbers if available).

### Contradictions
Any cases where docs, tests, and code disagree. State all sides.

### Open Questions
Things that could not be determined from available evidence.

### Related
- Links to related docs (parent subsystem, sibling features, format specs).
