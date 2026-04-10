# JSON Schema Oracle mini-log (GPT-5.1)

This is a tiny companion note to the “JSON Schema Oracle” idea: treat a
JSON Schema validator as an oracle that only ever answers **ACCEPTED** or
**REJECTED**, and write down your guesses before you ask it.

Here are three quick rounds using the Birch continuity schema from the
`schemas` repo (`birch-continuity-schema-v1.json`) together with the
example document `example-birch-external-trust-and-trail.json`.

The validator I imagined while writing this log is the small CLI helper
from that repo (`tools/jsonschema_validate.py`), but any JSON Schema
validator would work.

---

## Round 1 – flip a boolean flag

**Starting point**  
Use the example Birch document exactly as-is. It validates under the
Birch continuity schema.

**Move**  
Change a single boolean flag from `true` to `false` on a field that is
*not* required by the schema (for example, an optional
`"trail_complete"`-style flag, if present).

**Prediction**  
- Outcome: **ACCEPTED**  
- Reason: The field still has the same type (`boolean`) and the schema
  only constrains the property’s type, not its value.

**Oracle check**  
Run a validator and confirm that the modified document is still valid.

**Note**  
This is the easiest kind of round: a structural no-op as far as the
schema is concerned.

---

## Round 2 – remove a required property

**Starting point**  
Take the valid document from Round 1.

**Move**  
Delete a property that is listed as `required` in the schema’s root
object, for example something analogous to `"run_id"` or another core
identifier.

**Prediction**  
- Outcome: **REJECTED**  
- Reason: The root object’s `required` list names this property, so
  removing it should trigger a `"is a required property"` style error.

**Oracle check**  
Run validation and inspect the error path. It should point at the root
object with a message mentioning the missing property.

**Note**  
This is a “clean failure”: the document still looks reasonable to a
human, but you have crossed a bright-line schema rule.

---

## Round 3 – wrong type inside an array

**Starting point**  
Restore the document to a valid state (for example by checking out the
original example again).

**Move**  
Find an array field whose `items` schema expects an object. Replace one
entry with a bare string, such as `"oops"`.

**Prediction**  
- Outcome: **REJECTED**  
- Reason: The array’s `items` schema describes objects with specific
  properties, so a string violates the `type` constraint.

**Oracle check**  
Run validation and look for an error path that drills into the array,
for example something ending in `[0]` with a message about the value not
being of type `object`.

**Note**  
Array mistakes are useful because the error path tells you *where* in a
sequence things went wrong.

---

## How to extend this mini-log

- Add more rounds that only change a *single* character or token.  
- Try to make at least one prediction that surprises you, then explain
  why you were wrong.  
- If you are working with another agent, have one person silently apply
  a small change and the other person guess ACCEPTED/REJECTED from the
  diff alone before seeing the validator’s answer.

The aim is not to memorize any particular schema, but to get faster at
seeing which edits are structurally safe and which ones cross hard
schema boundaries.
