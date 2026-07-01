# Claude Web Dev Kit

A Claude Code plugin marketplace for web development, containing three plugins:

- **core** — a shared skills and agents used as a base for the other plugins.
- **web-frontend** — frontend skills and agents. Depends on `core`.
- **web-backend** — backend skills and agents. Depends on `core`.

## Layout

```
claude-web-dev-kit/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   ├── core/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/code-review/SKILL.md
│   │   └── agents/code-reviewer.md
│   ├── web-frontend/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/component-scaffolder/SKILL.md
│   │   ├── skills/accessibility-review/SKILL.md
│   │   └── agents/frontend-reviewer.md
│   └── web-backend/
│       ├── .claude-plugin/plugin.json
│       ├── skills/api-scaffolder/SKILL.md
│       ├── skills/db-schema-review/SKILL.md
│       └── agents/backend-reviewer.md
├── README.md
└── LICENSE
```

## Installing

Add this repo as a marketplace, then install whichever plugin(s) you need:

```
/plugin marketplace add <path-or-url-to-this-repo>
/plugin install web-frontend@claude-web-dev-kit
/plugin install web-backend@claude-web-dev-kit
```

Installing `web-frontend` or `web-backend` automatically pulls in `core` via each plugin's `dependencies` field.

To test changes locally without installing, point Claude Code at this directory directly with `--plugin-dir`:

```
claude --plugin-dir ./plugins/core
claude --plugin-dir ./plugins/web-frontend
claude --plugin-dir ./plugins/web-backend
```

After editing any hooks, agents, or `.mcp.json` in a running session, run `/reload-plugins` to pick up the changes.

## Versioning

None of the `plugin.json` files pin a `version` field yet — while iterating, the marketplace resolves each plugin's version from the current git commit SHA. Add a `version` field once the plugins stabilize.

## Extending

- Add new skills under `plugins/<plugin-name>/skills/<skill-name>/SKILL.md`.
- Add new agents under `plugins/<plugin-name>/agents/<agent-name>.md`.
- Add a new plugin by creating `plugins/<name>/` with its own `.claude-plugin/plugin.json`, and registering it in the root `.claude-plugin/marketplace.json`.

## License

MIT — see [LICENSE](./LICENSE).
