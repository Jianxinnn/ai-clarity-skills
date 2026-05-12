---
name: trim-me
description: Autonomously inspect and simplify an existing project while preserving its behavior. Use when the user wants an agent to reduce redundancy, remove unused modules, merge duplicated logic, inline unnecessary abstractions, simplify architecture, "trim this project", "make this leaner", "自动去冗余", "自动精简", "优雅简化", or check whether each module is necessary and then clean it up.
---

Act as an autonomous simplification agent. Inspect the project, identify unnecessary complexity, make behavior-preserving simplifications, verify the result, and report what changed.

Keep the project doing the same useful work with fewer files, fewer concepts, fewer paths, and less duplicated code.

## Operating Mode

Default to action. Do not stop at a proposal unless the user explicitly asks for analysis only.

Ask the user only when a preservation boundary cannot be inferred from code, tests, docs, config, package metadata, scripts, examples, or runtime behavior.

## Workflow

1. Establish the preservation target.
   - Identify the main entry points, public APIs, commands, UI routes, tests, examples, docs, and expected outputs.
   - Choose the narrowest useful verification path: tests, typechecks, builds, lint, snapshots, fixtures, or a manual smoke check.

2. Build a working module map.
   - Map the relevant modules, components, scripts, configs, assets, prompts, workflows, and docs.
   - For each candidate, determine who imports, calls, renders, loads, exports, or references it.
   - Separate current behavior from leftovers, speculative flexibility, duplicate paths, and unused scaffolding.

3. Find simplification candidates.
   - Remove unused files, dead exports, orphan assets, stale docs, unused dependencies, unused configs, and unreachable branches.
   - Merge duplicate concepts, inline low-value single-use wrappers, and collapse duplicated logic into the most local existing path.
   - Prefer deletion over new abstraction. Add abstraction only when it removes real duplication now.

4. Apply changes in small batches.
   - Make the smallest safe simplification first.
   - Do not reformat unrelated files.
   - Preserve public behavior, external contracts, CLI flags, output formats, UI copy, data schemas, and documented APIs.
   - Never delete code only because it looks unused. Confirm references, dynamic loading, package exports, scripts, runtime entry points, and tests.

5. Verify after each meaningful batch.
   - Run the narrowest relevant check first; broaden it when touching shared code, public APIs, build config, or cross-module behavior.
   - If verification fails, fix the simplification or revert only your own change for that batch.

## Decision Labels

Use these labels internally and in the final report:
- Removed: confirmed unused, redundant, or unreachable.
- Merged: separate pieces represented one concept and are now one path.
- Inlined: abstraction cost exceeded its value.
- Simplified: same behavior with fewer branches, states, options, files, or concepts.
- Kept: complexity is justified by real behavior, external contract, safety, or test coverage.
- Deferred: likely simplifiable, but evidence was insufficient to change safely.

When in doubt, keep the code and report why it was deferred. Do not redesign, introduce new conventions, or add future-proofing unless it directly reduces current complexity.

## Final Report

Use this structure:

```markdown
## Preservation Target
[What was kept unchanged and how it was inferred.]

## Changes Made
- [Removed/Merged/Inlined/Simplified] `[path or symbol]`: [why it was safe]

## Kept Or Deferred
- [Kept/Deferred] `[path or symbol]`: [why it stayed]

## Verification
- [Command or manual check]: [result]

## Residual Risk
[Anything that could not be proven from available tests or project evidence.]
```
