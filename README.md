# first-plugin

Support kit for pre-PR checks: identify and summarize branch changes, and review code quality.

A small Claude Code plugin with two read-only components you run before opening a pull request. Neither one edits your files.

## What it adds

| Component | Type | What it does |
|-----------|------|--------------|
| `/first-plugin:summarize-changes` | Slash command | Lists every file touched on the current branch with a one-line description of what changed. The output is short enough to paste into a pull-request description. |
| `code-reviewer` | Subagent | Reviews recent changes for bugs, missing error handling and unclear names. Returns a short list grouped by severity (high, medium, low), naming the file and the fix for each item. Uses `Read`, `Grep`, `Glob` and `Bash` (read-only git commands to find the diff). |

## Structure

```
.
├── .claude-plugin/
│   └── plugin.json            # manifest: name, version, description, author
├── commands/
│   └── summarize-changes.md
├── agents/
│   └── code-reviewer.md
└── README.md
```

## Usage

Load the plugin from the repo root:

```bash
claude --plugin-dir .
```

Then, inside the session:

- **Summarize your branch:** run `/first-plugin:summarize-changes`.
- **Review your changes:** ask Claude something like *"review my recent changes"*. Claude delegates to the `code-reviewer` subagent, which you can confirm with `/agents`.

After editing any component, run `/reload-plugins` to pick up the changes without restarting.

## Validation

The **Validate plugin** GitHub Actions check runs on every push and confirms the structure. You can run the same check locally:

```bash
node .github/scripts/validate-plugin.js
claude plugin validate .
```
