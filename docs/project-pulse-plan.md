# Project Pulse implementation plan

## Summary

Build a lightweight, static Project Pulse dashboard for Mona's contributors. It should make projects, owners, status, recent activity, priority/risk, and short summaries easy to scan in a polished, responsive card layout. The solution must use only `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` (no framework or build system). The Orchestrator coordinates; the Planner identifies work and dependencies; the Designer owns UI/UX direction and styling; the Coder implements and validates the app and launch configuration.

## Ordered implementation phases

1. **Contract phase — establish shared ownership and data.**
   - The Orchestrator confirms the required top-level `projects` array and its fields: `name`, `owner`, `status`, `recentActivity`, and `priority`.
   - Agree on the shared `.dashboard` and `.project-card` hooks, card content structure, and a small consistent status/priority vocabulary.

2. **Design phase — produce Project Pulse visual direction.**
   - Designer works from the brief and creates the responsive visual system in `app/styles.css`.
   - Define card hierarchy, status badges, priority treatment, readable spacing, contrast, focus states, and narrow-screen behavior.
   - This phase can begin once the markup hooks and field contract are agreed.

3. **Data and markup phase — create the dashboard files.**
   - Coder creates `app/project-data.json` with a top-level `projects` array and representative valid records.
   - Coder creates `app/index.html` with the exact title `Project Pulse`, stylesheet and JSON references, project-card markup, and visible status, `recentActivity`, and priority values.
   - The Coder should use an inline script or other small static technique for rendering without adding extra application files.

4. **Launch phase — make the dashboard directly runnable.**
   - Coder creates strict-JSON `.vscode/launch.json` after `app/index.html` exists.
   - Name the configuration `Run Project Pulse Dashboard`, set `cwd` to `${workspaceFolder}/app`, run `python3 -m http.server 5500`, and open `http://localhost:%s/index.html`.

5. **Integration phase — verify the complete dashboard.**
   - Orchestrator reviews markup, CSS, data, and launch behavior together.
   - Resolve cross-file mismatches, then complete browser and code validation.

## File assignments

- `app/index.html` — **Coder owns.** Semantic structure, exact title, CSS/JSON references, project-card rendering, loading/empty/error handling, and accessible labels.
- `app/styles.css` — **Designer owns.** `.dashboard`, `.project-card`, border-radius, box-shadow, typography, spacing, responsive layout, status/priority styling, contrast, and focus states.
- `app/project-data.json` — **Coder owns.** Valid JSON with top-level `"projects"` and required fields for every project.
- `.vscode/launch.json` — **Coder owns.** Strict JSON configuration for port 5500, working directory `app/`, and direct opening of `index.html`.

## Designer responsibilities

- Guide information hierarchy and visual clarity.
- Provide polished cards, status badges, readable priority treatment, and responsive behavior.
- Ensure keyboard usability, semantic support, text/label alternatives to color, sufficient contrast, and readable overflow behavior.
- Report design decisions, files touched, tradeoffs, and validation recommendations.

## Coder responsibilities

- Implement deterministic, accessible markup and rendering.
- Keep the JSON schema exact and the JavaScript/control flow simple.
- Create only the assigned support configuration.
- Parse and validate JSON, make launch behavior explicit, report changed files and remaining risk.
- Do not stage, commit, or push changes.

## Dependencies

- HTML rendering depends on the exact JSON key and field names.
- CSS styling depends on the agreed `.dashboard` and `.project-card` hooks.
- Launch configuration depends on `app/index.html` and the `app/` directory.
- Integration review depends on all four files existing.

## Parallel work decisions

- **Parallel:** After the contract is agreed, Designer can create `app/styles.css` while Coder creates `app/index.html` and `app/project-data.json`, because file scopes do not overlap.
- **Parallel:** Data population and styling may proceed independently once field names and card hooks are fixed.
- **Sequential:** Integration and launch validation must follow completion of the app files.
- **Sequential:** Do not change the HTML markup contract or JSON field names without communicating the change to the Designer and Orchestrator.

## Sequential constraints

1. Agree on data fields and CSS hooks.
2. Create project data and markup contract.
3. Complete HTML and CSS.
4. Create `.vscode/launch.json`.
5. Run integrated validation and browser preview.

## Edge cases

- Empty or malformed project data must not produce an invisible or broken dashboard; provide a useful empty/error state.
- Opening `index.html` through `file://` may not support JSON fetch; normal use is through the launch server.
- Missing fields, unknown status/priority values, long names, summaries, or activity text must remain readable and must not break card layouts.
- The UI must remain useful on narrow screens and without hover or color-only cues.
- Port 5500 may be occupied; the launch should make the failure visible rather than opening an unrelated page.
- Launch JSON must contain no comments or trailing commas.

## Validation expectations

- `python3 -m json.tool app/project-data.json` succeeds and confirms top-level `"projects"`; each project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `python3 -m json.tool .vscode/launch.json` succeeds and confirms strict JSON.
- HTML contains `Project Pulse`, references `styles.css` and `project-data.json`, and uses `project-card` rendering for status, `recentActivity`, and priority.
- CSS contains `.dashboard`, `.project-card`, `border-radius`, `box-shadow`, and responsive rules.
- Launch configuration is named `Run Project Pulse Dashboard`, uses `${workspaceFolder}/app`, serves port 5500, and opens `/index.html`, not a directory listing.
- Manual browser review confirms visible cards, readable desktop/mobile layouts, keyboard/focus visibility, and good contrast.

## Open questions

- Which representative project names and status/priority labels should seed the initial dataset? Use a small illustrative set if no approved team data is provided.
- Does the local environment support the chosen VS Code launch configuration type and external browser action? Adjust the launcher only while preserving `app/` as `cwd` and direct `/index.html` opening.
- Should a concise `summary` field be included in addition to the five brief fields? It is useful for contributor context, but an approved field contract should decide whether it is mandatory.
