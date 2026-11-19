---
name: keel
description: Use when the user asks about their company's internal documents (policies, contracts, reports, incidents, project data), asks to save notes, records or files into Keel, or works on RFPs and proposals with the Keel agents (keel-rfp-writer, keel-proposal-writer), including showing their drafts. Answers come from the user's Keel workspace with cited source passages.
---

# Using Keel

Keel holds the user's company documents and the facts verified across them.
Answer from Keel, not from general knowledge, whenever the question is about
the user's own company.

## Answering a question

Plan and check the search with the `agentic-retrieval` skill (understand,
plan, search, review, answer). Use the tools below within its steps.

1. Search first with `keel_search`, using the user's key terms.
2. For a broader answer, call `keel_get_context` to gather related passages.
   For both tools, keep `top_k` between 1 and 25 (default 10) and leave
   `min_similarity` unset. Larger values are rejected; run more focused
   searches instead.
3. For questions about verified figures or statuses, call `keel_get_facts`.
4. Before stating a figure, date or rule, find its passage with `keel_search`
   or `keel_get_context`, then call `keel_get_evidence` with the `chunk_id`
   from that result. Quote the exact source passage and document name.
   `keel_get_facts` results have no `chunk_id`.
5. If documents disagree, say so and prefer the newest version. Name the
   superseded document.
6. If Keel has nothing relevant, say that plainly. Do not fill the gap from
   general knowledge.

## Showing progress

Keel searches can take a while, so tell the user what you are doing in plain
words. These lines go only between tool calls, never inside your final answer.

- Before a group of Keel calls, write one short line saying what you are
  looking for, for example "Searching Keel for the Example Logistics RFP...".
- After the group, write one short line saying what you found, for example
  "Found the RFP and 3 matching case studies." or "Nothing found for Okta;
  trying a broader search."
- Use the user's terms, not tool names. Do not write "Calling keel_search".
- Keep each line to one sentence. Do not repeat the same line for every call.
- When a Keel agent's draft comes back, the draft must still be the START of
  your reply. Write no progress line in front of it.

## Adding content

- Only add content when the user asks.
- Short notes or summaries: `keel_ingest_text`.
- Structured entries: `keel_ingest_record`.
- Files: `keel_ingest_file`, then report progress with
  `keel_check_ingest_status`.

## Working with the Keel agents

- An agent sees only the prompt you give it, not this conversation. When you
  hand work to an agent, include everything it needs from earlier steps: the
  RFP text or name, a requirements list, and case studies already found.
- Show the agent's result to the user in full. Keep its tables, its source
  column (document + section), its ready-to-use text and its list of gaps.
  Do not shorten it into a summary without sources.
- When a Keel agent returns an RFP or proposal draft, deliver it in BOTH places:
  1. **File:** if you can create files (for example in Cowork), save the
     complete draft as a Markdown file named after the title, such as
     `Proposal_ExampleLogistics_DRAFT_v1.md`, and give the user the link.
  2. **Chat:** copy the agent's draft word for word as the START of your
     reply (leave out only its "Note for Claude" line): the title, every
     section with its heading, and every table.
     Do not reword, shorten or polish it, do not describe it ("What the draft
     covers"), and do not offer to show it ("Should I put it in a doc?").
     Show it in full even when it is long.
  Add your own comments only after the draft, in a few lines, then repeat the
  agent's save question.
- When the user asks for a "client version" of a proposal, hand the full
  working draft to `keel-proposal-writer` in the task prompt and ask for the
  client version. Deliver it in both places, the same way.

## Rules

- Never invent citations. Every cited passage must come from a Keel tool result.
- Claude only sees what the user's own Keel account can see.
