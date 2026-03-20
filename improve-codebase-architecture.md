# Improve Codebase Architecture

Explore a codebase like an AI would, surface architectural friction, discover
opportunities for improving testability, and propose module-deepening refactors.

A **deep module** (John Ousterhout, "A Philosophy of Software Design") has "a
small interface hiding a large implementation." Deep modules are more testable,
more AI-navigable, and let you test at the boundary instead of inside.

## Process

### 1. Explore the codebase

Navigate the codebase organically and note where you experience friction:

- Where does understanding one concept require bouncing between many small files?
- Where are modules so shallow that the interface is nearly as complex as the implementation?
- Where have pure functions been extracted just for testability, but the real bugs hide in how they're called?
- Where do tightly-coupled modules create integration risk in the seams between them?
- Which parts of the codebase are untested, or hard to test?

The friction you encounter IS the signal.

### 2. Present candidates

Present a numbered list of deepening opportunities. For each candidate, show:

- **Cluster**: Which modules/concepts are involved
- **Why they're coupled**: Shared types, call patterns, co-ownership of a concept
- **Dependency category**: See categories below
- **Test impact**: What existing tests would be replaced by boundary tests

Do NOT propose interfaces yet. Ask: "Which of these would you like to explore?"

### 3. Frame the problem space

Before designing solutions, write an explanation of the problem space for the
chosen candidate:

- The constraints any new interface would need to satisfy
- The dependencies it would need to rely on
- A rough illustrative code sketch to make the constraints concrete — not a
  proposal, just grounding

### 4. Design multiple interfaces

Create 3+ radically different interface designs, each with a different constraint:

- **Minimal**: 1-3 entry points max
- **Flexible**: Support many use cases and extension
- **Ergonomic**: Make the default case trivial for the most common caller
- **Ports & Adapters** (if applicable): For cross-boundary dependencies

Each design should include:
1. Interface signature (types, methods, params)
2. Usage example showing how callers use it
3. What complexity it hides internally
4. Dependency strategy
5. Trade-offs

Compare designs, then give an opinionated recommendation — which is strongest
and why. Propose a hybrid if elements from different designs combine well.

### 5. Create RFC

After a design is chosen, create a refactor RFC (as a GitHub issue or doc) with:
- Problem description (friction, coupling, risk)
- Proposed interface
- Dependency strategy
- Testing strategy (new boundary tests to write, old tests to delete)
- Implementation recommendations (responsibilities, not file paths)

## Dependency Categories

### 1. In-process
Pure computation, in-memory state, no I/O. Always deepenable — merge the
modules and test directly.

### 2. Local-substitutable
Dependencies with local test stand-ins (e.g., PGLite for Postgres, in-memory
filesystem). Deepenable if the stand-in exists. Test with it running in the
test suite.

### 3. Remote but owned (Ports & Adapters)
Your own services across a network boundary. Define a port (interface) at the
module boundary. Logic is tested with an in-memory adapter. Production uses
the real adapter.

### 4. True external (Mock)
Third-party services you don't control (Stripe, Twilio). Mock at the boundary.
The deepened module takes the dependency as an injected port.

## Testing Strategy

The core principle: **replace, don't layer.**

- Old unit tests on shallow modules are waste once boundary tests exist — delete them
- Write new tests at the deepened module's interface boundary
- Tests assert on observable outcomes through the public interface, not internal state
- Tests should survive internal refactors — they describe behavior, not implementation
