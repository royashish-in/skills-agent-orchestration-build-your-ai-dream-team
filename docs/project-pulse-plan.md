# Project Pulse implementation plan

## Summary

Build Mona's Project Pulse as a small static dashboard that helps contributors see project names, owners, status, recent activity, priority or risk, and a short summary at a glance. Follow the project brief and use the provided Designer and Coder agents under the Orchestrator's coordination. Serve the app from `app/` so the page can load its JSON data and the browser opens `index.html` rather than a directory listing.

## Ordered implementation steps

| Step | Owner and responsibility | File assignments | Dependencies |
| --- | --- | --- | --- |
| 1. Confirm scope and agree on the interface | Orchestrator reviews this plan and the Project Pulse brief, gives each specialist a bounded assignment, and makes sure the CSS hooks and data shape are clear. | No implementation files. | None; complete before delegation. |
| 2. Create visual styling and app data | Designer creates the polished, accessible, responsive dashboard styling. Coder creates deterministic sample project data using the agreed schema and configures the preview launch. | Designer: `app/styles.css`. Coder: `app/project-data.json`, `.vscode/launch.json`. | Step 1. These assignments can run in parallel: they touch separate files. Designer must provide `.dashboard` and `.project-card` hooks; the Coder's data must use a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority` for each project. Include a concise contributor-friendly `summary` as well. |
| 3. Build the dashboard page | Coder creates accessible semantic markup, loads the project JSON, and renders a visible card for each project using the agreed CSS hooks. | Coder: `app/index.html`. | Step 2; integrate the Designer's selectors and the Coder's JSON schema. |
| 4. Integrate and validate | Orchestrator checks the files together, verifies the launch configuration, runs validation, and reports any remaining limitations. Coder fixes implementation defects within the assigned files; Designer addresses styling defects in `app/styles.css`. | Existing assignments only: `app/index.html`, `app/styles.css`, `app/project-data.json`, `.vscode/launch.json`. | Steps 2 and 3. Fixes must follow file ownership; coordinate if a correction requires changing the interface between files. |

## Specialist responsibilities

- **Designer:** Own `app/styles.css`. Establish clear information hierarchy, legible spacing and typography, responsive behavior, accessible contrast and focus states, and polished project cards with status badges and clear priority treatment. Provide stable `.dashboard` and `.project-card` selectors for the page markup.
- **Coder:** Own `app/project-data.json`, `app/index.html`, and `.vscode/launch.json`. Supply representative, deterministic project records; implement the accessible page and render project cards from the JSON data; configure the named preview to serve the `app/` directory and open `index.html`.
- **Orchestrator:** Coordinate the handoffs and dependencies, keep file ownership explicit, integrate the independently produced CSS/data with the page, and validate the result before reporting the outcome.

## Dependencies and parallel work decisions

- `app/index.html` depends on the CSS hook names and the JSON schema. Complete it after Designer and Coder agree on those interfaces and finish the related CSS and data.
- Designer's `app/styles.css`, Coder's `app/project-data.json`, and Coder's `.vscode/launch.json` have separate file ownership and can be developed in parallel after scope is confirmed. The HTML integration is sequential after the CSS and data interfaces are available.
- Final end-to-end validation must happen after all four files are integrated. Any changes crossing ownership boundaries should be coordinated by the Orchestrator rather than made concurrently.

## File assignments

- `app/index.html` — Coder: accessible dashboard structure, exact page title `Project Pulse`, stylesheet and data references, and visible project cards with name, owner, status, recent activity, priority, and summary.
- `app/styles.css` — Designer: polished responsive layout, readable visual hierarchy, status and priority treatment, and `.dashboard` / `.project-card` styling including rounded corners and shadows.
- `app/project-data.json` — Coder: valid JSON with a top-level `projects` array; each record includes `name`, `owner`, `status`, `recentActivity`, and `priority`, plus a brief contributor-friendly `summary`. Use representative sample content because the brief does not provide a canonical project inventory.
- `.vscode/launch.json` — Coder: strict JSON launch configuration named `Run Project Pulse Dashboard`; run `python3 -m http.server 5500` from the `app/` working directory and use `serverReadyAction` to open `http://localhost:%s/index.html`.

## Validation expectations

1. Confirm all four assigned files exist and `.vscode/launch.json` is valid JSON with no comments.
2. Parse `app/project-data.json` as JSON; verify its top-level `projects` array and required fields on every project.
3. Inspect `app/index.html` to confirm the exact `Project Pulse` title, references to `styles.css` and `project-data.json`, and rendering of each record as a `.project-card` showing the required fields.
4. Inspect `app/styles.css` for `.dashboard`, `.project-card`, responsive layout, readable spacing, `border-radius`, and `box-shadow`; check keyboard focus and text/background contrast.
5. Run the `Run Project Pulse Dashboard` configuration. Confirm the server serves from `app/`, the browser opens `/index.html` rather than a directory listing, data loads, and the dashboard remains usable at narrow and wide viewport sizes. Stop the preview server after testing.
6. Run the repository's `scripts/validate-exercise.sh` and resolve any failures relevant to these outputs.

## Risks and open questions

- The brief does not provide real project records, so sample data should be clearly treated as representative rather than factual.
- The static page must be served over HTTP for browser JSON loading; opening `index.html` directly as a local file may prevent the data request from working.
- No framework or build tooling is required by the brief. Keep the implementation static and avoid adding dependencies unless a concrete need emerges.
