# CLAUDE.md

This repo is a Claude Code **plugin marketplace**, not an application. There is no build/test/lint step — the "code" is markdown (`SKILL.md`, agent `.md` files) and plugin manifests (`plugin.json`). Follow the conventions below rather than typical app-repo conventions.

## Structure

```
claude-web-dev-kit/
├── .claude-plugin/marketplace.json   # the catalog — lists all plugins
└── plugins/<plugin-name>/
    ├── .claude-plugin/plugin.json    # ONLY the manifest lives here
    ├── skills/<skill-name>/SKILL.md  # skills live at plugin ROOT, never under .claude-plugin/
    └── agents/<agent-name>.md        # agents live at plugin ROOT, never under .claude-plugin/
```

Three plugins exist: `core` (shared code-review skill/agent), `web-frontend` (depends on `core`), `web-backend` (depends on `core`). Cross-plugin deps use `"dependencies": ["core"]` in the dependent's `plugin.json` — installing `web-frontend` or `web-backend` auto-installs `core`.

## plugin.json conventions

- No `version` field yet, intentionally — while iterating, the marketplace resolves version from the git commit SHA. Add `version` (semver) once a plugin's shape stabilizes. `claude plugin validate --strict` will flag its absence as an error; that's expected and not a bug to fix.
- Required fields: `name` (kebab-case), `displayName`, `description`, `author` (`name`/`email`), `license: "MIT"`, `keywords`.
- Never add `hooks`, `mcpServers`, or `permissionMode` to a plugin-shipped agent — unsupported for plugin agents.

## Writing SKILL.md / agent .md files

- `SKILL.md` frontmatter: `name` + `description`. The description must state both *what* the skill does and *when* Claude should invoke it (this is how Claude decides relevance — vague descriptions mean the skill won't get picked up).
- Agent frontmatter: `name`, `description` (when to invoke), `model: sonnet`.
- Keep instructions concrete and stack-agnostic where possible (detect the project's actual conventions rather than imposing new ones) — see existing skills for the pattern.

## Third-party skills

Skills imported from other repos (e.g. `web-frontend/skills/react-best-practices`, from `davila7/claude-code-templates`) are kept **unmodified** and attributed in the root `README.md`'s "Third-party skills" section, with license and original author noted. Don't edit imported skill content in place — pull updates from upstream instead.

## Commit convention

Use [Conventional Commits](https://www.conventionalcommits.org/) tags, optionally scoped, e.g. `feat(web-frontend): add react-best-practices skill`:

- `feat` — a new skill, agent, or plugin
- `fix` — correcting a mistake in existing skill/agent/manifest content
- `docs` — README, CLAUDE.md, or other documentation-only changes
- `refactor` — restructuring skill/plugin content with no behavior change
- `chore` — manifest/tooling upkeep (e.g. `.gitignore`, dependency bumps) not covered above
- `revert` — reverting a previous commit

This repo has no build/test/lint pipeline, so `build`, `ci`, `test`, `style`, and `perf` rarely apply — use them only if that specific kind of change genuinely occurs (e.g. adding a GitHub Actions workflow would be `ci`).

One logical change per commit — e.g. a new skill (`feat`) and its README documentation (`docs`) are separate commits, not bundled.

## Validating and testing locally

```
claude plugin validate .                    # validate the marketplace
claude plugin validate ./plugins/<name>     # validate one plugin
claude --plugin-dir ./plugins/<name>        # load a plugin for a session without installing it
```

After editing any agent, hook, or `.mcp.json` in a running session, run `/reload-plugins` to pick up the change without restarting.
