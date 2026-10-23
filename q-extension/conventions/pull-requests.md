# Pull Requests

Rules for authoring a pull request. Hold its prose to the writing rules (see: @lab43/q conventions/writing.md). A project's own PR conventions win over this doc (source: @lab43/q conventions/conventions.md, Three tiers of conventions).

## Body

A PR has two goals, in order: make the change easy for a human to review, and prove it does what it says. Every sentence in the body:

- Serves one of the two goals.
- Assumes no session context — the future reader arriving through git history has none, however much today's reviewer knows.

Whatever its shape, the body conveys:

- What the change delivers and why the project wants it, stated as outcomes the reader can now rely on, not as a catalog of edits.
- Answers to the questions the diff will raise: choices that look wrong but are deliberate, expected changes deliberately not made, and rules added to or changed in standing law that the PR's purpose doesn't explain.
- The known problems shipping with the change.
- The evidence it works that the diff doesn't carry: what was exercised and what it demonstrated. The project's standing checks prove nothing about this change and go unlisted.

Describe the change, never the run that produced it. The run's story is not the change's:

- review rounds
- rulings
- session events
- the plan the run followed

The body stands without the plan: a reviewer who never opens it still learns what the PR delivers and why.

## Templates

GitHub reads a PR template from `pull_request_template.md` in the repo root, `.github/`, or `docs/`, in any capitalization, or from the files of a `PULL_REQUEST_TEMPLATE/` directory in one of those places. When the repo has one, fill it in and add nothing the template lacks. Put each thing the body conveys in the field it fits (see: Body). When the repo has several, ask the user which one the PR uses. Without a template, the body takes whatever shape serves the two goals.

## Linking the work

Link the work's source: the tracker item when one exists (source: @lab43/q conventions/issue-tracking.md, Work links back), and the plan doc when the PR implements a plan. Where the tracker closes an item from the link's wording, word it to close only in the PR whose merge finishes the item — the last layer of a stack, the only PR otherwise. Every other layer links the item without closing it, or the first merge closes work the rest of the stack hasn't delivered.

## Screenshots and recordings

Show a change a person sees on screen as part of the evidence it works:

- a screenshot for a state
- a recording for a flow, an animation, or an interaction
- for a bug fix, the same steps before and after: first showing the bug, then not

Show only what the change touched. A change with nothing on screen to see carries none.
