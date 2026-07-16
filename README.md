<div align="center">

# FN2 skill for OpenClaw 🦞

**Grounded market research and schedulable research agents — right inside [OpenClaw](https://openclaw.ai).**

[![CI](https://github.com/fn2ai/fn2-openclaw-skill/actions/workflows/ci.yml/badge.svg)](https://github.com/fn2ai/fn2-openclaw-skill/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/agentskills.io-compatible-7c3aed.svg)](https://agentskills.io)

</div>

Ask your OpenClaw agent *"how did NVDA do this week and why?"* or *"set up a daily
macro brief"* — and it answers with [FN2](https://fn2.ai)'s grounded, sourced
research, or spins up an agent that does it on a schedule.

- 🔎 **Research** stocks, markets, earnings, and the economy — answers cite live
  prices, transcripts, filings, and economic data.
- 🤖 **Agents** that run on a schedule (daily brief, weekly recap, earnings
  monitor) and report back.
- 🪶 **Zero dependencies.** One small Python-3-stdlib CLI. No `pip install`.
- 🔓 **No secrets in the repo.** Auth is a `FN2_API_KEY` you provide.

---

## Install

**From ClawHub:**

```bash
openclaw skills install @fn2/fn2
```

**Or from this repo** — copy the skill into your OpenClaw workspace:

```bash
git clone https://github.com/fn2ai/fn2-openclaw-skill.git
mkdir -p ~/.openclaw/workspace/skills/fn2
cp -r fn2-openclaw-skill/{SKILL.md,scripts,references} ~/.openclaw/workspace/skills/fn2/
```

OpenClaw discovers the skill by its `name`/`description` and loads it on demand.
Either path works — the ClawHub scope `@fn2/fn2` installs under the `fn2/` group,
the manual copy uses the bare `fn2/` folder; the `name: fn2` in `SKILL.md` is what
identifies the skill.

## Get an API key

New to FN2? Create a free account and API key here — it takes a minute:

### → **[fn2.ai/api-keys?ref=openclaw](https://fn2.ai/api-keys?ref=openclaw)**

(Give the key the `chat`, `agents`, and `models` scopes.) Then export it so the
skill can use it:

```bash
export FN2_API_KEY=fn2_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

If you don't have a key set, the skill points you to that link automatically.

> 🔒 Treat this key like a password. Never commit it or paste it into a chat.
> You can revoke it any time from the same page.

## Use it

Just talk to your agent:

> **You:** How did the semiconductor stocks do today, and what moved them?
>
> **You:** Set up an agent that gives me a market brief every weekday at 8am ET.
>
> **You:** What did Apple's CFO say about guidance on the last call?

OpenClaw loads the skill and runs the bundled CLI with its `exec` tool.

### Or run the CLI directly

```bash
fn2=~/.openclaw/workspace/skills/fn2/scripts/fn2.py

python3 "$fn2" research "What's the macro setup going into the next Fed meeting?"
python3 "$fn2" agents create --name "Macro Brief" \
     --prompt "Morning macro brief: overnight moves, key data, what to watch" \
     --every weekdays --timezone America/New_York
python3 "$fn2" agents list
python3 "$fn2" models
```

Add `--json` to any command for machine-readable output. See
[`references/api.md`](references/api.md) for the full command and endpoint
reference.

## What's in here

```
SKILL.md            # the skill (agentskills.io standard)
scripts/fn2.py      # the CLI (Python 3 stdlib, no deps)
references/api.md   # full command + API reference
tests/              # offline unit tests (mocked HTTP)
```

## Develop & test

The CLI is plain Python 3 (3.8+). Tests are fully offline — no key needed:

```bash
python3 -m unittest discover -s tests
```

CI runs the test suite and an install-smoke check on every push. Contributions
welcome — open an issue or PR.

### Publish to ClawHub (maintainers)

Publishing uses the `clawhub` CLI (distinct from the `openclaw` runtime that
*installs* skills):

```bash
clawhub publisher create fn2 --display-name "FN2" # one-time setup
clawhub skill publish . --owner fn2 --slug fn2 \
  --name "FN2 skill for OpenClaw 🦞" --dry-run
clawhub skill publish . --owner fn2 --slug fn2 \
  --name "FN2 skill for OpenClaw 🦞"
```

The owner and slug produce the canonical install reference
`openclaw skills install @fn2/fn2`. Do not publish without `--owner fn2`, or the
skill will be released under the authenticated user's personal publisher.

## Compatibility

`SKILL.md` follows the open **[agentskills.io](https://agentskills.io)** standard,
so this skill also works with Hermes Agent, Claude Code, Codex CLI, OpenCode, and
other compatible agents. A Hermes-packaged version lives at
[fn2-hermes-skill](https://github.com/fn2ai/fn2-hermes-skill).

## License

[MIT](LICENSE) © FN2. Not affiliated with OpenClaw or ClawHub.
