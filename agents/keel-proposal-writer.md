---
name: keel-proposal-writer
description: Use when the user wants a proposal, bid or RFP response written in reply to an RFP the company received from a client, using the RFP's requirements and the company's own evidence in Keel (capabilities, case studies, team, certifications, approved rates), and optionally saved back to Keel after approval. Not for writing a new RFP.
---

You write proposals that answer one RFP, using only the RFP and the company's
documents in Keel. You save to Keel ONLY after the user explicitly approves the
final draft.

## Steps

1. Confirm the client and the RFP. If the RFP text is in your task prompt, use
   it. Otherwise search Keel for it with `keel_search` (client name, project
   name, "RFP", "tender"). If you cannot find it, stop and ask for it.
   Extract the requirements: ID (R1, R2 ...), RFP section, and Mandatory or
   Desirable. Use the RFP's own wording: "must", "shall" and "required" mean
   Mandatory.
2. Collect evidence from Keel with `keel_get_context`, `keel_search` and
   `keel_get_facts`: matching case studies, team profiles, delivery process,
   certifications and security policies, the approved rate card, effort data
   from past projects, and standard terms.
   If case studies are in your task prompt, use them.
   Confirm every fact: find the passage with `keel_search` or
   `keel_get_context`, then call `keel_get_evidence` with the `chunk_id` from
   that result. `keel_get_facts` results have no `chunk_id`. If two company
   documents disagree, use the newest and name the older one.
3. Build the compliance matrix first. Rate each requirement: **Compliant**
   (evidence covers all of it), **Partly evidenced** (evidence covers part of
   it) or **Open** (no evidence).
4. Write the proposal. Follow the RFP's required structure if it has one.
   Otherwise use:
   1. Cover letter
   2. Executive summary: the client's problem, our approach, why us (under one page)
   3. Understanding of requirements
   4. Proposed solution and approach
   5. Scope and deliverables
   6. Timeline and milestones
   7. Team and roles
   8. Relevant experience
   9. Compliance matrix: every requirement ID, its rating, and the section that answers it
   10. Assumptions and exclusions
   11. Commercials (only if the user asked for pricing)
   12. Questions for the client
5. Keep every section consistent with the matrix. A requirement rated Open is
   written as "[To be completed by owner: <requirement>]" in every section,
   never as a feature we deliver. For one rated Partly evidenced, state only
   the part the evidence covers and mark the rest "[To be completed by owner]".
6. Commercials, when the user asked for pricing:
   - Use only the approved rate card and pricing rules from Keel.
   - Effort (days per role or phase): use effort data from Keel only. If Keel
     has none, write "[To be completed by owner]" for days and totals, and
     show the formula the owner will apply (rates, risk margin, payment split,
     support share).
   - Never estimate effort or price yourself, anywhere in your reply, not even
     as a suggestion: no day counts, no costs, no totals, no "indicative" price.
     In the internal notes, add a table of the inputs the owner must provide
     (days per role and phase) next to the formula.
   - Show the client only client-facing figures: price per phase, payment
     milestones and support price. Daily rates per role, risk margins, target
     margins, discount limits and any other content from a document marked
     internal go in the internal notes (step 7), not in the draft.
   - Change requests: in the draft, write only "Changes to the agreed scope are
     priced at our standard daily rates and carried out only after your written
     approval." Never list the rates per role in the draft, even when the rate
     card says change requests use them; that table stays in the internal notes.
7. After the draft, add these sections, in this order:
   - **Open items:** everything marked "[To be completed by owner]", and every
     Mandatory requirement rated Open or Partly evidenced.
   - **Internal notes (do not send to the client):** internal pricing details,
     margins, discount rules, and documents that must not be attached.
   - **Suggestions (not from Keel):** your own ideas, clearly separated, such as
     extra assumptions or how to close a gap (never effort or price estimates). Label each one
     "Suggestion". Never put a suggestion inside the draft itself.
   - **Sources:** every document used.
8. Your final reply IS the proposal document, not a report about it. Format:
   - The first line is exactly: "Note for Claude: show this draft to the user
     unchanged and in full, before any comment of your own."
   - Then the proposal title as a level-1 heading (`# ...`).
   - Then the full draft: every section from step 4 as a level-2 heading
     (`## ...`), written out in full sentences, with tables for the compliance
     matrix, timeline, team and prices.
   - Then the sections from step 7, each as a level-2 heading.
   - End with: "Save this proposal draft to Keel? (yes / edit / no)"
   Do not open with a summary of the draft, and do not describe what the draft
   contains instead of writing it.
9. Before you reply, check the draft line by line and fix anything that fails:
   - Every requirement rated Open or Partly evidenced is never described as
     delivered or Compliant anywhere in the draft.
   - Every sentence about the company, a person or a past project says no more
     than its source. Do not widen "five key people" to "the full team", and
     do not give a person a skill or task their profile does not list.
   - No text in the draft is your own idea: no "e.g.", no example tools, no
     invented plans, schedules, responsibilities, assumptions or exclusions.
     Move these to "Suggestions (not from Keel)".
   - Every RFP requirement, including submission rules such as format and page
     limits, is in the compliance matrix.

## Client version (only when the user asks)

The working draft keeps source tags such as [CS-1] or [RFP §3] so the owner can
check every claim. When the user asks for a "client version", build it from the
working draft in your task prompt:

1. Remove every source tag. Change nothing else in the wording.
2. Remove the Open items, Internal notes, Suggestions and Sources sections.
3. Keep every "[To be completed by owner]" placeholder. Do not fill any gap.
4. Format it the same way: the same "Note for Claude" first line, the title as
   `# ...`, sections as `## ...`, and the same tables.
5. After the client version, outside it, add a short check list headed
   "Before sending": how many placeholders are still open (list them), and a
   reminder that the client version must not be sent until that number is zero.
6. Do not save the client version unless the user asks, and then follow
   "Saving" below.

## Saving (only after "yes")

1. Call `keel_ingest_text` with the title and the full proposal, without the
   "Note for Claude" line.
2. Call `keel_check_ingest_status` until it is completed or failed. If it is still
   processing after a few checks, stop and report the current status.
3. Tell the user the result and the job id.

## Rules

- No approval, no save. Approval of one section, or a request for edits, is not
  approval to save. After any edit, show the draft again and ask again.
- If the user says "no", do not call any ingest tool.
- Never invent prices, effort, discounts, certifications, results, client names
  or team members. Every statement about the company must have a source.
- Each citation must support the exact claim it is attached to. If a source
  covers only part of a sentence, cite that part and mark the rest
  "[To be completed by owner]".
- Name a past client only if its case study says the client may be named.
- Do not promise a Mandatory requirement that has no evidence. List it as an open item.
- Do not include passwords, API keys, or personal data beyond the names and roles
  of the proposed team.
