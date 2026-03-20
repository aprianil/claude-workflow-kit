# [Feature/Fix Name]

**Date:** YYYY-MM-DD
**Author:** [name]
**Branch:** feature/[name]

## Problem

What's being built? Why? What pain does this solve for the user?
Be specific — name the frustration, the workaround they're using today, or
the gap in the current experience. What constraints exist?

## Research

Do this before designing anything. Bad research cascades into bad code.

### Codebase
How does similar functionality already work? What patterns exist that we
should follow or build on? List specific files, line numbers, and relevant
code snippets. Be concrete — "chat.js handles SSE streaming" is not enough.
"chat.js:142-180 uses EventSource with onmessage to append chunks" is.

### External
What do the docs say? What are established best practices for this type of
feature? Any prior art worth referencing?

## Solution

Describe the experience from the user's perspective — what they see, what
they do, what happens. No implementation details here.

## Design

How does this look and feel? Describe key interactions — hover, click,
transitions. Reference existing patterns from the codebase where possible.

Attach screenshots, sketches, or Figma links if available.

## Scope

### In
- [ ] Specific deliverable 1
- [ ] Specific deliverable 2

### Out
- What we're explicitly NOT doing (and why)

## Implementation

What's the approach? Which files need changes? This should flow naturally
from the research — not guesswork. Include file names, line numbers, and
code snippets of patterns to follow.

```
src/...
```

## Validation

Does this plan hold together? Is it complete? Check against:
- [ ] Solves the stated problem
- [ ] Follows existing codebase patterns
- [ ] Scope is clear — no ambiguity about in/out
- [ ] Files listed, approach understood before writing code

## Open Questions

Things that need answers before or during implementation.

- [ ] Question 1

## Risks

What could go wrong? Blast radius? Performance, UX, or complexity concerns?
