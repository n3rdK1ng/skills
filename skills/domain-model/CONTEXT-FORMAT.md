# CONTEXT.md Format

`CONTEXT.md` is the project's domain glossary. It lives at the repository root and gives names to the concepts in the codebase.

The glossary is shared infrastructure: `/domain-model` writes it, `/improve-codebase-architecture` reads it and adds to it inline, and humans consult it when onboarding.

## Structure

````markdown
# CONTEXT

## Domain

One paragraph: what this project is about, in domain terms — not technical terms. "A portal where employees record attendance and managers approve time off" — not "a Hono + tRPC backend with a React frontend."

## Terms

### Order
A customer's request for goods. An Order is created when a customer submits a cart and has a lifecycle of placed → confirmed → shipped → delivered. Cancelable until confirmed.

### Fulfilment
The process of preparing an Order for the customer. Distinct from Shipping — Fulfilment ends when the box leaves the warehouse.

…
````

## Rules

- **One term per heading.** No combined headings ("Order and Fulfilment"). If two terms always travel together, that's a smell — give the pair its own term.
- **Definitions are short.** Two or three sentences. If a term needs a long explanation, it's actually several terms.
- **Cross-reference, don't repeat.** If "Fulfilment ends when the box leaves" refers to Shipping, link Shipping; don't redefine it.
- **Avoid circular definitions.** Don't define Order in terms of Fulfilment and Fulfilment in terms of Order. Pick one as primitive.
- **No code, no file paths, no class names.** This is the *domain* glossary, not a code map. "An Order is created when a customer submits a cart" — not "an Order is a row in the `orders` table."
- **Status verbs match the team's mouth.** If the team says "placed," don't write "submitted." The glossary records the language people use; it doesn't propose a new one.

## When to update

- A new concept appears in a design conversation — add it.
- An existing term is being used in two different ways — split it, name each clearly.
- A term in the glossary has stopped matching how the team talks — sharpen the definition, or rename it.

Glossaries that get stale get ignored. Update inline, in the same conversation that surfaced the change — don't queue it for "later."
