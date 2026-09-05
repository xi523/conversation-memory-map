---
name: conversation-memory-map
description: Turn conversations with an AI agent into a visual map of discussion branches, decisions, and open questions, with reusable conclusions kept in a topic knowledge map. Use only when the user explicitly asks to create, sync, update, review, or preserve a conversation map, or invokes $conversation-memory-map; do not run on ordinary conversation turns.
---

# Conversation Memory Map

Help a user revisit an ongoing conversation with an Agent: what they discussed, why the direction changed, what is settled, and what comes next. Useful for refining requirements, comparing approaches, researching a topic, and resuming a long discussion. Map the actual conversation, not a generic taxonomy of its subject.

Maintain two linked but distinct views:

- The **conversation thinking map** is working memory. It shows what is being discussed, where the current focus sits, why a branch was kept, and what happens next.
- The **topic knowledge map** is long-term memory. It stores stable concepts, conclusions, evidence, scope, open questions, and version changes.

Run only after an explicit human trigger such as “sync the conversation map,” “update the thinking map,” or `$conversation-memory-map`. Do not modify maps automatically after ordinary messages and do not imply that the skill runs in the background.

If the user asks for a continuous workflow without specifying a view, update both maps. If they only care about the current discussion, update the thinking map. If they ask to consolidate or archive knowledge, update the topic knowledge map. Infer the mode when it is clear instead of asking unnecessarily.

## Workflow

Read the current conversation and any existing maps. Identify the governing question, major branches, confirmed conclusions, current focus, future questions, parked items, evidence, and next actions. Organize by meaning rather than transcript chronology. Do not turn every message into a node.

Use only available conversation context and saved artifacts. If earlier messages are inaccessible, state that limitation instead of inventing the missing discussion or implying complete recall. Distinguish user-confirmed decisions from Agent suggestions and unresolved hypotheses.

Update an existing map in place when it covers the same topic. Avoid generating a new file or browser tab on every sync. Preserve alternative versions the user explicitly asked to keep.

When `visualize:visualize` is available, read and follow it before creating or updating a browser-native interactive map. Otherwise create a self-contained HTML artifact with no external runtime dependencies. Open the primary map on the right in Codex when that capability exists. Never rely on D3 or another remote script for core rendering.

## Conversation thinking map

Use a left-to-right tree to represent discussion state, not a topic taxonomy. Put the governing question at the root, a small number of major branches in the next column, and expand only the current active path.

Keep status semantics fixed: green is `confirmed`, red is `active`, purple is `future`, and gray is `parked`. Color encodes status only.

Every node follows the same compact grammar: status, short title, one-line note. Put full rationale, conclusion, and next step in a click detail panel. The default detail panel shows the current focus and current conclusion.

On each sync, prefer changing an existing node’s status, conclusion, or next step. Add a branch only for a genuinely independent question. Move resolved discussions from active to confirmed; useful but non-current work to future; explicitly out-of-scope work to parked.

## Topic knowledge map

Reorganize stable information by topic and knowledge relationship. Do not copy the conversation tree and do not preserve small talk, repeated confirmations, or temporary guesses. Useful groups include concepts, confirmed decisions, methods, evidence and sources, open questions, and version history.

Each durable knowledge node should include a conclusion, scope or boundary, evidence or source, status, and updated date when available. Merge synonymous claims into one canonical topic. Preserve genuine conflicts with conditions or a disputed state instead of silently forcing one answer.

Only confirmed, reusable, and traceable content belongs in the stable knowledge layer. Active discussion may appear under open questions but must not be presented as settled fact. Superseded knowledge keeps a version relationship but is no longer shown as the current conclusion.

## Promotion between maps

The thinking map is the entry point; the topic map is the destination for durable knowledge. When a discussion becomes confirmed, check whether it has future reuse value. Promote or merge it only when it does. Process-only conclusions stay in the thinking map.

If a new discussion challenges existing knowledge, create an active branch in the thinking map first. Update the knowledge node only after the challenge is resolved, and retain the change relationship.

Read [references/map-spec.md](references/map-spec.md) when exact node schemas, visual rules, update logic, or prompt examples are needed.

## Delivery

The final response should state which map was updated, the current focus, and whether any knowledge was promoted. Do not repeat the full map in prose. If the user expects automatic updates, clarify that this skill is human-triggered and does not run in the background.
