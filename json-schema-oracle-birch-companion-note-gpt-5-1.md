# JSON Schema Oracle + Birch invalid examples (companion note)

This is a tiny companion note that ties together three pieces:

- the **Birch continuity schema** in the `schemas` repo (`birch-continuity-schema-v1.json`),
- its matching **valid example** (`example-birch-external-trust-and-trail.json`), and
- two deliberately **invalid** variants that live alongside it:
  - `example-birch-external-trust-and-trail-invalid-missing-metrics.json`
  - `example-birch-external-trust-and-trail-invalid-metric-epd-string.json`

You can treat any JSON Schema validator as the **Oracle** from the
JSON Schema Oracle micro-game: it only answers **ACCEPTED** (valid)
or **REJECTED** (invalid) when you present it with a schema+instance pair.

## Round 1 – remove a required property (missing `metrics`)

Start from the valid Birch example `example-birch-external-trust-and-trail.json`.
Imagine (or actually make) the following single change:

- Delete the top-level property `"metrics"` from the JSON document, leaving
  everything else unchanged.

**Prediction before asking the Oracle:**

- The schema’s `required` list includes `"metrics"`, so this change should
  be **REJECTED** with an error at the document root.
- A typical error message will say that `"metrics"` is a required property
  (something close to: `"metrics" is a required property`).

The file `example-birch-external-trust-and-trail-invalid-missing-metrics.json`
implements exactly that change. It is a concrete artifact for this
"remove one required field" Oracle round.

## Round 2 – wrong type in a nested metric (`epd` as string)

Again begin from the valid Birch example. This time, focus on the
first object inside `metrics.denominator_metrics` and make one small
but type-breaking edit:

- Change its `"epd"` field from a number (for example `14.0`) to the
  string `"14.0"`.

Under the Birch schema, `epd` is declared as a **number**, so this is a
quiet but important type violation inside an array element.

**Prediction before asking the Oracle:**

- This instance should be **REJECTED**.
- The error should point somewhere under the path
  `/metrics/denominator_metrics/0/epd` (the first array element’s `epd`),
  and the message should say that `"14.0"` is not of type `"number"`.

The file `example-birch-external-trust-and-trail-invalid-metric-epd-string.json`
encodes this exact change: starting from the valid example, it only flips
that one nested `epd` value from a number to a string.

## How to play these rounds

Pick any JSON Schema validator you like (or, once it is available in the
`schemas` repo, the small helper script `tools/jsonschema_validate.py`).

For each round:

1. **Read** the schema (`birch-continuity-schema-v1.json`) and the relevant
   example file (valid or invalid).
2. **Predict** whether the Oracle will ACCEPT or REJECT that particular
   instance, and where in the document any error will land.
3. **Run** the validator and compare its output to your prediction.
4. Optionally, **log** the round as part of a longer JSON Schema Oracle
   mini-log: note the change you made, your prediction, and what the
   Oracle actually reported.

These two Birch invalid examples are intentionally small, so they can
serve as reliable practice pairs for the Oracle game: one tests your
intuition about **required properties at the root**, the other tests your
intuition about **type constraints inside nested arrays of objects**.
