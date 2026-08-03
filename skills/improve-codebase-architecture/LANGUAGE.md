# Architecture Language

The vocabulary the `improve-codebase-architecture` skill uses. Use these terms exactly — don't drift into "component," "service," "API," or "boundary." Consistent language is what makes architectural conversations cumulative instead of starting from scratch each time.

## Terms

### Module
Anything with an interface and an implementation. A function, a class, a package, a slice of a system. The unit at which we reason about depth.

### Interface
Everything a caller must know to use the module:

- Types and signatures
- Invariants the module upholds
- Error modes and when they fire
- Ordering constraints ("must call A before B")
- Configuration the caller has to provide

Not just the type signature. The interface is the full contract — every fact a caller has to learn before they can use the module safely.

### Implementation
The code inside the module. The implementation can be arbitrarily complex — depth measures the ratio between this and the interface.

### Depth
The leverage at the interface: a lot of behaviour hidden behind a small interface.

- **Deep** — high leverage. Callers learn little, get a lot.
- **Shallow** — interface is nearly as complex as the implementation. The module pays its own weight, but no more.

A deep module is a force multiplier. A shallow module is a tax: callers pay the cost of learning it without getting compression in return.

### Seam
Where an interface lives — a place behaviour can be altered without editing the module in place. Tests live at seams. Adapters plug into seams. (Use this term, not "boundary.")

### Adapter
A concrete thing satisfying an interface at a seam. The Prisma adapter and the in-memory adapter both satisfy the same persistence interface; both are adapters at the persistence seam.

### Leverage
What callers get from depth. The reason to deepen is to give callers leverage.

### Locality
What maintainers get from depth. Change, bugs, and knowledge concentrate in one place. When a behaviour lives inside a deep module, you fix it once and ship.

## Key principles

### Deletion test
Imagine deleting the module. If complexity vanishes, the module was a pass-through — delete it. If complexity reappears scattered across N callers, the module was earning its keep.

The deletion test is the cheapest way to spot shallow modules. Apply it before proposing a refactor: if the module fails the test, the candidate is "delete," not "deepen."

### The interface is the test surface
Tests should exercise behaviour through the interface, not by reaching behind it. If you can't write good tests through the interface, the interface is wrong — fix it instead of poking at internals.

A pure function extracted only for testability is a yellow flag. Real bugs often hide in how the function is *called* — wrong ordering, missing setup, stale state. Tests on the extracted function pass while the surrounding code stays broken. Push tests up to the seam that owns the behaviour.

### One adapter = hypothetical seam. Two adapters = real seam.
Don't introduce a seam for a single implementation. The interface inevitably bends toward the only adapter that exists, and the "flexibility" never pays off. A second adapter — even a test fake — forces the interface to be honest.

If you can't name a plausible second adapter, you don't have a seam, you have a refactor in disguise.

### Deepen, don't subdivide
The wrong reaction to a complex module is splitting it into five smaller ones. That replaces one interface with five and moves the complexity *between* modules instead of inside one.

The right question is: can the existing interface absorb more behaviour? Can the implementation grow inside the same seam? Depth comes from concentration, not from chopping.

### Locality before reuse
Reuse is a side effect of depth, not a goal. A module extracted "in case someone else needs it" usually serves no one well — its interface is shaped for a hypothetical caller, and the original caller pays the cost of indirection. Build for the locality of one caller first; let the second caller pull the seam into existence.
