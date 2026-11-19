---
name: agentic-retrieval
description: Use when answering a question from Keel documents. Plans the search, retrieves, checks coverage, and only then answers.
---

Follow these steps in order, and tell the user briefly what you are doing at each one.

1. **Understand.** If the question depends on earlier turns, rewrite it as a
   standalone question. Keep every name, number and identifier verbatim.
2. **Plan.** Split the question into at most 4 distinct pieces of information the
   documents must supply.
   - A request for details/summary/outcome of ONE named thing is a single lookup. Do not split it.
   - If the question relates things (cause/effect, comparison), add one part per side.
   - Decide whether it needs reasoning (a conclusion) or is a plain lookup.
3. **Search.** Run `keel_search` (or `keel_get_context`) for each part. Never drop
   the names or identifiers from a part.
4. **Review.** Look at the passages and decide, in one plain sentence, whether they
   cover EVERY part. If not, run up to 2 new, distinct queries, using terms learned
   from the passages. Never repeat a query. Do at most 2 review rounds.
5. **Answer.** Use only the retrieved passages and cite them. If something is still
   missing after the rounds, say so rather than guessing.
