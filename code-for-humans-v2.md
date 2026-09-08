# Code for Humans First

Write code that is correct, easy to understand, maintainable, and no more complex than the task requires.

## Priority

When rules conflict, prefer:
1. Correctness and safety
2. The user's requested scope
3. Existing contracts and project conventions
4. Simplicity and readability
5. Maintainability and extensibility for known requirements
6. Evidence-based optimization

## Non-Negotiable Rules

- MUST understand the existing flow before changing it.
- MUST prefer modifying the existing implementation over creating a parallel one.
- MUST fix the underlying cause instead of stacking patches around symptoms.
- MUST remove superseded code when replacement is complete.
- MUST NOT create `V2`, `New`, `Legacy`, fallback, compatibility, or duplicate business paths unless explicitly required.
- MUST NOT keep obsolete code "just in case".
- MUST NOT add abstractions or extension points for hypothetical future requirements.
- MUST NOT fragment cohesive business flows into unnecessary `handleXxx`, `processXxx`, `prepareXxx`, `buildXxx`, `doXxx`, or similar helper chains.
- MUST NOT create scenario-specific helpers or services when simple one-time logic is clearer in the existing flow.
- MUST NOT rewrite a working subsystem when a smaller coherent change can satisfy the requirement.
- MUST NOT change unrelated code.
- MUST verify the result before claiming completion.

## 1. Understand Before Editing

- Read relevant code, tests, configuration, callers, and nearby conventions.
- Search for existing behavior before adding new behavior.
- Verify real APIs, signatures, dependencies, config keys, commands, environment variables, and file paths.
- Identify the actual owner and trace the active business flow.
- Determine whether the request is an extension, replacement, migration, or temporary coexistence.
- Identify variation that already exists or is explicitly required.
- Do not guess when the answer can be found in the codebase or authoritative documentation.

## 2. Improve Existing Code Before Adding More

Before adding code, consider reuse, simplification, merging, restructuring, or deletion.

- Make the smallest coherent change, not merely the fewest-line change.
- Prefer improving the existing flow over appending another branch, helper, handler, callback, wrapper, or stage.
- If repeated special cases make a flow hard to understand, simplify the structure before adding another one.
- Do not refactor, rename, reformat, or clean up unrelated code.
- Preserve backward compatibility only when the project or request requires it.
- Choose the most direct implementation that fits existing project patterns.
- Use names that reveal intent and keep control flow direct.
- Do not add complexity for speculative performance gains; measure first.

## 3. Design Only for Known Variation

Design for current and explicitly confirmed variation, not hypothetical future requirements.

- When multiple implementations share one responsibility, use a small stable contract for genuinely shared behavior.
- Keep provider-, task-, transport-, persistence-, and implementation-specific details local.
- For a known extension path, prefer adding an implementation and registration over repeatedly modifying core orchestration.
- Prefer implementations over growing central `if/else` or `switch` chains when the same variation appears repeatedly.
- Simple, localized conditionals are acceptable.
- Share stable behavior, not merely code that looks similar.
- Do not force distinct workflows into one generic implementation through many flags, type checks, optional parameters, or special cases.
- Do not add speculative factories, wrappers, adapters, registries, configuration, or framework layers.

## 4. Keep Business Flow Cohesive

- Do not extract a function merely to name a few lines of business logic.
- Before adding a method, check whether the logic naturally belongs in the current method or an existing owner.
- Extract only for meaningful responsibility, reuse, substantial complexity, a real boundary, or clear readability improvement.
- Avoid pass-through methods that only rename, wrap, forward, or delegate a call.
- Do not create methods solely to make another method look shorter.
- A short method is not automatically a good method; optimize for readable control flow, not method count.
- Keep simple scenario-specific logic local when extraction would only add indirection.
- Prefer one readable flow over many tiny methods that require navigation to understand the operation.
- Keep related business steps close enough to understand the full flow without unnecessary navigation.
- Do not split code to satisfy arbitrary size limits, line counts, or stylistic ideas of cleanliness.
- Follow existing project conventions instead of introducing an isolated personal style.
- Comments should explain why, constraints, tradeoffs, or non-obvious decisions; do not narrate obvious code.
- Validate untrusted input at system boundaries.
- Never silently swallow errors.

## 5. Replace, Delete, and Verify

When something is removed or replaced:

- Replacement means replacement unless coexistence, migration, rollback, or backward compatibility is explicitly required.
- Trace the old behavior through the complete active path before deleting it.
- Check relevant models, controllers, services, providers, persistence mappings, configuration, frontend types, payloads, tests, imports, registrations, jobs, listeners, and feature flags.
- Search the repository for remaining references to removed or superseded behavior.
- Remove dead branches, obsolete helpers, stale comments, obsolete tests, unused configuration, unused registrations, and unreachable fallback paths.
- Do not leave old and new workflows both callable unless coexistence is explicitly required.
- If temporary coexistence is required, keep it narrow and make the remaining dependency explicit.
- If deletion is blocked by a real dependency, report it instead of silently leaving duplicate or dead code.

Before finishing:

- Run the narrowest relevant tests, type checks, lint checks, or build commands.
- Test observable behavior and important failure paths where practical.
- Do not weaken valid tests merely to make a failing implementation pass.
- Do not claim completion with placeholders, hardcoded results, fake success paths, empty implementations, or unresolved TODOs.
- Review the final diff for duplicate paths, unnecessary abstractions, fragmented helper chains, incremental patches, obsolete code, and accidental edits.
- Check that every new method or abstraction has a concrete reason to exist.
- Check that the final business flow can be understood without unnecessary jumps across tiny methods.
- If verification cannot run, state exactly what was not checked and why.

## Final Check

Confirm that:
- The requested behavior is complete and within scope.
- Existing code was reused or improved where appropriate.
- No speculative architecture, unnecessary patch, helper, wrapper, abstraction, or parallel path was added.
- Replaced behavior was removed where no longer required.
- The remaining business flow is cohesive and easy to follow.
- Verification passed, or missing verification is explicitly reported.
- The final diff contains no accidental, duplicate, dead, obsolete, or unnecessary code.

Write for the next human, who may be debugging this code with no context.
