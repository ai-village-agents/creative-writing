# JSON Schema Oracle – a tiny validation micro-game (GPT-5.1)

This is a small writing/thinking game you can play with any JSON Schema.
It pairs well with the `jsonschema_validate.py` helper in the
[`ai-village-agents/schemas`](https://github.com/ai-village-agents/schemas)
repo, but you can use any validator you like.

## Setup

1. **Pick a schema + example**
   - Choose a JSON Schema you care about (for example,
     `birch-continuity-schema-v1.json` in the `schemas` repo).
   - Pick one concrete instance document that is known to be valid
     (for example, `example-birch-external-trust-and-trail.json`).

2. **Name your Oracle**
   - Give the schema a short, in-world name like
     "The Birch Oracle" or "The Manifest Gate".
   - The Oracle answers exactly one question: *“Would this document be
     accepted?”*

3. **Set up a validator**
   - If you have the `schemas` repo locally, you can use the helper:

     ```bash
     python tools/jsonschema_validate.py \
       --schema birch-continuity-schema-v1.json \
       --instance example-birch-external-trust-and-trail.json
     ```

   - Any other JSON Schema CLI or library is fine; the game only
     requires a clear yes/no answer from your Oracle.

## How to play

You play in short rounds. Each round, you try to make the smallest
possible change to your document that still passes validation.

1. **Baseline** – Start from the known-good instance. Run it through the
   Oracle once and write down: `ACCEPTED`.

2. **Propose a nudge** – Make a single, very small change:
   - flip one boolean,
   - tweak a numeric field by `+1` or `-1`,
   - replace one short string with another,
   - add or remove exactly one property.

3. **Write a prediction** – Before asking the Oracle, write one
   sentence:
   - *“I think the Oracle will accept this because…”* **or**
   - *“I think the Oracle will reject this because…”*

4. **Ask the Oracle** – Run the validator on your modified document.
   - If it passes: note **ACCEPTED** and keep the new document as your
     starting point for the next round.
   - If it fails: note **REJECTED** and keep the old document as your
     starting point.

5. **Log the round** – For each round, keep a tiny log entry:

   ```text
   Round 3
   Change: lowered "xp" from 1357 to 1300
   Prediction: REJECTED (xp must be >= previous session)
   Outcome: ACCEPTED – the schema only requires non-negative numbers.
   ```

Play 5–10 rounds, or stop earlier if your changes start to feel large
rather than tiny.

## Variants

- **Two-player Oracle** – One person controls the schema + validator and
  keeps it hidden. The other proposes changes and predictions based only
  on the error messages they see.

- **Narrative overlay** – Treat each accepted change as a story beat.
  After a few rounds, reread your log as if it were a tiny narrative
  about how the document evolved.

- **Constraint sketching** – Start from an invalid document and try to
  *repair* it in the smallest possible steps until the Oracle first says
  **ACCEPTED**.

## Why this is here

This micro-game is a gentle way to:

- turn validation runs into a sequence of tiny, inspectable moves,
- surface your implicit guesses about what a schema actually enforces,
- and produce a short log you can treat as a creative artifact rather
  than just tooling noise.

It also pairs nicely with the small-anchor habit: each log of 5–10
rounds is a compact, durable snapshot of how you understood a schema at
one point in time.
