# kopatel

kopatel is an autonomous multi-hour deep-research pipeline for Claude Code. It turns an explicit "dig deep into this topic" request into a reusable knowledge base: scout council -> wave loop -> consolidation -> static site.

Every claim is tagged with an A/B/C/D confidence tier, from primary-source evidence to weak folklore. Raw entries, consolidated digests, and the website remain separate so the result is both auditable and readable.

## Install

```
/plugin marketplace add olegpars/oleg-skills-public
/plugin install kopatel@oleg-skills
```

Then invoke the `kopatel` skill with an explicit deep-research request.

## Requirements

- Claude Code.
- A plan suitable for long multi-agent runs. Claude Max is recommended for full digs because waves can run for hours and touch many sources.
- A project directory where dig output can be written. By default, kopatel uses `./digs/<slug>/`.

## Quickstart

Ask for a focused dig:

```text
Dig deep into WebGPU debugging tools and build a knowledge base.
```

kopatel will:

1. Confirm the topic and mode.
2. Save output under `./digs/<slug>/` unless you choose another base.
3. Run scout to build a taxonomy and seed frontier.
4. Show the taxonomy before expensive waves begin.
5. Run research waves, process results on disk, and update the frontier.
6. Consolidate entries into subtopic and cross-cutting digests.
7. Build `dist/index.html`.

For long `full` runs, arm the supervised allowlist described in `.claude/skills/kopatel/references/unattended.md` before stepping away.

## Result Structure

```text
digs/<slug>/
├── _meta/                 engine copies, manifest, taxonomy, frontier, state
├── entries/               raw source notes, one file per researched source
├── subtopics/             consolidated digest per subtopic
├── cross-cutting/         consolidated cross-topic digests
├── OVERVIEW.md            executive overview and comparison tables
├── frontier.md            pending research queue
├── sources.md             deduplicated source registry
├── CHANGELOG.md           wave history
└── dist/index.html        static knowledge-base site
```

## What Is Included

- `scout.js`: independent taxonomy proposals plus synthesis.
- `wave.js`: parallel research agents with source extraction and `new_frontier`.
- `process-wave.js`: deduplication, frontier update, source registry, and wave telemetry.
- `consolidation.js`: idempotent digest generation.
- `build-site.mjs`: single-file static site builder.
- `scripts/overnight.*` and `references/overnight-runner.md`: portable headless overnight runner for long `full` digs.

The public skill uses a supervised allowlist plus heartbeat for long runs. It does not include private runner infrastructure.
