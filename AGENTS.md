# Repository Guidelines

## Project Structure & Module Organization
- All simulation source resides in `code/`; modules mirror agent types (`firm.py`, `consumer.py`, `bank.py`, `centralBankUnion.py`, etc.).
- `timing.py` orchestrates the simulation loop and should be treated as the entry point.
- `parameter.py` holds scenario parameters; adjust copies rather than editing defaults when possible.
- Runtime output is written under `code/data/<scenario-name>/` as defined by `Parameter.folder`; the directory is created automatically; confirm it stays untracked with `git status` before committing.
- Legacy docs live in `code/READMY.txt`; update this guide instead for contributor-facing changes.

## Build, Test, and Development Commands
- `cd code && python2 timing.py` runs the default simulation; use the system's Python 2.7 to respect `print` syntax.
- `cd code && python2 -m compileall .` validates bytecode generation before packaging or distribution.
- `cd code && python2 printParameters.py` echoes the active configuration for quick sanity checks.
- When adapting scenarios, duplicate `parameter.py` and import the alternate module via `PYTHONPATH=. python2 -c "import timing"`.

## Coding Style & Naming Conventions
- Follow PEP 8 defaults with 4-space indents; existing modules mix camelCase attributes, so match surrounding code.
- Keep module names lowercase (e.g., `newpolicy.py`) and class names in CamelCase.
- Preserve Python 2 idioms (`print` statements, integer division expectations) unless porting the entire stack.
- Document non-obvious simulation steps with concise comments placed above the block they clarify.

## Testing Guidelines
- There is no automated test suite; verify changes by running shortened simulations (set `parameter.ncycle` to a small integer).
- Add targeted assertions in staging branches (e.g., checking `aggregator` outputs) and remove or guard them before merging.
- Capture CSV snapshots from `code/data/<scenario-name>/` and compare against baselines when altering financial logic.
- Share observed KPI deltas in the PR description to provide regression evidence.

## Documentation & Reporting Workflow
- Quarto will be the standard publishing toolset; track setup work in issue #3 and land changes through the backlog.
- Place source notebooks and markdown under a forthcoming `docs/` Quarto project and render with `quarto render` before sharing results.
- Keep generated HTML/PDF artifacts out of git unless publishing via Pages; attach key exports to issues or releases instead.

## Issue Backlog Workflow
- Treat the backlog as the single source of truth: open or update an issue before starting any work.
- Pull active work from the open backlog list and keep each branch/PR tied to exactly one issue.
- File backlog items using the `Backlog Item` issue template (GitHub issues → New issue).
- Use the `backlog` label plus area tags (`area:bank`, `area:policy`) to group work.
- Link issues in branches/PRs (`git checkout -b feature/123-short-title`) and commits (`Refs #123`).
- Update checkboxes in the issue as tasks land and attach KPI snapshots or logs from `code/data/<scenario>/`.
- Close the issue only after the Definition of Done checklist is complete.

## Commit & Pull Request Guidelines
- No Git history ships with this snapshot; adopt `type: short summary` messages (e.g., `fix: stabilize credit network loop`).
- Reference related issues in the first line, and describe parameter changes or output impacts in the body.
- PRs should summarize scenario settings tested, attach runtime duration, and note any generated artifacts kept out of version control and attach `git status` output if cleanup was manual.
- Request review from a domain lead before merging structural or policy modules.
