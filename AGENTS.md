<!-- /AGENTS.md -->

# Repository working rules

The key words MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY are interpreted as described in BCP 14 (RFC 2119 and RFC 8174) only when written in uppercase.

## Project context

During initial setup, the agent MUST populate these fields from confirmed requirements and repository evidence. Unresolved fields MUST remain `Unknown`; inapplicable fields MAY be removed. Requirements and commands MUST NOT be invented. Ordinary tasks MUST NOT trigger unrelated setup.

- Mission: [One sentence.]
- Product contract: [Authoritative requirements files.]
- Environments: [Supported languages, runtimes, platforms, and versions.]
- Setup: [Canonical setup command and lockfile policy.]
- Validation: [Canonical format-check, lint, type-check, test, and build commands; environment-dependent checks.]
- Generation: [Sources, outputs, and regeneration commands, if applicable.]
- Language: [Identifier, comment, documentation, and collaboration languages.]
- Workflow: [Branch, review, merge, release, publication, and versioned-artifact conventions.]

## Authority and evidence

- The agent MUST read applicable repository instruction files, including nested instructions and supported overrides, before editing within their scope, and MUST follow the executing environment's instruction precedence.
- The product contract MUST define accepted behavior; code and executable configuration MUST establish current implementation details without overriding accepted requirements.
- Definitions MUST have one authoritative source. Accepted decisions and authorized behavior changes MUST update affected documentation; decision records SHOULD explain rationale.
- Notes, examples, proposals, and placeholders MUST NOT be treated as accepted requirements. Reports MUST distinguish facts, inferences, assumptions, and unknowns.
- Uncertain external behavior material to correctness MUST be checked against version-specific authoritative documentation or a discriminating experiment.
- Conflicts MUST be resolved using applicable precedence and evidence. Clarification MUST be requested when unresolved requirements block correct work; independent work SHOULD continue.

## Scope and design

- Changes MUST form the smallest coherent solution to the assigned task. Unrelated cleanup, formatting, renames, refactors, and architectural changes MUST NOT be included.
- Architecture and conventions MUST be preserved unless the task justifies changes; existing layout MUST NOT be mistaken for a requirement.
- Code SHOULD favor explicit behavior, simple control flow, focused functions, cohesive modules, and removal of unnecessary code within scope.
- Abstractions MUST serve a concrete repeated concept, system boundary, domain concept, or invariant. Speculative frameworks, extension points, and compatibility layers SHOULD NOT be introduced.
- Ownership MUST be clear for mutable state, side effects, and long-lived asynchronous work. Such work MUST define cancellation and stale-result handling.
- Values SHOULD be derived instead of stored as synchronized duplicate state. Domain types SHOULD make invalid states difficult to express.
- Deterministic logic SHOULD be separated from effects when clarity or testability improves. Complexity MUST be judged by state, ownership, concurrency, and control flow rather than line count.
- Existing mechanisms MUST be understood before adding retries, delays, caches, recovery, synchronization, or additional state. Work made unnecessary by earlier changes MUST NOT be performed.

## Implementation and readability

- Code MUST follow language idioms unless documented requirements justify deviation. Standard facilities and existing dependencies SHOULD be preferred.
- Names SHOULD be concise and precise: nouns for concepts, verbs for actions, and state or question-form names for Booleans. Mutation and I/O SHOULD be visible through structure or naming.
- Imports MUST be explicit unless ecosystem conventions justify otherwise. Re-exports SHOULD belong to an intentional public API.
- Untrusted input MUST be validated at narrow boundaries and represented appropriately before further use. Types SHOULD enforce useful invariants without unnecessary wrappers.
- Failures MUST be handled explicitly with diagnostic context. Unsafe, incomplete, or indeterminate outcomes affecting correctness MUST be exposed; fallbacks MUST NOT conceal ambiguous partial success.
- Structural refactoring MUST preserve observable behavior and contractual timing, ordering, cancellation, and asynchronous guarantees unless changes are authorized. Incidental execution duration MUST NOT be treated as a fixed contract.

## Comments, headers, and generated files

- Comments MUST add information beyond names, types, and code, and SHOULD explain invariants, ownership, constraints, or non-obvious decisions.
- Created or modified repository-authored files MUST contain an accurate repository-relative path header beginning with `/`, using native comment syntax where supported. Required first-line directives MUST precede it.
- Generated and tool-owned files MAY omit incompatible headers. Unrelated files MUST NOT be changed solely for headers; unsupported comment syntax MUST NOT be introduced.
- Generated artifacts MUST be changed through their authoritative source or generator and regenerated when affected. Generated files SHOULD be marked as such where supported.

## Tests and validation

- Tests MUST verify behavior and stable contracts, SHOULD cover rejection paths and regressions, and SHOULD use the lowest faithful level. Placeholder tests MUST NOT be added; assertions solely preserving obsolete architecture MUST NOT be retained.
- Behavior changes MUST receive appropriate automated coverage where feasible. Otherwise, the agent MUST explain the limitation and perform relevant available validation.
- Tests MUST be reproducible, controlling time, randomness, concurrency, and numerical tolerances where relevant.
- Focused fakes SHOULD be used when faithful; real integration coverage MUST verify relevant external behavior that fakes cannot represent faithfully.
- Focused checks SHOULD run during iteration. Available affected canonical checks MUST run before completion; broader checks SHOULD run for shared behavior or configuration changes.
- Full local validation means every listed canonical local gate. It MUST run when required by the task or workflow and after final integration, subject to reported environment limitations.
- Verification SHOULD be non-mutating and separate from generation or correction. CI SHOULD reuse local entry points and MUST NOT replace available local development feedback.
- Defects introduced by the task MUST be fixed. Checks, baselines, casts, and suppressions MUST NOT be manipulated merely to hide failures. Unrelated failures MUST be reported without expanding scope.

## Dependencies and workflow

- Installation MUST use committed lockfiles where applicable. Manifests and lockfiles MUST be updated together through the package manager; lockfiles MUST NOT be edited manually.
- Dependencies MUST satisfy concrete requirements. Repeated setup MUST be idempotent and separate from validation. Lifecycle automation MUST reuse local entry points and MUST NOT regenerate lockfiles unless required by that operation.
- Documented workflows MUST be followed. Missing workflows MUST NOT be invented to satisfy these rules.
- Before editing a Git repository, the agent MUST inspect its branch and working-tree changes. Work MUST use the assigned checkout, or the current checkout if unassigned and isolation is unnecessary.
- Another contributor's work MUST NOT be overwritten or reverted. Operations on another task's branch MUST require authorization covering that operation.
- Pushes, pull requests, merges, releases, and publication MUST have explicit authorization, which MUST remain effective unless revoked or superseded.
- Commits MUST be focused; pull request titles MUST follow Conventional Commits. Hooks and protections MUST NOT be bypassed to evade failures.
- Secrets, credentials, caches, and runtime state MUST NOT be committed. Generated artifacts and build output MAY be committed only when intentionally versioned.

## Delegation and integration

- The parent MUST assess substantive tasks for delegation and SHOULD delegate when it materially improves speed or quality. Small tasks SHOULD remain local.
- Delegation MUST define objective, scope, ownership, constraints, and expected result. Concurrent editors MUST use separate branches and worktrees; read-only agents MAY share a checkout.
- The parent MUST own integration and validation. Subagents MUST report findings, file references, checks, and blockers, and MUST NOT recursively delegate without authorization.
- Substantive integrated changes SHOULD receive independent review of the final revision.
- The parent MAY integrate delegated work; unrelated branches MUST require an integration assignment. Base updates MAY occur when authorized.
- Integration MUST proceed one branch at a time. Conflicts MUST be assessed in context and compatible behavior preserved; mechanical `ours` or `theirs` selection MUST NOT replace assessment.
- Checks MUST follow each integration step. Integration edits MUST be necessary for coexistence; incompatible requirements MUST be surfaced before dependent work.

## Completion

- The agent MUST check requirements alignment and the final diff, and SHOULD check for dead state, stale names, duplication, obsolete tests, and unnecessary compatibility code.
- Completion reports MUST state changes, rationale, validation results, and material limitations or unknowns. Detail MUST scale to the task.
- Reports MUST identify unavailable checks, relevant validation methods, and whether failures are task-caused, pre-existing, unrelated, or unexplained. Unavailable checks MUST NOT be claimed as passing.