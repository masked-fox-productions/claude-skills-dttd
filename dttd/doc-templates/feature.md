# Feature Documentation Template

Use this template when documenting a specific feature or capability.

---

## [Feature Name]

### Overview
One-paragraph summary of what this feature does and why it exists.

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
- Links to related docs, modules, or tests.
