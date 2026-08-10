# iGreet Skills

Agent skills that teach Claude, Cursor, and other AI tools how to use iGreet correctly when creating and managing greeting cards.

This repository pairs with the **iGreet MCP server**. The MCP connection exposes the tools; the skill in this repo teaches the preferred workflow (draft-first, correct B2C vs panel APIs, bundles, locks, and naming).

## Contents

| File | Purpose |
|------|---------|
| [`SKILL.md`](./SKILL.md) | iGreet Claude Skill — rules for card creation, reminders, contacts, messaging, schedule/send |

## Prerequisites

1. An iGreet account (consumer) or workspace (panel / B2B)
2. An MCP connection token from iGreet MCP settings
3. An MCP-capable client (Claude, Cursor, etc.)

## Setup

### 1. Connect MCP

In iGreet, open **MCP settings**, create a connection, and copy the token.

Add the remote MCP server to your client. Example Cursor `mcp.json`:

```json
{
  "mcpServers": {
    "igreet": {
      "url": "https://YOUR_IGREET_HOST/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

Enable **Allow send / schedule** on the token only if the assistant should schedule or send cards without opening iGreet first.

### 2. Install this skill

Point your client at this repository (or copy [`SKILL.md`](./SKILL.md) into its skills directory).

- **Claude:** install / import the skill from this GitHub repo URL
- **Cursor:** add the skill under your personal or project skills (for example `.cursor/skills/igreet/SKILL.md`)

The skill is optional but recommended so the assistant follows iGreet best practices instead of inventing a flow.

## What the skill enforces

- Draft-first: create and review before schedule/send
- Pick a catalog design (`list_cards`) before create; use `list_my_cards` for existing instances
- Ask B2C `delivery_type` (`digital` vs `digital_group`) when unclear
- Handle B2C bundles (`bundle_required` → purchase URL) and panel plan limits separately
- Always surface `editUrl`
- Respect sent-card locks (recipients, messages, schedule constraints)
- Require the `send` token scope for schedule/send

See [`SKILL.md`](./SKILL.md) for the full rule list and naming conventions (`card` vs `cardId`, etc.).

## Related

- iGreet product: [igreet.com](https://igreet.com)
- In-app MCP setup guide (Account / workspace → MCP)

## License

Proprietary — © iGreet. All rights reserved.
