# Ticket to Design Prompt — Claude Code plugin

Turn a Jira ticket, epic, and/or Confluence doc into a structured **Claude Design prompt** for
a new prototype or an addition to an existing one, plus a **Jira comment** to post alongside the
design link.

## Install (one time)

In Claude Code:

1. Add this marketplace:
   ```
   /plugin marketplace add <your-github-username>/ticket-to-design-prompt
   ```
2. Install the plugin and reload:
   ```
   /plugin install ticket-to-design-prompt
   /reload-plugins
   ```
   **When prompted for a scope, choose "you" (user scope)** so the skill works in every
   project, not just the current folder.
3. `/ticket-to-design-prompt` should now be available. Restart Claude Code only if it isn't.

## Requires

- An Atlassian MCP connection (Jira + Confluence) authorized for your account.
- A Figma MCP connection, for iterating on an existing prototype.

## Use

Point it at a Jira ticket, epic, or Confluence page:
```
/ticket-to-design-prompt PROJ-1234
```

The skill gathers the ticket/epic/doc context, checks for gaps (success metric, scope
boundaries, unconfirmed dependencies), applies Zone terminology and design-system rules, and
produces a Claude Design prompt plus a draft Jira comment for review before posting.

## Team-specific references

This skill assumes your team has, and can be pointed to:
- A terminology / product-language reference doc
- A team directory (who owns review for which product area)
- An OKR/KR tracking doc for the current quarter

It references these by name rather than hardcoded links, so update the skill's wording in
`plugins/ticket-to-design-prompt/skills/ticket-to-design-prompt/SKILL.md` if your team's docs
go by different names.

## Updating

Edit the skill, bump `version` in
`plugins/ticket-to-design-prompt/.claude-plugin/plugin.json`, commit, and push. Anyone with the
plugin installed picks up the change on their next `/plugin marketplace update` +
`/plugin update`.
