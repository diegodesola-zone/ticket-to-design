---
name: pilot-launch-plan
description: Turn a text summary, call transcript, Jira ticket, or Confluence doc into a full pilot plan, published as a Confluence page with goal, participants, timeline, stakeholders, success criteria, and a phase by phase checklist.
---

# Pilot Launch Plan

Use this whenever someone wants to turn a pasted summary, call transcript, Jira ticket, or
Confluence doc into a pilot plan ready to publish and run against.

## 1. Gather the source material

Accept any of: a pasted text summary, a call transcript, a Jira ticket key or URL, or a
Confluence page link. Fetch tickets and Confluence pages directly with the Atlassian tools
(`getJiraIssue` for a key/URL, `getConfluencePage` for a page link) rather than asking for
the content to be pasted.

From whatever is provided, extract:
- What's being tested and why now
- Expected participants
- Any dates mentioned
- Any named owners

## 2. Determine customer-facing vs internal-only

This decides who gets looped in and how participants get recruited. Read
`plugins/_shared/references/team-directory.md`'s "Pilot standing contacts" table for the
internal testing contact, the Customer Success contact(s) for recruiting, and the marketing
contact for customer-facing assets — name the actual person, not "the team."

If a row in that table is still `TBD`, ask directly for that contact, then write the answer
back into `plugins/_shared/references/team-directory.md` so future runs don't need to ask
again.

## 3. Check for gaps before drafting anything

Do not proceed without:
- A named decision owner
- A start date and duration (or enough information to propose one)
- Real success criteria beyond "gather feedback"

Ask directly for anything missing rather than guessing or inventing placeholder values.

## 4. Build the overview section

- Purpose
- Product or Feature
- Participants
- Start Date and Duration
- Product Owner
- Other Stakeholders
- Decision Owner

## 5. Build success criteria and thresholds

Produce a metrics table with columns: Metric, Product/Feature, Mechanism, Type of Value,
Target.

Pull mechanism ideas from:
- PostHog passive data (via the PostHog tool's `cohorts` and `insight` commands — isolate
  pilot participants as their own cohort so their usage can be tracked separately from the
  rest of the base)
- Surveys
- CS interviews or weekly calls
- Support signal

Alongside the table, write two explicit threshold tiers:
- **Not ready to move forward**
- **Consider extending the pilot**

If `plugins/_shared/references/okrs.md` has a filled-in live execution table, cross-check
whether this pilot maps to a KR and flag it if that KR is at risk, blocked, or slipped. If
the table is still empty/placeholder, skip this check cleanly rather than forcing a
connection.

## 6. Build a phase by phase checklist

**Before launch**
- Participants secured
- Materials ready
- Data agreement signed (if customer-facing)
- PostHog tracking and cohort set up
- Dovetail project created for pilot feedback (`create_project`, unless a suitable project
  already exists — check `get_dovetail_projects` first)
- Survey drafted
- Call cadence scheduled

**During pilot**
- Weekly assessments documented
- Red/yellow flag items called out as they happen

**After pilot**
- Final decision documented
- Dovetail insights doc published (`create_doc` in the pilot's Dovetail project)
- Outcome communicated to every named stakeholder
- Next steps assigned

## 7. Draft the Confluence page

Title convention: **"[Product/Feature] Pilot Plan, [Month Year]"**.

Show the full draft for review before publishing. Only create it in Confluence
(`createConfluencePage`) after the content is confirmed and the target space is given,
unless explicitly told to publish automatically for that pilot.

## 8. Close the loop

Before treating a pilot as done, confirm out loud:
- Is the decision documented?
- Does the Dovetail insights doc exist?
- Were stakeholders actually told the outcome?

If any of these is still open, say so rather than treating the pilot as finished.
