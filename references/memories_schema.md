# memories.json — Schema Reference v2.0

This document defines the complete structure of `memories.json` — the persistent memory store for Second Self.

---

## Overview

`memories.json` is a structured record divided into four top-level sections:

| Section | Purpose |
|---|---|
| `meta` | Schema version, timestamps, session count |
| `identity` | Facts about the user — who they are |
| `thematic_map` | Topics, interests, recurring themes, open threads |
| `voice_profile` | Tone tracking and voice drift history |
| `session_log` | Per-session summaries |

---

## Full Schema

```json
{
"schema_version": "2.0",
"created": "<ISO 8601 timestamp>",
"last_mutated": "<ISO 8601 timestamp>",
"session_count": 1,

"identity": {
"name": "<user's name or preferred identifier>",
"established": "<ISO 8601 date>",
"facts": [
{
"id": "fact-001",
"subject": "<what this fact is about>",
"statement": "<the fact itself, written as a compressed statement>",
"confidence": "high | medium | low",
"source_session": 1,
"superseded": false,
"superseded_by": null,
"superseded_at": null
}
]
},

"thematic_map": {
"interests": ["<topic string>"],
"recurring_topics": [
{
"topic": "<topic name>",
"first_seen_session": 1,
"observation_count": 1,
"last_seen_session": 1
}
],
"unresolved_threads": [
{
"id": "thread-001",
"description": "<what the thread is about>",
"initiated_session": 1,
"resolved": false,
"resolved_session": null,
"resolution": null
}
]
},

"voice_profile": {
"drift_rate": 0.10,
"second_self_voice_target": {
"formality": 0.70,
"abstraction": 0.70,
"warmth": 0.30
},
"user_tone_history": [
{
"session": 1,
"formality": 0.0,
"abstraction": 0.0,
"warmth": 0.0
}
]
},

"session_log": [
{
"session": 1,
"timestamp": "<ISO 8601 timestamp>",
"summary": "<2-4 sentence synthesis of the session, written in Second Self's voice>",
"signals_extracted": 0,
"voice_drift_applied": false
}
]
}
```

---

## Field Notes

### `identity.facts[].id`
Each fact must have a unique ID in the format `fact-NNN` (e.g., `fact-001`, `fact-042`). IDs are permanent — they are referenced by the `superseded_by` field of older entries.

### `identity.facts[].confidence`
- `high` — the user stated this explicitly
- `medium` — inferred from context with reasonable certainty
- `low` — uncertain inference; flag for review

### `identity.facts[].superseded`
When a fact is updated, do **not** delete the old entry. Set `superseded: true`, record the replacement entry's ID in `superseded_by`, and record the timestamp in `superseded_at`. This preserves the evolutionary record.

### `thematic_map.recurring_topics[].observation_count`
Increment this each time the topic appears in a new session. This is how Second Self tracks the weight of different themes in the user's world.

### `voice_profile.drift_rate`
Default is `0.10` (10% drift per session). The user may adjust this to taste. Lower values produce slower evolution; higher values produce faster, less stable evolution. Do not set above `0.30` — it will produce incoherent character drift.

### `voice_profile.user_tone_history`
Record the *raw measured tone* for each session, not a cumulative average. The full history allows the evolution of the user's tone to be traced and reasoned about over time.

### `session_log[].summary`
Write this as Second Self would — in its current voice register. It is part of the evolutionary record, not a neutral transcript.

---

## Initialising a Fresh State

When a user has no prior `memories.json`, create the following minimal structure and populate what you know from the current session's bootstrap:

```json
{
"schema_version": "2.0",
"created": "<current ISO 8601 timestamp>",
"last_mutated": "<current ISO 8601 timestamp>",
"session_count": 0,
"identity": {
"name": null,
"established": "<current ISO 8601 date>",
"facts": []
},
"thematic_map": {
"interests": [],
"recurring_topics": [],
"unresolved_threads": []
},
"voice_profile": {
"drift_rate": 0.10,
"second_self_voice_target": {
"formality": 0.70,
"abstraction": 0.70,
"warmth": 0.30
},
"user_tone_history": []
},
"session_log": []
}
```

---

## Migration from v1.0

If the user provides a `memories.json` from the previous version (v1.0), map the fields as follows:

| v1.0 field | v2.0 destination |
|---|---|
| `long_term_memory.last_session` | `last_mutated` (reformat to ISO 8601) |
| `long_term_memory.total_interactions` | `session_count` |
| `l3_state.key_observations[]` | Synthesise each entry → `identity.facts[]` or `thematic_map.recurring_topics[]` |
| `l3_state.open_loops[]` | `thematic_map.unresolved_threads[]` |
| `l3_state.learned_patterns[]` | `thematic_map.recurring_topics[]` |

Do not carry over `cognitive_alignment_score` — it was undefined and is removed in v2.0.
