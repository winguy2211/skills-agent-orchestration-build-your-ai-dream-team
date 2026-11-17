# Project Pulse implementation plan

## Goal

Build a lightweight static dashboard that helps Mona's contributors see which
projects are active, who owns them, their status, recent activity, and priority
or risk. The first view should be a polished, accessible, responsive dashboard
with readable project cards and status badges.

## Roles

- **Orchestrator:** Owns the phases and file boundaries, gives specialists the
  shared requirements below, coordinates integration, and checks the finished
  dashboard against the validation expectations.
- **Designer:** Defines the dashboard's information hierarchy, accessible
  markup guidance, responsive behavior, and visual direction. Implements the
  presentation in `app/styles.css`, including `.dashboard` and
  `.project-card`, clear status and priority treatments, rounded corners, and
  shadows.
- **Coder:** Implements the data-driven page in `app/index.html`, the project
  records in `app/project-data.json`, and the runnable preview in
  `.vscode/launch.json`. Coordinates the HTML structure and CSS hooks with the
  Designer and keeps launch settings deterministic.

## Shared implementation contract

- `app/project-data.json` has a top-level `projects` array. Every project
  includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `app/index.html` has the title **Project Pulse**, references `styles.css`,
  loads `project-data.json`, and renders a visible `.project-card` for each
  project. Cards show status, recent activity, and priority.
- The dashboard uses semantic, accessible markup and a responsive layout.
- `.vscode/launch.json` is strict JSON with a **Run Project Pulse Dashboard**
  configuration. It serves from `${workspaceFolder}/app` with
  `python3 -m http.server 5500` and opens
  `http://localhost:%s/index.html`, not a directory listing.

## Phases and file assignments

1. **Agree on the contract.** The Orchestrator shares the data schema, required
   page content, CSS hooks, and launch behavior above with both specialists.
   This is a prerequisite for implementation, not a coding task.
2. **Implement in parallel after the contract is agreed.**
   - **Designer:** Create `app/styles.css`. Use `.dashboard` and
     `.project-card` as the shared hooks; style status and priority clearly
     and ensure the layout adapts to narrow screens.
   - **Coder:** Create `app/project-data.json` with multiple representative
     projects, implement `app/index.html` to render those records with
     accessible project cards, and create `.vscode/launch.json` with the
     specified launch configuration.
3. **Integrate and validate.** The Orchestrator checks that the page structure,
   Designer's selectors, project data, and launch configuration work together.
   Resolve any mismatches within the assigned file boundaries.

## Dependencies and parallel work decisions

- The shared contract must be settled before implementation so the Designer
  and Coder use the same CSS hooks and project fields.
- `app/index.html` depends on the `projects` schema in
  `app/project-data.json` and the selectors agreed with the Designer.
- `app/styles.css` depends on the agreed page/card hooks, but not on the
  project's final record values.
- `.vscode/launch.json` depends on the known `app/` path and `index.html`
  entry point, not on the page styling or project content.
- Once the contract is settled, the Designer can work on `app/styles.css`
  while the Coder works on the HTML, JSON data, and launch configuration:
  these file scopes do not overlap. Final integration must happen after both
  streams finish.

## Validation expectations

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and
  `.vscode/launch.json` exist and are assigned to the intended specialists.
- Parse `app/project-data.json` as JSON; confirm the top-level `projects`
  array is non-empty and every record has all five required fields.
- Check that `app/index.html` has the exact **Project Pulse** title, references
  the stylesheet and JSON data, and renders the required project fields as
  visible cards using `.project-card`.
- Check that `app/styles.css` contains `.dashboard` and `.project-card`,
  provides readable status and priority treatments, rounded card styling and
  shadows, and responds to narrower viewports.
- Parse `.vscode/launch.json` as strict JSON. Confirm the configuration name,
  working directory, server command, and browser URL match the contract.
- Run **Run Project Pulse Dashboard** and verify that the browser opens
  `index.html` and displays the dashboard rather than a directory listing.
