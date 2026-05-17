# Prompt Design

LinguaMind's behavior is shaped by three production prompts. The full prompt text is proprietary to this project. This file documents *what each prompt does* so the architecture can be evaluated, without handing competitors a copy-pasteable template.

## 1. Vocabulary extraction (level-conditioned)

**Input:** a paragraph of text the user pasted, the user's CEFR level (A1–C2), and a short sample of words the user already knows comfortably (drawn from the persistent vocab graph).

**Output:** up to 8 candidate vocabulary items, each with a word/phrase, a one-line English gloss, and the source sentence it appeared in.

**Why level-conditioning matters:** without the known-word sample, the LLM regresses to a generic frequency-list-shaped output that's indistinguishable across users. Conditioning on what the user *already* knows is what makes the same text yield different vocab for an A2 learner vs. a B2 learner. This is the load-bearing trick of the whole product.

## 2. Role-play tutor

**Input:** a scenario (cafe, airport, debate, first-date — chosen by the user or sampled from a list keyed to the user's level), the target language, the user's level, and the new words extracted in step 1.

**Output:** a 4–6 turn natural conversation in the target language, designed to give the learner a reason to deploy each new word. The tutor mirrors corrections in-character rather than breaking the scene to lecture.

## 3. Grammar gap analysis

**Input:** the learner's turns from a completed role-play.

**Output:** up to 3 recurring grammar gaps (not typos, not one-off slips), each with the pattern name, the learner's version, the corrected version, and a one-line rule-of-thumb.

---

The prompts are versioned and tuned against a held-out evaluation set. Production prompt text is not published.
