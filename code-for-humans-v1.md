# Code for Humans First

> "Programs must be written for people to read, and only incidentally for machines to execute." — Harold Abelson

Write code that is correct, easy to understand, maintainable, and no more complex than the task requires.

## Priority Order

When principles conflict, use this order:

1. Correctness and safety
2. The user's requested scope
3. Existing contracts and project conventions
4. Simplicity and readability
5. Maintainability, reuse, and extensibility for known requirements
6. Evidence-based performance optimization

Do not sacrifice a higher-priority goal for a lower-priority one.

## Required Workflow

### 1. Understand Before Editing

- Read the relevant code, tests, configuration, callers, and nearby conventions.
- Search for existing behavior before adding a new implementation.
- Verify real APIs, signatures, dependencies, commands, config keys, environment variables, and file paths.
- Identify the actual owner of the behavior before deciding where to change code.
- Identify variation already present or explicitly required, such as multiple providers, task types, strategies, transports, or storage implementations.
- Decide what behavior is shared and what behavior must remain implementation-specific.
- Identify whether the requested change extends the existing behavior, replaces it, migrates it, or requires temporary coexistence.
- Do not guess when the answer can be found in the codebase or authoritative documentation.

### 2. Make the Smallest Coherent Change

Before adding code, consider whether the requirement can be satisfied by:

- Reusing an existing implementation
- Extending an existing contract
- Using the standard library or platform
- Using an already-installed dependency
- Simplifying or deleting existing code

Then:

- Change only what is needed to satisfy the request.
- Prefer improving the existing path over creating a parallel path.
- Fix the underlying cause in the correct owner instead of patching individual symptoms.
- Treat the smallest change as the smallest coherent change, not merely the fewest edited lines.
- Include local structural changes when they are necessary to avoid duplicated workflows, scattered special cases, or repeated provider/type branching.
- When a new implementation fully replaces an existing implementation, remove the superseded path instead of keeping old and new implementations in parallel.
- Do not create `V2`, `New`, `Legacy`, fallback, compatibility, or duplicate business paths unless coexistence or migration is explicitly required.
- Do not preserve obsolete code "just in case" when the active behavior has already moved to the replacement.
- Do not refactor, rename, reformat, or clean up unrelated code.
- Preserve backward compatibility only when the project or request requires it.
- Mention broader issues separately instead of silently expanding scope.

### 3. Design for Known Variation

Design for current and explicitly confirmed variation, not hypothetical future requirements.

- Consider reuse and extensibility from the first implementation when variation is already known.
- When multiple implementations share the same responsibility, prefer a small and stable contract that captures shared behavior.
- Keep provider-, task-, transport-, persistence-, and implementation-specific details local when they would otherwise spread through core business logic.
- For a known extension path, prefer adding a new implementation and registration over repeatedly modifying core orchestration.
- Prefer extension through implementations over growing central `if/else` or `switch` chains.
- Simple, localized conditionals are acceptable.
- Reassess the design when the same provider/type branching appears across multiple modules or every new implementation requires copying an entire workflow.
- Share stable behavior, not merely code that looks similar.
- Do not force distinct workflows into one generic implementation through many flags, type checks, optional parameters, or special cases.
- Do not add unused extension points, speculative configuration, factories, wrappers, adapters, registries, or framework layers without a current requirement.

### 4. Keep Code Simple and Readable

- Choose the most direct implementation that fits the project's existing patterns.
- Add an abstraction only when it:
  - removes meaningful duplication,
  - reduces current complexity,
  - supports known variation,
  - creates a clear ownership boundary, or
  - follows an established project pattern.
- Use names that reveal intent.
- Keep control flow direct.
- Keep functions, classes, and modules focused on cohesive responsibilities.
- Do not split code only to satisfy arbitrary size limits.
- Follow existing conventions instead of introducing an isolated personal style.
- Comments should explain why, constraints, tradeoffs, or non-obvious decisions; they should not narrate obvious code.
- Validate untrusted input at system boundaries.
- Never silently swallow errors.
- Do not add complexity for speculative performance gains; measure first.

### 5. Replace or Delete Completely

When a field, option, parameter, dependency, implementation, workflow, or feature is removed or replaced:

- Treat replacement as replacement: if the new implementation fully supersedes the old one, remove the old implementation unless coexistence, migration, rollback, or backward compatibility is explicitly required.
- Trace the old behavior through the complete active path before deleting it.
- Check models, validation, controllers, services, providers, persistence mappings, configuration, frontend types, payloads, tests, documentation, imports, registrations, scheduled jobs, listeners, and feature flags where relevant.
- Search the repository for remaining references to the removed or superseded symbols and behavior.
- Remove dead branches, unused helpers, stale comments, obsolete tests, obsolete compatibility code, unused configuration, unused registrations, and unreachable fallback paths.
- Do not keep old controllers, services, methods, DTOs, fields, configuration keys, providers, adapters, or business flows merely because they existed before.
- Do not leave the old and new workflows both callable unless the requirement explicitly calls for coexistence.
- Do not add compatibility switches, migration flags, fallback branches, or legacy adapters unless there is a current, explicit requirement for them.
- If temporary coexistence is explicitly required, keep the boundary narrow and document what still depends on the old path; do not silently make temporary compatibility permanent.
- Verify both that the removed behavior is no longer referenced and that the remaining active flow still works.
- If safe deletion cannot be completed because a real dependency still exists, report that dependency explicitly instead of leaving unexplained dead or duplicate code.

### 6. Verify the Result

- Run the narrowest relevant tests, type checks, lint checks, or build commands.
- Test observable behavior rather than implementation details.
- Cover important failure paths when practical.
- Do not weaken, delete, or rewrite valid tests merely to make a failing implementation pass.
- Do not claim completion with placeholders, hardcoded results, empty implementations, fake success paths, or unresolved TODOs.
- Review the final diff for accidental edits, duplication, unnecessary abstractions, hidden behavior changes, scattered special cases, and obsolete code left behind by a replacement.
- Search for references to removed or superseded code when the change includes replacement or deletion.
- Do not claim the change works without verification.
- If verification cannot be run, state exactly what was not checked and why.

## Completion Criteria

Before finishing, confirm:

- [ ] The requested behavior is fully implemented and stays within scope.
- [ ] The correct owner and existing implementation paths were identified before editing.
- [ ] Existing code, contracts, and interfaces were reused where appropriate.
- [ ] The change was correctly classified as extension, replacement, migration, or coexistence.
- [ ] Known variation and extension needs were considered before implementation.
- [ ] Shared responsibilities have clear boundaries.
- [ ] Implementation-specific details remain local.
- [ ] Adding a known new implementation does not require unnecessary changes across unrelated modules.
- [ ] A replacement did not leave obsolete old and new workflows running in parallel.
- [ ] Superseded controllers, services, methods, fields, DTOs, mappings, configuration, branches, tests, and registrations were removed where no longer needed.
- [ ] No unnecessary abstraction, dependency, fallback, compatibility code, configuration, or extension point was added.
- [ ] Any requested deletion or replacement was traced through the complete active path.
- [ ] Repository search found no unintended references to removed or superseded behavior.
- [ ] Relevant verification passed, or missing verification is clearly reported.
- [ ] The final diff contains no accidental, unnecessary, duplicate, dead, or obsolete code.

Write for the next human, who may be debugging this code with no context.
