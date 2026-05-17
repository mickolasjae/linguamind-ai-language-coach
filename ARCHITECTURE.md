# Architecture

LinguaMind is a four-plugin composition on MeDo. There is no separate backend — each plugin is a node in a flow, and state is threaded between them via the persistent storage plugin.

## Plugin 1 — URL/web

Optional first step. If the user submits a URL, this plugin fetches the page and returns readable text. If they paste raw text, this node is skipped.

```
text = web.fetch(url).readable_content
```

## Plugin 2 — LLM (extraction & analysis)

The LLM is invoked twice per session. The first call is vocabulary extraction, conditioned on the user's CEFR level and a sample of words they already know.

```
new_words = llm.complete(
  prompt = EXTRACTION_PROMPT,
  vars   = { text, level: "B1", known_sample: ["casa", "comer", "rápido"] }
)
```

A second call runs grammar gap analysis on user-written replies during role-play.

## Plugin 3 — Persistent storage

Per-user vocab graph (word, lemma, source sentence, mastery state), SM-2 scheduling state, and source library. Read before each extraction call (to condition the prompt), written after each session (to update the known-word list).

```
state = storage.get(user_id)
storage.put(user_id, {
  level: state.level,
  known_words: state.known_words ∪ new_words_accepted,
  history: state.history + [session_summary]
})
```

## Plugin 4 — Audio

Wraps text-to-speech. Called on role-play turns and on individual words when the user taps to hear pronunciation.

```
audio.speak(text = role_play_turn, voice = target_language_voice)
```

## Why this shape

The level-conditioning loop (storage → LLM → storage) is the part that makes LinguaMind feel personal rather than generic. Without it, the LLM produces the same vocabulary list it would for any B1 learner; with it, the list adapts session over session.
