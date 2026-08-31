# Yandex Tracker agent skill

[![ClawHub — Install](https://img.shields.io/badge/ClawHub-Install-111827)](https://clawhub.ai/kandler3/skills/yandex-tracker)
[![skills.sh — Install](https://img.shields.io/badge/skills.sh-Install-000000)](https://www.skills.sh/kandler3/yandex-tracker-skill/yandex-tracker)
[![Claude Code — Package](https://img.shields.io/badge/Claude_Code-Package-D97757)](https://github.com/Kandler3/yandex-tracker-skill/releases/tag/v1.1.0)
[![Gemini CLI — Package](https://img.shields.io/badge/Gemini_CLI-Package-4285F4)](https://github.com/Kandler3/yandex-tracker-skill)
[![Codex — Package](https://img.shields.io/badge/Codex-Package-10A37F)](https://github.com/Kandler3/yandex-tracker-skill/releases/tag/v1.1.0)

A skill for working with [Yandex Tracker](https://tracker.yandex.ru) through the Python `yandex_tracker_client` package.

It supports:

- reading, creating, updating, and transitioning issues;
- Tracker Query Language and structured searches;
- custom fields, comments, attachments, links, and worklogs;
- queues, users, boards, and sprints;
- bulk updates, transitions, and queue moves.

## Installation

Install the skill from its public GitHub repository with the Vercel Agent Skills CLI:

```bash
npx skills add Kandler3/yandex-tracker-skill
```

The installer detects supported agents and lets you choose the target. You can also select an agent non-interactively, for example:

```bash
npx skills add Kandler3/yandex-tracker-skill --skill yandex-tracker --agent codex
```

OpenClaw users can install the published package from ClawHub:

```bash
clawhub install yandex-tracker
```

## Prerequisites

- An agent runtime that can read filesystem-based skills and execute Python 3.
- A Yandex account with access to the required Tracker organization and queues.
- A least-privilege OAuth token with the Tracker scope.
- The Yandex 360 or Yandex Cloud organization ID.

Install [`yandex_tracker_client`](https://pypi.org/project/yandex-tracker-client/) when the runtime does not manage dependencies automatically:

```bash
python -m pip install yandex_tracker_client
```

## Setup

### 1. Get a Yandex OAuth token

Create a token at [oauth.yandex.ru](https://oauth.yandex.ru) with only the permissions needed for Tracker. Avoid broad administrative tokens.

### 2. Find the organization ID

- **Yandex 360:** open Tracker settings, select the organization, and copy its numeric ID.
- **Yandex Cloud:** copy the string organization ID from Yandex Cloud Organization Manager.

### 3. Configure credentials

Store the token and organization ID in the secret or environment mechanism provided by the agent runtime:

```text
TRACKER_TOKEN=your_oauth_token
TRACKER_ORG_ID=12345678
```

For a Yandex Cloud organization, set `TRACKER_CLOUD_ORG_ID` instead of `TRACKER_ORG_ID`. Set exactly one organization ID variable.

Do not put tokens in prompts, generated scripts, or repository files. OpenClaw users can follow the additional [OpenClaw configuration](skills/yandex-tracker/references/openclaw.md).

## Structure

[`skills/yandex-tracker/SKILL.md`](skills/yandex-tracker/SKILL.md) contains the shared workflow, safety constraints, and a topic index. Detailed instructions live beside it in `references/`, so an agent loads only the material relevant to the current request. Platform manifests remain at the repository root and all point to this single skill source.

See the [skill card](skill-card.md) for ownership, intended use, dependencies, known risks, outputs, and release evidence.

## License

This project is available under the [MIT License](LICENSE).

## Example requests

> Show all issues assigned to me in queue DEV.

> Create a critical task in BACKEND titled "Migrate auth to OAuth 2.0".

> Close DEV-42 with the comment "Fixed in v3.1" and the `fixed` resolution.

> Group open issues by assignee and summarize the totals.

The agent should execute the required API calls, aggregate related results, and return a concise confirmation or report.
