# Playbook: post-deploy UI acceptance

Use after a release when the question is "does the interface actually behave as expected".

## 1. Turn expectations into a check list

Before dispatching, write the expectations as concrete, observable statements. Vague
goals produce unusable results.

| Bad | Good |
|---|---|
| "check the checkout page" | "submitting the form with an empty email shows an inline error under the email field" |
| "make sure the redesign looks fine" | "at 1440px the sidebar is visible; at 390px it collapses behind the menu button" |

Aim for 3–8 checks. More than that in one dispatch degrades reliability — split into two
dispatches rather than one long one.

## 2. Decide the coverage dimensions

State these explicitly in the prompt, or the run will silently pick one:

- **Viewports** — at minimum one desktop and one mobile width (e.g. 1440×900, 390×844)
- **Auth state** — logged out vs logged in, if the page differs
- **Data state** — empty, populated, error, loading; pick the ones the release touched
- **Language/locale** — if the product is localised

## 3. Dispatch

Pass the expectation list verbatim, plus the contract block from `contract.md`. Example
prompt skeleton:

```
Open <URL> in Chrome.

Check each of the following and record a verdict for each:
1. <expectation>
2. <expectation>
3. <expectation>

For every check, take a screenshot at the moment that demonstrates the result and save it
to <RUN_DIR>/shots/. For failures, also capture the failing element's selector and the
browser console errors, if any. Use a 1440x900 viewport unless a check says otherwise.

<contract block>
```

## 4. Review

For each `fail`:

1. Open its screenshot. Confirm the screenshot shows what the text claims — a mismatch
   between claim and image is itself a finding about the run, not the product.
2. Check whether the failure is a **product defect** or an **environment problem**. A blank
   page from a login wall, an untrusted certificate, or a downed service is not a UI bug.
   These belong in `blockers[]`.
3. Only then decide the fix.

## 5. Reporting

Lead with the overall verdict, then failures ordered by severity, each with its screenshot.
Passing checks need one line each, not a screenshot. If a check was `unclear`, say why it
could not be settled rather than presenting it as a pass.

## Common pitfalls

- **Accepting a text claim without opening the image.** The most expensive mistake — it
  turns a verification step into an unverified assertion.
- **Letting the executor choose its own viewport.** It defaults to one, and responsive
  defects hide behind that default.
- **Bundling unrelated pages into one dispatch.** A failure half-way leaves the rest
  unevaluated, and the run is not resumable.
- **Re-dispatching for re-verification without a new run directory.** Evidence from the
  fix and evidence from the original get mixed together.
