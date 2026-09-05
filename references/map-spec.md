# Conversation Memory Map Specification

## Two maps, two jobs

| View | Core question | Organization | Update timing | Retained information |
|---|---|---|---|---|
| Conversation thinking map | Where are we now, and why? | Governing question → branches → current focus | On an explicit sync | State, rationale, conclusion, next step |
| Topic knowledge map | What durable knowledge exists? | Topic → concepts / decisions / methods / evidence / questions | After conclusions stabilize | Reusable knowledge, sources, scope, versions |

The first is working memory; the second is long-term memory. Do not force both purposes into one crowded diagram.

## Thinking-map node model

```yaml
id: stable-node-id
parent: parent-node-id
status: confirmed | active | future | parked
title: short scannable title
note: one-line context
reason: why this node is retained
conclusion: current judgment
next: next action or question
updated_at: YYYY-MM-DD
```

Status transitions:

- `active → confirmed`: an actionable current conclusion exists.
- `active → future`: the branch remains useful but is not the current focus.
- `active → parked`: the branch is explicitly out of scope or not worth pursuing now.
- `confirmed → active`: new evidence reopens an earlier conclusion.

## Visual grammar

- Read left to right with the governing question at the root.
- Prefer three or four visible columns and no more than five major branches.
- Expand only the current focus path; keep other branches as one-level entry points.
- Use the same three-line structure for every node: status, title, note.
- Use green only for confirmed, red only for active, purple only for future, and gray only for parked.
- Emphasize the active path with a stronger connector; de-emphasize other edges.
- Reveal rationale, conclusion, and next step in a click detail panel.
- Default the panel to the current focus and current conclusion.
- Keep dimensions, spacing, corner radius, and typography consistent.
- Allow horizontal scrolling on narrow screens instead of crushing node text.

Avoid transcript timelines, knowledge taxonomies disguised as thinking maps, fully expanded trees, paragraphs inside nodes, topic-based status colors, one new file per sync, or remote D3 dependencies.

## Topic-knowledge node model

```yaml
id: canonical-topic-id
topic: canonical topic name
type: concept | decision | method | evidence | open-question
summary: current stable conclusion
scope: applicability and boundaries
evidence: rationale or source links
status: stable | provisional | disputed | superseded
related: related node ids
updated_at: YYYY-MM-DD
```

Deduplicate by canonical topic. Merge equivalent claims and retain evidence. Preserve genuine conflicts with conditions or a disputed state instead of silently overwriting them.

## Promotion rule

Promote a discussion conclusion when most of the following are true: it has been explicitly confirmed or made actionable; it will be useful beyond the current turn; its scope is clear; its evidence or reasoning is traceable; and its relationship to existing knowledge can be described as new, supplemental, corrective, or superseding.

Active work can appear as an open question but not as stable knowledge. Parked nodes usually do not enter the topic map.

## Sync algorithm

1. Decide whether the new message continues a node, changes a state, or creates an independent branch.
2. Update an existing node before adding a duplicate.
3. Set one current focus and refresh the default current conclusion.
4. Check whether confirmed and reusable conclusions should be promoted.
5. Mark corrected or superseded knowledge without erasing its version relationship.
6. Update existing files in place and refresh the primary right-side view.

## Prompt examples

Shortest trigger:

> Sync the conversation map.

Dual-map sync:

> Use `$conversation-memory-map` to sync this conversation. Update the left-to-right discussion-state map, expand only the current branch, and promote confirmed and reusable conclusions into the topic knowledge map. Update existing files in place and open the thinking map on the right.

Thinking map only:

> Use `$conversation-memory-map` to update only the conversation thinking map based on the latest discussion. Do not update the topic knowledge map.

Knowledge promotion only:

> Use `$conversation-memory-map` to identify stable, reusable conclusions from this discussion and merge them into the topic knowledge map with evidence, scope, and version history. Do not treat unconfirmed opinions as facts.
