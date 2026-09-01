# AI Orchestrator

A human-in-the-loop CLI workflow for coordinating Antigravity, Codex, and Claude Code.

> Antigravity plans. Codex implements. Claude reviews. You approve every critical transition.

## Why?

Running multiple coding agents against the same project can produce conflicting edits, duplicated work, and unreviewed changes. AI Orchestrator assigns one role to each agent and permits only one writing agent at a time.

The workflow is:

```text
User
  |
  v
Antigravity -- plan (read-only)
  |
  v human approval
Codex -- implement (workspace-write)
  |
  v human approval
Claude -- independent review (no Edit tool)
  |
  v
Codex -- validate findings (read-only)
  |
  v human approval
Codex -- fix confirmed findings and test
```

## Safety model

- Human approval between critical phases
- One writing agent at a time
- Read-only planning, review, and finding validation
- Workspace-scoped implementation
- No automatic `git push`, merge, commit, or deployment
- Auditable reports under `.ai-runs/`
- Cooperative stop command that preserves generated files

This reduces risk; it does not make agent-generated changes inherently safe. Always inspect the diff and test before committing or deploying.

## Requirements

Install and authenticate these CLIs separately:

- `agy` - Antigravity CLI
- `codex` - Codex CLI
- `claude` - Claude Code

The host also needs Bash, Git, and `setsid` (normally provided by `util-linux` on Linux).

## Installation

Clone the repository and install the executable in your user-local binary directory:

```bash
git clone https://github.com/orleanss777-sys/ai-orchestrator.git
cd ai-orchestrator
install -Dm755 bin/ai-orchestrator "$HOME/.local/bin/ai-orchestrator"
```

Ensure `$HOME/.local/bin` is in your `PATH`, or invoke the executable using its complete path.

## Usage

Check the environment:

```bash
ai-orchestrator status
```

Enable new runs:

```bash
ai-orchestrator on
```

Enter a project and start a task:

```bash
cd ~/projects/personal-finance

ai-orchestrator run \
  "Add monthly budgets by category and warn at 80 percent of the limit"
```

The executable pauses for explicit approval before implementation, review, and correction.

Disable new phases and attempt to stop the currently active phase:

```bash
ai-orchestrator off
```

Other commands:

```bash
ai-orchestrator toggle
ai-orchestrator status
ai-orchestrator help
```

## Financial application example

For a personal-finance project:

```bash
cd ~/projects/personal-finance
ai-orchestrator on
ai-orchestrator run "Add income and expense registration with a monthly balance"
```

The prompts instruct agents to prioritize monetary precision, validation, privacy, idempotency, duplicate prevention, and data-loss risks. Do not use real financial or banking data while evaluating generated software.

## Execution artifacts

Each run creates a timestamped directory inside the target project:

```text
.ai-runs/YYYYMMDD-HHMMSS/
├── plano.md
├── implementacao.md
├── revisao.md
├── triagem.md
├── resultado-final.md
└── *.log
```

Add `.ai-runs/` to the target project's `.gitignore` if the reports should not be committed.

## Manual

The complete Portuguese manual is available at [docs/manual-pt-BR.pdf](docs/manual-pt-BR.pdf).

## Limitations

- CLI flags and behavior can change between provider versions.
- The stop command sends a termination signal but cannot guarantee immediate shutdown of every descendant or remote process.
- The workflow does not replace code review, backups, tests, or organizational security controls.
- The current implementation targets Linux environments with Bash and `setsid`.

## Security

Read [SECURITY.md](SECURITY.md) before using the tool with sensitive repositories. Do not place credentials, production data, or secrets in prompts or `.ai-runs` reports.

## License

MIT. See [LICENSE](LICENSE).

## Disclaimer

This is an independent community project. It is not affiliated with, endorsed by, or sponsored by Google, OpenAI, Anthropic, Antigravity, Codex, or Claude. Product names and trademarks belong to their respective owners.
