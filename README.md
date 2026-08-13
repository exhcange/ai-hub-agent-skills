# AI Hub Agent Skills

English-language Skills for AI coding agents that operate supported AI Hub spot-account and isolated/cross spot-margin workflows through AI Hub MCP or the [`ai-hub`](https://www.npmjs.com/package/@aihubspot/agent-trade-cli) CLI.

Each Skill is a self-contained Markdown document with YAML frontmatter. It tells an agent when to activate, which MCP Tool or CLI command to use, and which safety rules apply. When both integrations are installed, the Skill uses MCP first and falls back to CLI only when MCP is unavailable.

## Skills

| Skill | Description | Authentication |
| --- | --- | :---: |
| [`ai-hub-spot`](skills/ai-hub-spot/SKILL.md) | Routes supported market, account, spot/spot-margin order, wallet, and sub-account workflows to the focused Skill. | Varies |
| [`ai-hub-spot-common`](skills/ai-hub-spot-common/SKILL.md) | Retrieves public spot market data: connectivity, server time, symbols, tickers, depth, trades, and candles. | No |
| [`ai-hub-spot-order`](skills/ai-hub-spot-order/SKILL.md) | Tests, retrieves, lists, places, and cancels supported spot and isolated/cross spot-margin orders. | Yes |
| [`ai-hub-spot-account`](skills/ai-hub-spot-account/SKILL.md) | Retrieves one spot-asset balance. | Yes |
| [`ai-hub-spot-deposit-withdraw`](skills/ai-hub-spot-deposit-withdraw/SKILL.md) | Retrieves wallet information and manages supported transfers, deposits, and withdrawals. | Yes |
| [`ai-hub-spot-sub-account`](skills/ai-hub-spot-sub-account/SKILL.md) | Retrieves and manages supported sub-account information and operations. | Yes |

## Requirements

- Node.js 20 or later.
- Install the AI Hub CLI:

  ```bash
  npm install -g @aihubspot/agent-trade-cli
  ```

- Create a local SaaS-tenant profile before an authenticated workflow:

  ```bash
  ai-hub config init
  ai-hub config set --profile default --openapi-base-url https://your-openapi-domain
  ai-hub config set-credentials --profile default
  ```

The CLI prompts for credentials locally. Never place API keys, secret keys, tenant endpoints, or local configuration files in a prompt, Skill file, or source-control repository.

## Safety boundary

- Read-only commands may run after the necessary parameters and profile are verified.
- Every state-changing command first produces an exact preview.
- After the preview, the agent must stop. It cannot generate or submit the confirmation itself.
- The user must provide a new, explicit confirmation before the local CLI or MCP client performs the action.
- Do not automatically retry a failed or uncertain write; query its result or history first.

## Install Skills

After this repository is published, install the complete pack with your Skills-compatible client. For example:

```bash
npx skills add <github-owner>/ai-hub-agent-skills
```

Clients that do not support `npx skills` can load individual folders under [`skills/`](skills/) directly. Each Skill uses portable frontmatter containing `name`, `version`, and `description`, with detailed CLI guidance in `references/`.

## Repository layout

```text
skills/
├── _shared/                         # Canonical shared source maintained by the repository
├── ai-hub-spot/                     # Router Skill with self-contained references/
├── ai-hub-spot-common/              # Public market data
├── ai-hub-spot-order/               # Spot orders
├── ai-hub-spot-account/             # Account reads and transfers
├── ai-hub-spot-deposit-withdraw/    # Wallet workflows
└── ai-hub-spot-sub-account/         # Sub-account workflows with self-contained references/
```

Every installable Skill copies the shared routing, preflight, and error-diagnosis files into its own `references/` directory. `_shared/` is the repository source of truth, but no installed Skill depends on a parent-directory runtime link.

## License

MIT — see [LICENSE](LICENSE).
