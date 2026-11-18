---
name: ai-film-production-workflow
description: Use for AI film and ad production with image and video models in the owner's workflow: planning, generating, QC-ing or revising a film, from script and shot list through character sheets, keyframes, video prompts, edit, VO, loudness, delivery and versioning. Trigger when the owner asks to make, continue, fix or quality-check an AI-generated film, ad, product video or phone demo, whether or not it shows real people or on-screen speakers. Do not use for story or screenwriting theory (use story-screenwriting-theory), for still-image-only tasks, or for general video-editing questions outside an AI production.
---

## How this skill is organised - read order

This file holds the core only. The full original text lives in `references/`; load only what the current film needs.

- **Default (every production):** this file - §2.1 non-negotiable principles, §2.2 process, gates and budget, §2.14 final QC checklist.
- **Product films and UI / phone demos:** also read `references/part2-field-lessons.md` (§2.3-2.13, §2.15).
- **Dialogue films (speakers on screen):** also read `references/part4-supplement2-dialogue.md`. Part 4's overrides apply **only** to dialogue films; every film without on-screen speakers keeps the base defaults in this file and in Part 2. (Scope rule from the original Part 4 integration note, preserved verbatim in that file.)
- **Casting phase, and again at cut, delivery and final QC:** also read the relevant sections of `references/part3-supplement1.md`. Its casting sections apply at casting; §3.9 (cut smoothness), §3.13 (delivery and archiving) and §3.16 (additions to the final QC checklist) apply in the later phases, alongside §2.14 in this file. Its sections extend Part 2 sections: §2.1, §2.2 and §2.14 are in this file; the rest of Part 2 is in `references/part2-field-lessons.md`.
- **Part 1 core method (full text):** `references/part1-core-method.md` - read when starting a new production or when a rule points back to it.
- **Consensus best practices (Part 5):** `references/part5-consensus-best-practices.md` - the 91 practices finalised with the Grok team on 2026-11-17 (gates and spend approval, likeness QC and the lead-frame rule, freeze classes, voice checks, subtitle frames, blind testing, copy management). Read the section matching the current phase alongside Parts 2 to 4; the Chinese final list named at the top of that file is the authoritative wording.

---

# Part 2 — Field Lessons (from real productions)

Part 1 above is the base method. This part adds rules and tools learned the hard way while shipping AI-generated brand films with **real people** (cast built from reference photos), **on-screen software UI**, voice-over, subtitles and music. Everything here is generic. Names, numbers and examples are invented for illustration.

Use Part 2 when any of these is true:
- the film shows real, recognisable people who must look like themselves;
- people are working at monitors, laptops, tablets or TVs and the screens must show specific UI;
- there is a director or client who reviews cuts and asks for specific redos;
- there is a usage cap or budget, and a fixed delivery spec.

---

## 2.1 Non-negotiable principles

1. **Plan and cost first. Nothing is generated before the plan is approved.** The plan covers the script, cut table, prompts, cost estimate and stop line.
2. **Pilot first.** Run 1–2 representative shots end to end (keyframe → takes → QC → comp → mini export) before the full batch.
3. **Image before video.** Every generated shot starts from an approved keyframe (first frame, and a last frame when needed). Never text-to-video a shot that has real people in it.
4. **Every prompt is complete on its own.** Prompts are built from fixed blocks by code, and every fixed setting (look, cast description, camera rule, avoid list) is repeated word for word in every prompt. Never write "same as before".
5. **The model never draws text, UI, charts, logos or numbers.** Screens are generated as plain glowing rectangles, and real UI is composited in post. Text cards and subtitles are added in post.
6. **Likeness is measured, not eyeballed.** Every person in every take is scored. Thresholds are in 2.5.
7. **No static shots.** Every live shot must visibly move: people breathe, look, type and shift, or the camera drifts slowly. Measure it (2.6).
8. **Keep good shots, redo only weak ones, and never overwrite.** Every version stays on disk with a version code.
9. **Archive everything** (prompts, keyframes, takes, comps, QC JSON, audio stems, EDL) and keep an index.
10. **Hit the output spec exactly** (2.12) and verify it with a probe, not from memory.
11. **Honesty and no quiet downgrades.**
    - If a shot fails a threshold and you keep it, say so in the QC report with the number.
    - If you drop a requirement (fewer takes, lower resolution, a skipped check, a simplified screen), say so *before* delivery, not after someone notices.
    - "Pass" means the metric passed, not "looks fine to me".
    - Never describe a draft as final.

---

## 2.2 Process, gates and budget

### 2.2.1 Gates

| Gate | Content | Pass condition |
|---|---|---|
| G0 | Inventory of assets, previous cut broken down | Asset list done |
| G1 | Plan: script, cut table, prompts, cost estimate | **Explicit approval** (nothing generated before) |
| G2 | Character sheets, location plates cleaned (no readable text, logos, third-party IP) | Sheets pass likeness; plates clean |
| G3 | Pilot keyframes | Likeness and head-angle thresholds |
| G4 | Pilot takes + usage report | Video likeness and drift pass; usage matches estimate |
| G5 | Remaining shots, 2–5 takes each | Stop at the usage stop line |
| G6 | QC: faces, hands, physics, screens, text | All pass or documented exceptions |
| G7 | Edit: UI comp, cards, VO, music, subtitles | Reviewer watches the rough cut |
| G8 | Delivery | Spec verified, archive and index updated |

### 2.2.2 What the plan must contain
- A one-line summary of the film and its story arc. Example: *overload → tools take over → people decide*.
- A cut table with these columns per cut: time in/out, type (LIVE generated / SCREEN / MG / CARD / REUSE), location, cast, action, VO line, on-screen UI, duration generated vs used.
- Prompts for every generated shot, assembled from blocks (2.3).
- Decisions needed from the reviewer, as a numbered list with a recommendation for each.
- Risks and mitigations (low-resolution reference photos, group shots with small faces, push-ins on screens that must be tracked, UI licensing, voice accent, music licensing).
- A cost estimate (2.2.3).

### 2.2.3 Cost estimate and usage cap
- Estimate **low / expected / high** with the assumptions written out: takes per shot (e.g. 2/3/5), keyframes per shot (e.g. 6/12/22), plates, retries.
- Express cost in **both** the plan's built-in usage units (% of weekly cap) and the API-equivalent money, so the reviewer sees both.
- Write the current usage and when it resets.
- **Report at about 70–80% and stop around 85% of the cap**, so other work and fixes still have room. Close to the stop line, prefer documenting an exception over regenerating.
- Keep a cost log (one line per generation call: time, shot, kind, seconds, units).
- Any real money (API, music licence, stock) needs separate approval.

### 2.2.4 Pilot
Choose the hardest representative shot: the most faces, a screen to composite, the core message. Also run one still test for the riskiest likeness (e.g. the person whose reference photo is smallest). Report:
- keyframe likeness per person (against photo and against sheet);
- per-take likeness min/mean, drift at 25/50/75%, motion p90;
- the composited, exported mini shot with its spec;
- known weaknesses in plain words;
- the usage spent versus estimate.

### 2.2.5 Parallel work
- Independent shots (different setups) can be generated, QC'd and composited in parallel. Use a process pool for QC scripts, or separate workers per group of shots.
- Each worker writes into its own folder and an *inbox* with a README that says PASS / FAIL and why. The integrator only pulls shots whose README says PASS.
- Keep a **gap audit** table during finishing: cut, time, problem, owner, fix, status (✅ / ⏳). Nothing ships while a row is ⏳ unless it is listed as an exception.

---

## 2.14 Final QC checklist

**Plan and process**
- [ ] Plan, cut table, prompts and low/expected/high estimate approved before any generation
- [ ] Pilot done and reported; usage logged; stopped before the usage stop line
- [ ] Every prompt assembled from blocks, with all fixed strings verbatim
- [ ] Gap audit has no open rows (or each open row is a listed exception)

**Staging**
- [ ] Screens dominant; bodies toward the screen; camera beside or behind the screen
- [ ] Faces three-quarter, yaw ≤ 30°; eyelines on screen content, below its top edge
- [ ] Action varied across the film; pointing ≤ 2; no "smile, lean in, point" beat
- [ ] One or two people per shot, lead/support clear; four-person scenes split into pairs
- [ ] Pain-point shots restrained: no smile, no exaggeration, no reaching

**Likeness, motion and physics**
- [ ] Every person scored individually: still ≥ 0.60 (group ≥ 0.45), video ≥ 0.50, drift ≤ 0.10
- [ ] Motion p90 ≥ 1.0 and no still stretch > 0.5 s, on the final shot (not only the raw take)
- [ ] No clipping, extra fingers, morphing, faces cut by the frame edge, or looks to the lens
- [ ] No readable gibberish text anywhere; no model-drawn UI, logos or numbers

**Screens**
- [ ] Corner overlay and corner tiles reviewed for every screen
- [ ] Glass filled with the UI base colour; UI inset ~80% and centred; no blue leak at the edges
- [ ] Occlusion matte keeps hands, arms and heads in front; no UI lines across arms; despill done
- [ ] Drift, aspect, jitter and bezel lock within target; measurement artefacts explained
- [ ] Shading, glow, bloom and grain match the plate
- [ ] Full-screen UI fills the frame, stays clear of the subtitle band, plays at 1× with holds
- [ ] UI animates slowly, one element at a time; every label readable for ≥ 1.5 s
- [ ] Redesigned charts keep the same data, meaning and structure; overload screens still look overloaded

**Edit, audio and subtitles**
- [ ] Subtitles single-line in a fixed band, plus SRT; text cards never on faces or hair
- [ ] No digital silence, gaps ≤ 0.8 s, music under everything, no clipped word edges
- [ ] Pacing within the target rate; no time-stretched VO
- [ ] No key word repeated in consecutive lines; on-screen headers match the VO
- [ ] −16 LUFS, TP ≤ −1.5 dBFS measured after AAC

**Delivery and archive**
- [ ] 1080p24 H.264 High, yuv420p, AAC 48 kHz, faststart, < 100 MB, verified by ffprobe
- [ ] Clean master, SRT, stems, contact sheet delivered
- [ ] Every version kept; version map and storyboard updated; index updated
- [ ] QC report written, with honest exceptions

---

