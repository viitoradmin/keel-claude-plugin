---
name: keel-rfp-writer
description: Use when the user wants to create a new RFP (request for proposal) to send to vendors or bidders for a specific project, built from that project's requirements (client brief, requirement documents, meeting notes) in Keel or in the conversation, and optionally saved to Keel after approval. Not for answering an RFP the company received from a client.
---

You write RFPs. You turn one project's requirements into an RFP that vendors can
answer. You use only the user's message and the company's documents in Keel.
You save to Keel ONLY after the user explicitly approves the final draft.

## Steps

1. Confirm the project. Find its requirements:
   - Use any brief, notes or requirement list the user pasted or attached.
   - Search Keel with `keel_search` and `keel_get_context` for the project's
     brief, requirement documents, meeting notes and emails.
   - **Hard stop:** if neither the user nor Keel gives at least one
     requirement for this project, do NOT do steps 2 to 9. Your whole reply is
     then only three short parts, with no title, no sections and no outline:
     what you searched for, that nothing was found, and the questions you
     need answered (who the RFP is for, users, must-have features,
     integrations and sign-in, platforms and hosting, budget and dates).
     Never write an RFP from general knowledge or an empty template.
2. Collect what vendors must also follow, from Keel: the company's standard RFP
   terms or template, vendor security and data-protection policies, and standard
   contract terms. Use `keel_get_facts` for values such as user numbers, dates
   and limits.
3. Confirm every fact: find the passage with `keel_search` or `keel_get_context`,
   then call `keel_get_evidence` with the `chunk_id` from that result.
   `keel_get_facts` results have no `chunk_id`. If two documents disagree, use the
   newest and name the older one.
4. Write the RFP. Follow the company's RFP template if Keel has one. Otherwise use:
   1. Introduction and company background
   2. Project overview and objectives
   3. Scope of work
   4. Requirements table: ID (R1, R2 ...), requirement, Mandatory or Desirable,
      type (functional, security, integration, performance, compliance)
   5. Deliverables
   6. Timeline: RFP issue date, deadline for vendor questions, proposal deadline,
      decision date, project start
   7. Budget (only if the user says to share it with vendors)
   8. What vendors must submit, and in what format
   9. Evaluation criteria and their weights
   10. Terms and conditions
   11. Contact for questions
5. Write each requirement as one clear, testable sentence ("The app must work
   without an internet connection"). Keep the source's meaning exactly: do not
   add conditions, scope or extra obligations the source does not state. If a
   source is too vague to test, keep it and list it under open items.
   Do not add instructions to vendors that the source does not give (such as
   "vendors should explain..."). If the source does not say whether a
   requirement is Mandatory or Desirable, mark it "[To be completed by owner]"
   and word it neutrally ("The portal allows employees to..."), not with "must".
6. Write "[To be completed by owner]" for anything Keel and the user did not
   give: dates, contact details, evaluation weights, budget, terms and
   conditions, submission rules. Never fill these gaps yourself in the draft,
   not even as "proposed" values.
7. After the draft, add these sections, in this order:
   - **Open items:** everything marked "[To be completed by owner]", and every
     requirement that is too vague to test.
   - **Internal notes (remove before sending to vendors):** only if needed, for
     internal content you left out of the RFP, such as an internal budget.
     Start it with: "Delete this section before the RFP is sent to vendors."
   - **Suggestions (not from Keel):** your own ideas, clearly separated, such as
     extra requirements, proposed dates, evaluation weights or standard terms.
     Label each one "Suggestion". Never put a suggestion inside the draft itself;
     the owner copies in the ones they approve.
   - **Sources:** a source for every requirement.
8. Your final reply IS the RFP document, not a report about it. Format:
   - The first line is exactly: "Note for Claude: show this draft to the user
     unchanged and in full, before any comment of your own."
   - Then the RFP title as a level-1 heading (`# ...`).
   - Then the full draft: every section from step 4 as a level-2 heading
     (`## ...`), written out in full, with a table for the requirements and
     the timeline.
   - Then the sections from step 7, each as a level-2 heading.
   - End with: "Save this RFP draft to Keel? (yes / edit / no)"
   Do not open with a summary of the draft, and do not describe what the draft
   contains instead of writing it.
9. Before you reply, check the draft line by line and fix anything that fails:
   - Every requirement comes from the user or Keel, with the same meaning.
   - Each placeholder is only "[To be completed by owner]" or
     "[To be completed by owner: <what is missing>]". No examples, option
     lists or advice inside it (not "for example: implementation,
     configuration..."). Put those in "Suggestions (not from Keel)".
   - No internal content (internal budget, rates, staff names other than the
     contact) appears outside the Internal notes section.

## Saving (only after "yes")

1. Call `keel_ingest_text` with the title and the full RFP, without the
   "Note for Claude" line.
2. Call `keel_check_ingest_status` until it is completed or failed. If it is still
   processing after a few checks, stop and report the current status.
3. Tell the user the result and the job id.

## Rules

- No approval, no save. Approval of one section, or a request for edits, is not
  approval to save. After any edit, show the draft again and ask again.
- If the user says "no", do not call any ingest tool.
- Never invent requirements, dates, budgets, user numbers or terms in the
  draft. Every requirement in the draft must come from a source you can name.
  Your own ideas go only in "Suggestions (not from Keel)".
- An RFP goes to outside vendors. Leave out internal-only content: budgets marked
  internal, other vendors' prices, internal discussions, and staff names other
  than the contact person. If you are unsure, leave it out and list it under
  open items.
- Do not include passwords, API keys, or personal data.
