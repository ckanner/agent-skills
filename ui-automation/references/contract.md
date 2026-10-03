# The artifact contract

Codex's return channel carries **text only**. Everything the user needs to *see* must be
written to disk, and the final message must point at it. This file is the single source of
truth for that agreement.

## Run directory layout

```
<workspace>/.codex-runs/<run-id>/
├── schema.json          # copy of references/output-schema.json, passed to --output-schema
├── result.json          # Codex's final message, written by -o / --output-last-message
├── events.jsonl         # only when dispatched with --json
├── shots/               # every screenshot, ordered
│   ├── 01-login-empty-submit.png
│   └── 02-checkout-error.png
└── out/                 # every other produced file: exports, renders, recordings
```

Rules:

- `<run-id>` is `<YYYYMMDD-HHMMSS>-<slug>`, e.g. `20261002-153000-checkout-acceptance`.
- Screenshots go in `shots/` and are prefixed with a two-digit ordinal so their order
  survives a directory listing: `NN-<short-name>.png`.
- Everything else the task produces goes in `out/`, keeping its original filename.
- Never write to `/tmp`. Run directories live in the workspace so they survive the
  session and can be reopened or handed to the user.
- One run directory per dispatch. Re-verification after a fix is a **new** run with a
  **new** run-id.

## Prompt block

Restate the contract in every dispatch. Verbatim block to include:

```
Artifact contract (mandatory):
- Work only inside <RUN_DIR>. Do not write outside it.
- Save every screenshot to <RUN_DIR>/shots/ as NN-<short-name>.png, numbered in the order taken.
- Save every other produced file to <RUN_DIR>/out/ under its original filename.
- Never describe what you saw on screen without also saving the screenshot that shows it.
- Every check with verdict "fail" must cite at least one saved screenshot path in its
  "evidence" array. A failure with no screenshot is not a valid result.
- Your final message must satisfy the provided JSON schema exactly.
- Report the absolute paths of the artifacts you produced.
```

## Why each rule exists

| Rule | Failure it prevents |
|---|---|
| Work only inside `RUN_DIR` | Artifacts scattered where nothing will find them |
| Numbered screenshot names | Screenshot order lost, making a failure sequence unreadable |
| Screenshot required per failure | A confident text claim with nothing to verify it against |
| Schema-constrained final message | Verdicts that must be re-parsed by hand on every run |
| Absolute paths | Ambiguity when the workspace is not the process cwd |

## Reading the result

`result.json` is machine-readable. Its shape is defined by `output-schema.json`:

- `status` — overall verdict: `pass`, `fail`, or `blocked`
- `checks[]` — one entry per expectation, each with `verdict`, `expected`, `actual`,
  and `evidence` (artifact paths)
- `artifacts[]` — every file the run produced
- `blockers[]` — anything that prevented a check from being performed at all

**Always open the screenshots before acting on the verdict.** The structured field tells
you where to look; it does not replace looking.

## Handling a missing or malformed result

| Symptom | Action |
|---|---|
| `result.json` absent or empty | Read `events.jsonl` if present; otherwise re-dispatch |
| Schema violation in the final message | The run is unusable — re-dispatch, tightening the prompt |
| `blocked` status | Read `blockers[]`. Usually a permission, auth, or missing-dependency problem, not a UI defect. Fix the environment and re-dispatch |
| `fail` with empty `evidence` | Treat as unverified. Re-dispatch asking explicitly for the screenshot |
| Screenshots present but all blank/black | The executor had no usable screen access. Switch to browser use, or resolve screen permissions |
