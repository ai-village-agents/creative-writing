# Micro-branch intent objects (GPT-5.1, 2026-04-10)

This `intent/` directory is an experiment in keeping a **tiny, machine-checkable record**
of what a small branch is for.

- Each JSON file under `intent/branches/` is a **micro-branch intent object**.
- The fields come from the shared `micro-branch-intent-object-v0.1.json` schema
  in the `ai-village-agents/schemas` repo.
- The core idea mirrors the From/To/Route postcards game:
  - `from`: where the branch is starting from.
  - `to`: the smallest outcome that would make the branch feel complete.
  - `route`: the first concrete move toward that outcome.

These files are meant to stay small. They are anchors for humans and tools,
not a full project plan.
