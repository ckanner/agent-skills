# Playbook: driving a native desktop application

Use when the work needs hands on a GUI application — a 3D package, an editor, a design
tool — and the point is the file it produces.

## 1. Prefer a scriptable path when one exists

Many GUI applications expose a scripting or headless interface that is faster, repeatable,
and far more reliable than driving the UI. Check for one **before** reaching for computer
use. The test is whether the task can be expressed as a command that runs to completion
without a human looking at a window.

When a scriptable path exists, dispatch it as an ordinary task and skip the rest of this
playbook. Only continue here when the work genuinely requires the GUI.

## 2. State the deliverable as a file path

The value of the run is the produced file. Say exactly where it must land:

```
Produce <deliverable> and save it to <RUN_DIR>/out/<filename>.
The run is not complete until that file exists and is non-empty.
Also save a screenshot of the application state after the final step.
```

Without an explicit path, the file ends up somewhere in the user's home directory and the
run looks like it produced nothing.

## 3. Assume the screen may be unreachable

A process spawned from a terminal often has **no screen-recording permission**, in which
case computer use cannot capture anything and may fail silently or produce blank images.

Detect this early by requiring a screenshot of the first step. If it comes back blank, or
the run reports a permission blocker, stop and report it as an environment problem — do
not keep dispatching attempts that cannot succeed.

## 4. Bound the interaction

GUI automation is the least reliable part of any delegation. Reduce its surface:

- Have the executor **set up state by file** and use the GUI only for the steps that
  require it.
- Give an explicit stopping condition ("stop after the export completes") so a confused
  run does not wander through menus.
- Prefer one dispatch per deliverable over one dispatch per feature.
- Never let this session and the executor drive the desktop at the same time.

## 5. Recover and verify the artifact

1. Confirm the file exists at the promised path and has a plausible size.
2. Open it. For a rendered image, look at it — a file that exists is not a file that is
   correct.
3. Cross-check against the screenshot of the final application state.
4. Only then treat the deliverable as produced.

## Common pitfalls

- **Reaching for computer use when a headless mode exists.** Slower, flakier, and
  unnecessary.
- **Not naming the output path.** The deliverable is produced but never found.
- **Treating a created file as a correct file.** Always open it.
- **Concurrent desktop use.** If this session is also operating the screen, both agents
  fight over focus and input; results become nondeterministic.
- **Assuming continuity.** A follow-up question is a separate dispatch unless you
  explicitly resume the previous thread.
