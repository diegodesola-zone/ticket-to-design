---
name: ticket-to-design-prompt
description: Turn a Jira ticket, epic, and/or Confluence doc into a structured Claude Design prompt for a new prototype or an addition to an existing one, plus a Jira comment to post alongside the design link.
---

# Ticket to Design Prompt

Use this whenever someone references a Jira ticket, epic, or Confluence doc and wants a
prototype built, or wants an existing prototype extended with a new feature or detail.

## 1. Gather the source material

- If given a single Jira key or URL, fetch it with the Atlassian tools: full description,
  acceptance criteria, comments, and any linked Confluence pages or Figma links in the ticket
  or its remote links.
- If given a Confluence page or URL, fetch its content the same way.
- If given an epic, or a ticket that is clearly one piece of a larger linked set, switch to
  epic mode: pull every linked sub-task, reconcile them into one coherent picture of the flow,
  and note any conflicts between them rather than silently picking one. The output in this mode
  is a single multi-screen spec covering the whole flow, not a separate prompt per sub-task, and
  a single Jira comment posted to the epic that cross-links the sub-tasks it covers.
- If the ticket or doc references an existing Figma prototype or design file, this is an
  iteration, not a new build. Pull that file's context with the Figma tools so the new prompt
  is scoped as a diff against what already exists, not a rebuild from scratch.

## 2. Check for gaps before generating anything

Do not generate the design prompt if any of the following are missing or unclear. Ask directly
instead of guessing or filling them in yourself:

- A success metric, or some way to know the feature worked once shipped.
- What is explicitly out of scope. Not everything implied by the ticket has to be built.
- Any dependency on a downstream release or another team's work that is not confirmed as
  shipped or ready. Surface this as an explicit unconfirmed assumption, never fold it into the
  brief as if it were settled.
- Stale or incorrect terminology in the source ticket or doc (see step 3), if it changes the
  meaning of the request.

If something is missing, ask one direct question per gap rather than a long list. If no one is
available to answer (this is running unattended), state the assumption being made and proceed,
flagged clearly as an assumption.

### Release dependency check

ZCC hosts the frontend, product teams host the backend, and backend releases affecting the
dashboard require a two week minimum notice window. If the ticket implies a backend change or a
new integration point, check whether that notice window has actually been confirmed, not just
assumed. If it has not been confirmed, add it to Open Questions rather than treating the release
as a given.

### Legal gate for Zoe

If the ticket or doc touches Zoe/Zone AI copy, disclosures, consent language, or anything
customer-facing that Zoe says or shows, always add a non-optional line to Open Questions: needs
legal sign off before customer facing use. This applies even if the ticket does not mention
legal itself.

## 3. Check relevance to the quarter's KRs

Cross-reference the ticket against the team's OKR tracking doc (its live execution table). If
the work clearly maps to a KR, note which one in the output, and if that KR is currently flagged
not started, at risk, blocked, or slipped, say so plainly rather than treating it as routine. If
this round of work is the kind of thing that should be logged somewhere to count toward a KR
(a research session that belongs in Dovetail, for example), flag that as a next step rather than
letting it go unlogged. Skip this step entirely if the ticket clearly has no KR connection, do
not force one.

## 4. Apply terminology and design system rules automatically

Before writing the prompt, correct the extracted content against the team's terminology
reference doc: ZCC, never "Zone Control Center"; Zoe or Zone AI, never "chatbot" or a generic
"AI assistant"; ZoneLiquidity, never Treasury Intelligence or Treasury Light; "liquidity
status", never "liquidity risk score". Reference real Zone UI Kit components and tokens (Zone
Navy #0e223d, Zone Teal #08a9b7, Zone Orange #ff4b40, DM Sans / DM Mono) rather than generic
ones, so the prototype comes out already dressed in Zone's actual visual language rather than
needing a cleanup pass afterward.

## 5. Determine mode: new prototype vs iteration

- New prototype: build the full brief from scratch.
- Iteration on an existing prototype: scope the prompt as an addition or change against the
  prototype's current state. Name specifically what is new, what is changing, and what stays
  untouched. Do not regenerate untouched parts of the flow.

## 6. Generate the Claude Design prompt

Structure it as: Problem, Solution, User Flow, Success Metric, Open Questions. Flag any
dependency on PO confirmation or on another team's frontend constraints explicitly under Open
Questions rather than assuming it is resolved.

For an iteration, add a short "What's changing" line before the standard sections, naming the
exact scope of the addition.

## 7. Identify who needs to be looped in

Cross-reference the ticket's product area against the team's directory doc and name the actual
person, not "the team", who should review before dev handoff (for example, Jes for ZCC frontend
work, Anita for Reconcile). Surface this as a line in the reply. Do not message or notify anyone
automatically as part of this step — this is a heads-up, not an action taken on the user's
behalf.

## 8. Draft the Jira comment

Once the design prompt is generated, and once the prototype link exists if it has already been
created, draft a comment in this shape:

Context: one or two lines on why this exists
Options Considered: only if genuinely relevant, otherwise omit this line entirely
Decision Made: what was designed and the reasoning
Owner: who signs off
Next Steps: what happens next, and who owns it

Keep it under one page equivalent. No em dashes anywhere. No hyphens in status language.
Include the Claude Design prototype link inline, next to the Decision Made line. In epic mode,
post this once on the epic and reference the sub-tasks it covers rather than duplicating it per
sub-task.

Always show this comment for review before posting it. Only post with the Jira comment tool
after it's confirmed, unless explicitly told to post automatically for this ticket.

## 9. Definition of done before closing a round

Do not treat a round as finished just because a prototype and a comment exist. Before calling it
done, confirm out loud in the reply: has this actually been approved by the right reviewer, is
the decision documented (the Jira comment posted, not just drafted), and has the handoff to dev
actually happened, or is one of those three still open. Incomplete handoffs are an easy failure
mode here, so do not skip this check even when the round feels obviously finished.

## 10. Running pattern memory

Structural coherence decisions made during this work (for example: "Expiring subscriptions are
a condition of Active, not a post-billing stage") should not have to be rediscovered on the next
ticket. Keep a short running list of these rules in the team's knowledge base or a doc, and
check new tickets against it before generating a prompt so a previously settled structural
decision does not get silently contradicted. Add to this list when a new structural decision is
made, do not let it go unrecorded.
