# Project Pulse final handoff

Project Pulse is a static, card-based dashboard for Mona's team. It shows each project's owner, status, recent activity, priority, and a short summary. A team of custom agents built it, orchestrated from GitHub Copilot CLI in a Codespace.

## Agent team

| Agent | Role in this build |
| --- | --- |
| Orchestrator | Split the plan into phases, gave each specialist its own files, integrated the results, and ran validation. |
| Planner | Wrote [docs/project-pulse-plan.md](project-pulse-plan.md): the data and class contracts, file ownership, dependencies, parallel work decisions, and validation expectations. |
| Designer | Owned `app/styles.css`. Made the visual and accessibility decisions. |
| Coder | Owned `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. |

The Designer and Coder worked in parallel because they never edited the same file. The shared class contract in the plan kept their work aligned.

## Deliverables

| File | Contents |
| --- | --- |
| `app/index.html` | Page titled "Project Pulse". Links `styles.css`, loads `project-data.json` with `fetch`, and renders one `project-card` article per project. Each card shows the name, owner, summary, `status`, `priority`, and `recentActivity`. Uses `createElement` and `textContent`, not `innerHTML`. Shows loading, empty, and error messages, including a hint when the page is opened via `file://`. |
| `app/styles.css` | Includes the `.dashboard` and `.project-card` selectors. Cards use `border-radius` and `box-shadow`. The grid has 1 column on mobile, 2 at 640px, and 3 at 980px. Badge colours are meant to meet AA contrast, and badges show text, so meaning doesn't depend on colour. Includes visible `:focus-visible` styles, reduced-motion support, and wrapping for long text. |
| `app/project-data.json` | A top-level `projects` key holding 5 projects. Each has `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`. Together they cover every status (Active, At Risk, Blocked, Completed) and every priority (High, Medium, Low). |
| `.vscode/launch.json` | Strict JSON, with one configuration named "Run Project Pulse Dashboard". |

## Launch configuration

`.vscode/launch.json` defines **Run Project Pulse Dashboard**:

- `type`: `node-terminal`, so it doesn't need the Python extension.
- `command`: `python3 -m http.server 5500`
- `cwd`: `${workspaceFolder}/app`, so the server serves the dashboard rather than the repository root.
- `serverReadyAction` opens `http://localhost:%s/index.html`, so the browser lands on the dashboard rather than a directory listing.

## validation results

All checks were run against commit `d95c6ce` with a clean working tree. **All passed.**

| Check | Result |
| --- | --- |
| `python3 -m json.tool` on `app/project-data.json` and `.vscode/launch.json` | Pass. Both are valid strict JSON. |
| Data contract: top-level `projects` key, 5 items, all 5 required fields are non-empty strings, every status and priority used | Pass |
| `app/index.html`: exact `<title>` and `<h1>` "Project Pulse", links `styles.css`, fetches `project-data.json`, uses `project-card`, includes `status` / `recentActivity` / `priority`, `lang="en"`, viewport meta, no `innerHTML` | Pass |
| Inline script syntax (`node --check`) | Pass |
| `app/styles.css`: `.dashboard`, `.project-card`, `border-radius`, `box-shadow`, `@media`, grid columns, `:focus-visible`, `prefers-reduced-motion` | Pass |
| `.vscode/launch.json`: name, type, command, `cwd`, `uriFormat` | Pass |
| `serverReadyAction` pattern matches real `http.server` output and captures port 5500 | Pass |
| HTTP smoke test on temporary port 5599: `index.html`, `styles.css`, and `project-data.json` all return 200, the page contains "Project Pulse", and there is no directory listing | Pass |
| Editor diagnostics for all four files | No errors |

### Not verified automatically

- The dashboard has not been checked in a browser: card layout at each breakpoint, keyboard focus, and badge contrast. Badge contrast was the Designer's goal but was not measured with a tool.
- The plan's Phase 2 Designer review of the `app/index.html` markup didn't happen as a separate step.

## handoff notes

- **To run:** open Run and Debug, choose **Run Project Pulse Dashboard**, and press play.
- **Port 5500 is already in use:** a preview server was still running on port 5500 when this was validated. If you start the launch configuration while that server is running, it exits with "Address already in use". Stop the old server first. Don't change the port, because the exercise requires 5500.
- **Don't open `index.html` directly from disk:** browsers block `fetch` over `file://`, so the page shows a message asking you to use the launch configuration instead.
- **To change the data:** edit `app/project-data.json` and keep the five required fields. A status or priority outside the documented values still shows up, with a neutral badge.
- **File ownership for future work:** visual changes belong in `app/styles.css`; markup, data, and launch changes go in the Coder-owned files. Add any new class names to the contract in [docs/project-pulse-plan.md](project-pulse-plan.md) before changing markup or CSS.
- **Git:** no agent staged, committed, or pushed anything during this step. You control all git operations.
