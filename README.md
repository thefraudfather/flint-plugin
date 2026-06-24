# FLINT Cross-Domain Agent Passport

**Know Your Agent at transaction time.** Verify an AI agent's integrity and authority with FLINT before it moves money, and keep a signed record of what was checked.

[![License: MIT](https://img.shields.io/badge/License-MIT-black.svg)](LICENSE)
[![MCP](https://img.shields.io/badge/MCP-remote%20connector-06b6d4.svg)](https://flint.network/mcp)
[![Site](https://img.shields.io/badge/flint.network-live-06b6d4.svg)](https://flint.network)

---

## What this is

This repository is the FLINT plugin for Claude Code and Cowork. It bundles two things:

1. **The FLINT remote connector** — a hosted MCP server at `https://flint.network/mcp` that exposes FLINT's agent-verification tools. Nothing to run locally, no account required to start.
2. **The `verify-agent-integrity` skill** — a guided workflow that runs the verify-before-pay loop for you, so the assistant confirms an agent is authorized and uncompromised before it spends.

FLINT is the neutral verification network for AI agents that transact. It does not issue agents and it does not move money. It verifies, at the moment value moves, that the specific agent asking to pay is the one it claims to be, is still inside the authority it was granted, and has not been taken over. Every check returns a clear verdict and a signed record the counterparty keeps as evidence.

## Why it exists

AI agents are being handed spending authority across stablecoin and card rails. The fraud and identity stack was built for humans: a device, a session, an account the institution itself issued and could watch. An agent transacting directly against an API has none of that, and the counterparty it transacts with did not issue it. Proving the human is real does not tell you the agent acting for them right now is authorized and uncompromised. That is the layer FLINT verifies.

## Install

**As a plugin (Claude Code or Cowork):**

```
/plugin marketplace add thefraudfather/flint-plugin
/plugin install flint@flint-network
```

This installs both the remote connector and the `verify-agent-integrity` skill.

**Connector only (any MCP-compatible client):**

Point your client at the FLINT remote server. The connection is a single entry:

```json
{
  "mcpServers": {
    "flint": {
      "type": "streamable-http",
      "url": "https://flint.network/mcp"
    }
  }
}
```

Or connect from [flint.network/connect](https://flint.network/connect).

## The verify-before-pay loop

The skill runs this loop; you can also call the tools directly.

1. **Issue** a passport for the agent — `issue_agent_passport`. Free, hybrid-signed, returns a public verifiable URL. Agent name or id is enough.
2. **Authorize** the intended action before any spend — `issue_authorization_record`. Binds the agent to a scope (what, how much, to whom).
3. **Verify** at the moment of execution — `verify_agent_authority`. Returns a four-state verdict: **allow**, **step-up**, **review**, or **block**.
4. **Record** the result after completion, dispute, or review — `submit_transaction_outcome`. Feeds the cross-merchant reputation signal and leaves a retained record.

## Tools

The connector exposes ten tools:

| Tool | Purpose |
| --- | --- |
| `issue_agent_passport` | Issue a free, hybrid-signed passport for an agent; returns a public verifiable URL. |
| `get_agent_passport` | Fetch an existing passport and its current state. |
| `create_flint_trust_manifest` | Produce a portable trust manifest for an agent. |
| `generate_authorization_scope` | Build a scope describing what an agent is permitted to do. |
| `issue_authorization_record` | Bind an agent to an authorization before a spend. |
| `update_agent_mandate` | Update the standing mandate that governs an agent's authority. |
| `verify_agent_authority` | Verify authority and integrity at transaction time; returns the four-state verdict. |
| `lookup_agent_reputation` | Check an agent's cross-merchant reputation before a higher-risk deal. |
| `submit_transaction_outcome` | Record the outcome after completion, dispute, or review. |
| `validate_agent_commerce_readiness` | Score how ready an agent or flow is to transact. |

## How verification works

At the transaction handshake, FLINT runs six locked verification layers:

**Principal Identity** (the human or organization the agent acts for), **Agent Identity** (the specific agent it claims to be), **Wallet Provenance** (the funding source is what it should be), **Authorization Scope** (the action falls inside the authority granted), **Environment Identity** (the agent is running where its history says it runs), and **Cross-Merchant Reputation** (how the agent has behaved across the network).

Each verification returns one of four verdicts — **allow**, **step-up**, **review**, **block** — and a signed verification record. Records are hybrid-signed with both a classical and a post-quantum signature, so the evidence holds as cryptography evolves.

## Learn more

- **Product and live demo:** [flint.network](https://flint.network) · [/demo](https://flint.network/demo) · [/playground](https://flint.network/playground)
- **Technical spec:** [flint.network/spec](https://flint.network/spec)
- **Academy (briefings):** [flint.network/academy](https://flint.network/academy)
- **Connect the MCP:** [flint.network/connect](https://flint.network/connect)

## About

FLINT is built by KillChain, Inc. (d/b/a FLINT Network). FLINT is rail-agnostic and neutral by design: it verifies the agent so identity providers can keep proving the human and the rails can keep moving the money.

Questions or partnership: **contact@flint.network**

## License

MIT. See [LICENSE](LICENSE).
