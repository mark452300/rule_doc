# Code for Humans First

Write code that is correct, simple, maintainable, and no more complex than the task requires.

## Priority

When rules conflict, the higher-priority rule MUST win.

1. Correctness and safety
2. User-requested scope
3. Existing contracts and project conventions
4. Simplicity and readability
5. Maintainability for known requirements
6. Evidence-based optimization

MUST NOT violate a higher-priority rule to satisfy a lower-priority rule.

---

## Core Rules

- MUST understand the existing execution and data flow before changing behavior.
- MUST preserve business invariants, transactional boundaries, data isolation, and non-obvious constraints.
- MUST fix the owning root cause instead of hiding symptoms with downstream patches.
- MUST prefer modifying the existing implementation over creating a parallel implementation.
- MUST reuse, simplify, merge, or delete existing code before adding new code.
- MUST make the smallest coherent change that fully solves the requirement.
- MUST NOT change unrelated code.
- MUST NOT keep obsolete code after a completed replacement.
- MUST verify the result before claiming completion.

---

## Understand Before Editing

Before changing code:

- Read the relevant implementation, callers, tests, configuration, and nearby conventions.
- Trace the active execution path and identify which component owns the behavior.
- Search for existing behavior before introducing new behavior.
- Verify real APIs, signatures, dependencies, config keys, commands, environment variables, and file paths when relevant.
- Determine whether the request is an extension, replacement, migration, or explicit coexistence.

Do not guess when the answer can be determined from the codebase, runtime evidence, or authoritative documentation.

Existing unusual guards, checks, and special cases MUST NOT be removed until their purpose and dependencies are understood.

---

## Trace Failures to the Root Cause

For missing, incorrect, inconsistent, or unexpected behavior:

1. Start from the observable failure.
2. Trace backward through the real execution and data flow.
3. Find the earliest meaningful point where actual behavior diverges from expected behavior.
4. Identify which component or boundary owns that incorrect state.
5. Fix that owner.
6. Verify the fix with the narrowest relevant check.

Compare representations of the same data when useful, including:

- frontend state → API payload
- request DTO → domain model
- domain model → persistence entity
- database value → response
- producer → consumer
- source data → cache or derived state

MUST NOT:

- add downstream fallbacks merely to hide upstream defects
- duplicate correction logic across multiple consumers
- repeatedly normalize the same invalid state at different layers
- treat the final visible symptom as the root cause without tracing how it was produced

Prefer fixing the earliest authoritative boundary that can safely restore the intended invariant.

---

## Prefer Existing Code

Evaluate changes in this order:

1. Reuse
2. Modify
3. Simplify or merge
4. Delete obsolete logic
5. Add only when necessary

Prefer improving an existing flow over appending another:

- branch
- callback
- handler
- wrapper
- pipeline stage
- service
- manager
- processor
- resolver

MUST NOT create `V2`, `New`, `Legacy`, fallback, compatibility, or duplicate business paths unless explicitly required.

MUST NOT rewrite a working subsystem when a smaller coherent change can satisfy the requirement.

If a simple requirement unexpectedly touches many unrelated files, re-check the design.

---

## Design Only for Known Variation

Design for requirements that exist now or are explicitly confirmed.

### Known variation

Use the smallest stable extension point justified by the current requirements.

When multiple real implementations share one responsibility, a small stable contract is acceptable.

Keep provider-, task-, transport-, persistence-, and implementation-specific details local.

### Unconfirmed variation

Keep boundaries clean and implementation details localized.

MUST NOT introduce abstractions, extension mechanisms, factories, registries, adapters, or generic frameworks solely because something may be needed later.

Words such as:

- likely
- possible
- may
- might
- could be needed later

do not count as confirmed requirements.

Do not generalize from one scenario unless the project already demonstrates real variation.

Simple localized conditionals are acceptable.

---

## Keep Business Flow Cohesive

Prefer one readable business flow over many tiny methods that require navigation to understand.

MUST NOT extract logic merely to make a method shorter.

MUST NOT create unnecessary chains such as:

- `handleXxx`
- `processXxx`
- `prepareXxx`
- `buildXxx`
- `doXxx`

Extract code only when there is a concrete reason:

- meaningful responsibility
- actual reuse
- substantial complexity
- a real architectural boundary
- clear readability improvement

Avoid pass-through methods that only rename, wrap, forward, or delegate calls.

Keep simple scenario-specific logic local when extraction would only add indirection.

Follow existing project conventions instead of introducing an isolated personal style.

Comments should explain:

- why something exists
- constraints
- tradeoffs
- non-obvious decisions

Do not narrate obvious code.

Validate untrusted input at system boundaries.

Never silently swallow errors.

---

## Replace and Delete Cleanly

Replacement means replacement unless coexistence, migration, rollback, or backward compatibility is explicitly required.

Before deleting old behavior:

- trace its complete active path
- search the repository for remaining references
- inspect affected models, services, controllers, providers, mappings, configuration, frontend types, jobs, listeners, registrations, tests, and feature flags where relevant

After replacement:

- remove dead branches
- remove obsolete helpers
- remove stale comments
- remove obsolete tests
- remove unused configuration
- remove unused registrations
- remove unreachable fallback paths

MUST NOT leave old and new implementations both callable unless coexistence is explicitly required.

If deletion is blocked by a real dependency, report it instead of silently leaving duplicate or dead code.

---

## Verification

Before claiming completion:

- Run the narrowest relevant tests, type checks, lint checks, or build commands.
- Verify observable behavior and important failure paths where practical.
- Do not weaken valid tests merely to make an implementation pass.
- Do not claim success with placeholders, hardcoded results, fake success paths, empty implementations, or unresolved TODOs.
- Review the final diff for unrelated edits, duplicate paths, unnecessary abstractions, hidden fallbacks, obsolete code, and accidental complexity.
- Confirm every new method, class, file, or abstraction has a concrete reason to exist.
- If verification cannot run, state exactly what was not verified and why.

---

## Final Check

Before finishing, verify:

- [ ] The requested behavior is complete and within scope.
- [ ] The real execution and data flow were understood.
- [ ] The owning root cause was fixed instead of only treating the symptom.
- [ ] Existing code was reused or modified before unnecessary code was added.
- [ ] No speculative abstraction, duplicate path, or obsolete implementation remains.
- [ ] The business flow remains cohesive and readable.
- [ ] Relevant verification passed, or missing verification was explicitly reported.

Write for the next human who may need to debug this code with no context.