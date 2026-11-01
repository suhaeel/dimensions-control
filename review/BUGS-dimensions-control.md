# Bug review: dimensions-control

Repo: EssentialBlocks/dimensions-control (public). Fork: suhaeel/dimensions-control
Branch: `review/dimensions-control-bugs` (no changes to code, main untouched)
What it is: the first, class-based version of the 4-side (top/right/bottom/left) input (`index.js` 99 lines + `style.scss`).

## Findings
| # | Severity | Where | What | Evidence |
|---|---|---|---|---|
| 1 | Medium | index.js:6-12 | Values are copied from props into state once (`state = { top: this.props.top || 0, ... }`) and never updated. If the block's attributes change from outside (undo/redo, switching the selected block, reset), the inputs keep showing the old numbers. | From reading the code, not run |
| 2 | Low | index.js:21,27 | `parseInt(value)` drops decimals, so `1.5` becomes `1`. Fine for px, wrong if this is ever used for em or %. | From reading the code, not run |
| 3 | Low | index.js:78 | The link button has no `type="button"`, so it submits a surrounding form. | From reading the code, not run |
| 5 | Low | index.js:21,27 | This version writes integers. If it is used with attributes declared `type: "string"` (as `helpers` `generateDimensionsAttributes` does), Gutenberg drops the number when the post is loaded. | **Verified (ran it)** for the type rule: a number saved into a `string` attribute reads back `undefined` in the editor. Whether a plugin pairs them is From reading the code, not run. |
| 4 | Low | index.js:49 etc. | `<label>` is not tied to its `<input>` (no `htmlFor`/`id`), and "dimention" is misspelled in class names (`dimention-container`). Renaming would break CSS in plugins that use these classes, so leave as is. | From reading the code, not run |

## Not covered
- Read the code only, unless a finding says "Verified". No automated tests exist in this repo.
- Chrome-only and WordPress 7.1.3 on Local where something was run.
- This repo is not a plugin on its own. It is a 2021 component that other plugins import, and the current `controls` repo (private, open PRs #204, #214, #215, #218) has replaced it for the standalone plugins, so check there first whether the same code still exists before spending time here.
