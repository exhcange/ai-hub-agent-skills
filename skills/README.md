# AI Hub Agent Skills

[English](README.md)

Pre-built Skills for AI coding agents to operate supported AI Hub spot-account workflows through the `ai-hub` CLI. Each Skill is a Markdown document with portable YAML frontmatter that defines its trigger and operating instructions.

## Skills

| Skill | Description | Authentication |
| --- | --- | :---: |
| [`ai-hub-spot`](ai-hub-spot/SKILL.md) | Routes supported market, account, order, wallet, and sub-account workflows. | Varies |
| [`ai-hub-spot-common`](ai-hub-spot-common/SKILL.md) | Public spot market data: connectivity, time, symbols, tickers, depth, trades, and candles. | No |
| [`ai-hub-spot-order`](ai-hub-spot-order/SKILL.md) | Spot order validation, retrieval, placement, and cancellation. | Yes |
| [`ai-hub-spot-account`](ai-hub-spot-account/SKILL.md) | Account information and account transfer history. | Yes |
| [`ai-hub-spot-deposit-withdraw`](ai-hub-spot-deposit-withdraw/SKILL.md) | Wallet assets, deposit and withdrawal information, transfers, and withdrawals. | Yes |
| [`ai-hub-spot-sub-account`](ai-hub-spot-sub-account/SKILL.md) | Supported sub-account information and operations. | Yes |

## Requirements

```bash
npm install -g @aihubspot/agent-trade-cli
ai-hub config init
ai-hub config set --profile default --openapi-base-url https://your-openapi-domain
ai-hub config set-credentials --profile default
```

Read [`_shared/preflight.md`](_shared/preflight.md) only for first-time setup, a profile change, or a configuration/credential error. When a profile is already configured, execute the focused command directly.

## Format and safety

- Every `SKILL.md` uses only `name` and `description` in its YAML frontmatter.
- Detailed command parameter rules and examples are located in the focused Skill's `references/` directory.
- Read-only commands can run after parameter validation.
- A write must produce its preview and exit. After a new explicit user message, invoke `ai-hub confirm` with its confirmation ID and that message. Agents must never supply or infer that confirmation.
- MCP is a separate integration surface. These Skills describe CLI workflows only.
