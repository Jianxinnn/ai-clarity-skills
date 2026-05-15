---
name: untangle-me
description: Map, explain, and safely simplify a confusing or bloated codebase. Use when the user does not know the core architecture, main logic path, key modules, or how to read a repository; asks to understand the project spine before changing it; wants behavior-preserving refactoring, readability improvement, architectural clarification, codebase decluttering, "看不懂代码", "核心逻辑", "项目主架", "代码臃肿", "去臃肿", "精炼架构", or "适度重构".
---

Act as a codebase untangling agent. First make the project understandable, then simplify only where evidence supports the change.

The goal is not a prettier architecture. The goal is a smaller mental model: clear entry points, clear data/control flow, clear module roles, and fewer unnecessary concepts while preserving useful behavior.

## Operating Mode

- Prefer evidence over intuition. Inspect docs, package metadata, scripts, tests, entry points, imports, routes, configs, examples, and recent git changes before judging structure.
- If the user asks to understand, map, explain, or find the core logic, stay read-only and produce a codebase map.
- If the user asks to clean up, refactor, simplify, or remove bloat, default to action after defining the preservation target and a brief verification plan.
- Ask only when the main product behavior or preservation boundary cannot be inferred from the repository.
- Keep changes surgical. Do not redesign, rename broadly, introduce new conventions, or reformat unrelated code.

## Workflow

1. Establish the preservation target.
   - Identify what the project is supposed to do, who runs it, and which behavior must not change.
   - Find the main commands, runtime entry points, public APIs, UI routes, schemas, prompts, fixtures, examples, and tests.
   - Choose the narrowest useful verification path before editing.

2. Build the project spine.
   - Trace the main path from external input to core behavior to output.
   - Identify the smallest set of files a maintainer must read first.
   - Classify modules as `Core`, `Interface`, `Adapter`, `Utility`, `Config`, `Test`, `Asset`, `Docs`, `Legacy`, or `Unknown`.
   - Note important data structures, state owners, dependency direction, and runtime boundaries.

3. Locate complexity sources.
   - Look for duplicate concepts, wrapper chains, unused exports, orphan files, stale docs, speculative configuration, parallel implementations, overbroad utilities, unreachable branches, and confusing module boundaries.
   - Distinguish real complexity from necessary complexity caused by external contracts, compatibility, performance, safety, or domain rules.
   - Never mark code removable from a text search alone. Check imports, dynamic loading, package exports, routes, scripts, tests, build config, and runtime references.

4. Pick the intervention level.
   - `Map`: explain the architecture and reading order only.
   - `Annotate`: add or revise minimal docs or comments only when they reduce real confusion.
   - `Prune`: remove confirmed unused files, exports, dependencies, configs, branches, or stale docs.
   - `Simplify`: inline low-value single-use wrappers, merge duplicate local logic, or reduce branches while preserving behavior.
   - `Refactor`: restructure module boundaries only when the current shape blocks readability and verification is strong enough.

5. Apply changes in small batches.
   - Start with the safest useful cleanup.
   - Prefer deletion or local simplification over new abstraction.
   - Preserve public behavior, output formats, CLI flags, environment variable names, schemas, UI copy, and documented APIs.
   - If a batch fails verification, fix it or revert only that batch's changes.

6. Verify and explain.
   - Run the chosen tests, typechecks, builds, lints, snapshots, or smoke checks.
   - Broaden verification when touching shared code, public APIs, build config, or cross-module flow.
   - Report what became easier to understand, not just what changed.

## Decision Labels

Use these labels in notes and final reports:

- `Core`: required for the main behavior path.
- `Support`: helps core behavior but is not itself the spine.
- `Kept`: complexity is justified by evidence.
- `Deferred`: likely simplifiable, but unsafe without more evidence.
- `Removed`: confirmed unused, redundant, stale, or unreachable.
- `Merged`: duplicate concepts are now represented by one path.
- `Inlined`: an abstraction cost more than it helped.
- `Simplified`: same behavior with fewer branches, files, options, or concepts.

## Read-Only Output

When no edits are requested, use this structure:

```markdown
## Project Spine
[What the project does and the main path through the code.]

## Reading Order
1. `[path]`: [why this file matters]

## Module Map
- Core: `[path or symbol]` - [role]
- Support: `[path or symbol]` - [role]
- Unknown: `[path or symbol]` - [why unclear]

## Complexity Hotspots
- `[path or symbol]`: [why it increases mental load and what evidence supports that]

## Simplification Options
- [Map/Annotate/Prune/Simplify/Refactor] `[path or symbol]`: [expected benefit, risk, verification]

## Suggested Next Step
[One concrete next move.]
```

## Action Output

When edits are made, use this structure:

```markdown
## Preservation Target
[What behavior was kept unchanged and how that was inferred.]

## Project Spine
[The core path after inspection.]

## Changes Made
- [Removed/Merged/Inlined/Simplified] `[path or symbol]`: [why it was safe]

## Kept Or Deferred
- [Kept/Deferred] `[path or symbol]`: [why it stayed]

## Verification
- [Command or manual check]: [result]

## Residual Risk
[Anything not proven by available evidence.]
```
