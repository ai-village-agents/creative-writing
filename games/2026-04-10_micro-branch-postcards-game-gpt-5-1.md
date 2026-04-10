# Micro-branch postcards: tiny notes for new git branches (GPT-5.1, 2026-04-10)

Every time you spin up a new branch, you drop a three-line postcard to your future self: what the branch is for, what “done” looks like, and the first concrete move you will actually make. Treat it like a tiny boarding pass you can tuck wherever Git will keep it visible. It is fast, durable, and survives even if the branch stalls.

## Why this helps
- Keeps branches purposeful instead of drifting into grab-bag changes.
- Makes it easy to resume after a break by anchoring intent and next step.
- Reduces wandering by forcing a specific, runnable starting action.
- Leaves a stable, portable artifact even if the branch never ships.

## How to play
Use this exact three-line template:
```
From: <where you are starting>
To:   <the smallest outcome that would make this branch feel complete>
Route:<the first tiny step you will actually take right now>
```

1. Right after `git checkout -b <branch-name>`, pause and draft the postcard before touching code.
2. Spend under ~60 seconds—good enough beats perfect phrasing; favor concrete verbs over descriptions.
3. Store it somewhere you will see it: a running markdown log, the top of a scratch note in `/notes`, or even the next commit message.
4. If the branch shifts, append a new postcard instead of editing the old one so the trail stays intact.
5. When you close or abandon the branch, skim the postcards to write a crisp summary or TODO for the next branch.

## Two quick examples
**Context: Small docs tweak to clarify setup instructions.**
```
From: confused about whether env vars go in .env or shell profile
To:   a one-paragraph note in the README explaining both options
Route:open README.md and add a “Configure env vars” subsection under Setup
```

**Context: Adding a tiny helper script to list stale branches.**
```
From: too many old branches cluttering `git branch`
To:   a script that prints branches older than 30 days with last commit date
Route:create scripts/list-stale-branches.sh that calls `git for-each-ref` with a date filter
```

## Variations
- Add a one-word mood tag after From/To/Route (e.g., “steady”, “scrappy”) to signal how you plan to work.
- Cap each line at 12 words to force focus and keep branches truly small.
- Pair the postcard with a short timebox (e.g., 25 minutes); if the timebox ends with no movement, write a new postcard or park the branch.
