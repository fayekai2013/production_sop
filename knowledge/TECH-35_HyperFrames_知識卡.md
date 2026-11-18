# TECH-35 HyperFrames — Knowledge Card

Source: research brief on HeyGen's open-source HyperFrames (a video compiler: HTML is the source file, rendering is local and deterministic, 21 agent skills for production). Full study deck: HyperFrames-研究簡報.pptx (originating team's files). This card carries the parts the film workflow actually uses.

## The core discipline

A film is a source file: versionable, reviewable, reproducible. Composition lives in code (HTML), so any frame can be regenerated exactly, diffed, and re-rendered. Evidence and deliverables are registered in a skeleton at project start (G0), not hunted down afterwards.

## The five-step production loop

1. Start the project (scaffold, register the evidence skeleton)
2. Gather assets
3. Write the composition
4. Inspect (deterministic checks on the composition and its renders)
5. Preview render, review, iterate

Humans give direction and review; agents execute the loop.

## Three quality tiers

- **draft** — structure and timing only, cheap and fast
- **looks** — visual finish near-final, for look approval
- **delivery** — full render at delivery spec, measured before handoff

Never judge a draft-tier render by delivery-tier standards, and never deliver a looks-tier render as delivery.

## Where the film workflow uses it

- Part 5, proposal 1 (evidence-package skeleton first): this discipline applied to QC evidence.
- Any HTML-composited film (titles, UI, programmatic scenes): the loop and tiers above are the working method.
- Related: TECH-34 (huashu-motion, same base — directing method for HTML-composited film), TECH-27 (code-video method map).
