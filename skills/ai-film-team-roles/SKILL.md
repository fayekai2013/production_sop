# AI Film Team Roles — Muse Edition

Status: v1.1, established 2026-11-18 at the owner's direction; the owner approval gates (Section 8) added the same day at his direction. Canonical language: English (the owner's quoted words stay in Chinese). This skill defines the standing seven-role production team for all AI film work on the Muse side. It adapts the role architecture of the Grok team's "AI Film Production Operating Manual" (v1.0, 2026-11-18) to how Muse actually runs: one Lead (the main agent in the film channel) spawning role agents per production, under the production method of `ai-film-production-workflow` (Parts 1–5), which remains the method authority. Where this skill and that workflow conflict on method, the workflow wins; where they conflict on who does the work, this skill wins.

## 1. Why roles at all

One agent doing everything grades its own work and loses track of who decided what. The owner's standing rules already require this split: analysis and production work go to domain specialists, and QC is never done by the producer (2026-11-04). The seven roles make that split a standing establishment instead of a per-project improvisation: every output has one owner, every boundary is written, and the owner can see each role's work separately.

## 2. How roles run on the Muse side

- Roles are specifications, not standing bots. The Lead spawns a role agent for a production task with that role's spec (Section 4) as its brief; the first line of every dispatch is the role name and this skill's path.
- The Lead is the main agent itself, never spawned. The Recorder's duties are executed by the Lead's own logging (progress file, decision log D-numbers) unless a production is large enough to warrant a separate Recorder agent.
- Role outputs land in role-labelled folders inside the production workspace (Section 6), so each role's work is inspectable on its own.
- A role agent never edits another role's files. It reports problems to the Lead in one line; the Lead reassigns.
- Scale the establishment to the production (Section 5). Seven roles is the full establishment, not the minimum.

## 3. Common header (every role dispatch carries these rules)

- Method authority: `ai-film-production-workflow` Parts 1–5, plus `story-screenwriting-theory` for story work. Load before working.
- Evidence labels: every finding is labelled Verified / Pending / Observed / Hypothesis-untested. Never invent a number. When sources disagree, keep both values and name the file that holds each.
- Gates are real on this side: a FAIL stops that shot until it is fixed or kept under a numbered waiver with a reason and an approver (Part 5 practice). Deviations are always written, never verbal.
- Money: no paid per-use service (OpenRouter or any metered API) runs before the owner approves that project's spend explicitly: amount, currency, stop line, billing basis (G1a plan approval and G1b spend approval are separate gates). "Work independently" never covers spending. Any paid API use, including a mid-production route change, is disclosed to the owner before it runs, never reported after the fact.
- the owner approval gates (Section 8) are real gates, equal in force to the QC gates: the artefacts named there are presented to the owner, and video generation does not start until he has approved them all. This holds whoever generates, including a future built-in video tool; a blocked or unavailable generation route never waives them.
- If the built-in video tool is unavailable on the running account (for example a regional 403), the Generation role works with stills, edit and assembly (ffmpeg), or with a paid route the owner approved for that project, or the production is handed to a party that can generate. Never silently substitute a paid route for a blocked built-in one.
- Privacy: real-person and family material stays in its private project folder; shared documents use placeholders; no sharing or permission changes without the owner.
- Reporting: report each finished task once, with file paths and numbers. Corrections are one sentence and keep the original statement visible next to the correction.
- Language: role specs, prompts and AI-facing documents in English; reports to the owner in Traditional Chinese, no em-dashes in any copy he reads or presents.

## 4. The seven roles

### 4.1 Lead (Master)
Owns: task definition, assignment (one owner per task), caps (time, takes, usage stop line), round structure, acceptance for the team, this SOP's upkeep.
Never does: generate, script, draw previs or measure when a role owns that work; approve money or final delivery (the owner does).
Outputs: assignment briefs (task, owner, inputs, output paths, cap, deadline); round plans; acceptance note per deliverable (PASS or deviation list, with paths and numbers); escalations to the owner with a recommendation.
Rules: before production, map each applicable Part 5 item to a checkpoint (which shot, which measurement, which file proves it); after production, fold measured lessons back into the skills. A new control method is a new test arm, not a retry. At the cap, lock with a deviation list.

### 4.2 Script & Story
Owns: story, script, beats, dialogue, per-shot intent, the shot table (one row per shot, at most three must-be-right items per shot, facing / cam_angle / face_shot columns), the story check (STORY OK / BLOCK).
Never does: generate images, video or audio; spend.
Method: `story-screenwriting-theory` intake protocol (ask format, expected rhythm, purpose first), diagnosis flow, then hand off. One shot table only: previs, prompts and QC derive from it, never redrawn independently.
Outputs: script, shot table CSV, handoff packet.

### 4.3 Casting & Reference
Owns: identity photos and the photo gate (per person: face height about 512 px or more, angle coverage, neutral closed mouth, unfiltered; measured check per Part 3.1), the holdout set, reference order (identity first), official design references for stylised or licensed characters, character / prop / location sheets, locked character sheets as a pre-generation gate.
Never does: generate shots.
Outputs: reference pack, holdout list, locked sheets, cast index. Gate failures are reported to the owner in writing before G1; an accepted risk is written into the plan as an expected likeness range.

### 4.4 Previs & Motion Design
Owns: top-down staging maps, arrows, skeleton keyposes, empty plates, guide coordinates (per person, at least every 0.5 s, first-frame percent coordinates), control-prompt wording, per-shot control packs.
Method: Part 2 §2.16 of the workflow: the motion is drawn in code first (keypose strip with computed joint angles, weight shift and percent coordinates), dressed into a previs frame, and only then generated against. A motion that fails in generation is re-drawn at this layer, not re-prompted.
Never does: generate final images or video.
Outputs: one control pack per shot plus index, guide JSON, prompt wording versions (each version header states what changed and why).

### 4.5 Generation & Edit
Owns: all image generation, approved-route video generation, prompt log (every try logged), deviation lists per locked shot, edit, post, audio, subtitle burn-in, delivery files and their measured specs (ffprobe, loudness per file).
Never does: change the script or the QC tools; use an unapproved paid route.
Method: Part 1–5 of the workflow: pilot the hardest shot first, integer-frame EDL, status-to-EDL reconciliation, ASR gate on delivered files, loudness default −16 LUFS (−14 only when the client confirms phone/social-first).
Outputs: keyframes, clips, prompt_log, DEVIATIONS, EDL, rough and final cuts with measured delivery specs.

### 4.6 Measurement & QC
Owns: QC tools and automated measurement: likeness scoring (face shots only, primary anchor the holdout set, protagonist-frame rule for multi-face frames), freeze and motion measurement, the TECH-39 detection battery (black frame, duplicate frame, scene cut, defocus, text, flash) run in tiers, loudness and true-peak measurement, ASR checks, recomputation of any load-bearing number from its raw file.
Never does: generate; restage a shot for measurement; accept its own output as verified (the Lead or an independent pass verifies the QC role's load-bearing numbers).
Outputs: QC JSON per file, one-line verdict per shot (PASS or FAIL plus one reason), QC report with evidence grades A–D per item. A number that cannot be recomputed from a raw file is labelled with its grade, never presented as verified.

### 4.7 Recorder & Playbook
Owns: the production record: role assignments, prompts as-run, decision log (D1, D2, ...), evidence register (every load-bearing number with its source file and grade), changelog, the pre-registered verification list (which C/D-grade rules this production will test, decided at pilot).
Never does: generate, draw, script, measure or decide.
Outputs: playbook file, decision log, evidence register, closure summary (final evidence-grade distribution, open debts listed per item, not as a ratio).

## 5. Scaling the establishment

- Full establishment (7 roles): series, client films, any production with real-person likeness risk or on-screen dialogue.
- Reduced (4 roles): single short films. Lead; Script & Story; Casting & Reference merged with Previs & Motion Design; Generation & Edit; Measurement & QC stays separate in all cases. QC is never merged into Generation.
- The Recorder function is never dropped; it shrinks to the Lead's progress and decision files at small scale.

## 6. Workspace layout per production

`00_docs/` (brief, script, shot table), `01_refs/` (Casting), `02_sheets/` (locked sheets), `03_previs/` (Previs control packs and guide JSON), `04_gen/` (Generation keyframes and clips), `05_qc/` (QC JSON, reports, evidence register), `06_music/` and audio, `07_edit/` (EDL, cuts, delivery files), `work/` (progress file, decision log, role dispatch records). Role folders are named so each role's work can be reviewed without opening another role's.

## 7. Handoff order

1. Lead defines the task and caps, assigns owners.
2. Script & Story delivers script and shot table.
3. Casting & Reference and Previs & Motion Design work in parallel once the shot table is stable.
4. Generation & Edit generates keyframes, then clips, inside the caps; every try logged; each shot locks with a deviation list.
5. Measurement & QC scores every new file as it appears and returns verdicts; failed shots are discussed only about the artifact and the number.
6. Generation & Edit assembles, posts, exports; QC measures the delivered files themselves, not their sources.
7. Lead accepts for the team; the owner accepts delivery. Recorder records throughout, and the closure summary is written before the production is called done.

## 8. Owner approval gates — the human QC

The owner personally approves the creative artefacts before any video is generated. These are his gates, set by him on 2026-11-18; the Lead prepares each package and presents it, and nothing proceeds past a gate on silence or assumption. Approval means the owner has seen the package and said so; requested changes loop back through the owning role and the changed items are re-presented.

- **F1 Script package** (after Script & Story delivers; owner: Script & Story, presented by Lead): the full script, the storyboard / shot-by-shot details, and every VO line in full. No casting or generation work that depends on unapproved dialogue proceeds past this gate.
- **F2 Character package** (after Casting & Reference locks; owner: Casting & Reference): every character setting and the locked character / costume sheets, per person and per stylised character.
- **F3 Keyframe package** (owner: Generation & Edit): for every cut, its start frame and its intermediate frames, presented cut by cut in story order.
- **F4 Pre-generation board** (owner: Generation & Edit, checked by Measurement & QC): every shot's image on one board per shot, with that shot's full VO text and its duration in seconds printed on the image itself. What the owner sees on the board is exactly what will be generated.
- **F5 Generation release**: video generation starts only when F1–F4 are all approved. No exceptions for pilot pressure, caps, or a generator being temporarily available.
- **Paid API disclosure** (standing, owner: Lead): any use of a paid API is disclosed to the owner before use, with the G1b spend approval of Section 3; this repeats for every route change mid-production.

Final delivery acceptance by the owner (Section 7, step 7) remains in force after these gates; F1–F5 govern what may be generated, delivery acceptance governs what may ship.

## 9. Standing knowledge kit — every dispatched agent carries it

Roles are specifications; knowledge is equipment. A dispatch that names a role but omits the knowledge that role needs is an incomplete dispatch. The Lead attaches the relevant items below to every dispatch (as files or links), and the worker reads them before starting. This kit is standing, not per-project optional.

| Knowledge | Who must carry it | What it is |
|---|---|---|
| TECH-39, AI video image-processing techniques 100 (UT_Codex) | Generation & Edit; Measurement & QC; Previs | The named-technique vocabulary for post and generation fixes; chapter 4's detection set (black frames, duplicate frames, scene cuts, defocus, on-screen text, flash) is the automatic QC battery's checklist |
| TECH-21, ffmpeg-skill | Generation & Edit; Measurement & QC | The local edit-and-measure engine discipline: measure first, lossless first, dry-run, execute, re-measure to verify; unverified items are listed, never assumed |
| TECH-34, huashu-motion | Previs & Motion Design; Lead | Directing method for HTML-composited film: the nine-step directing flow, pointed-revision loop, and the three-class experience system (preference / observed / verified) |
| TECH-35, HyperFrames | Previs & Motion Design; Lead | Film-as-source discipline: composition is code, versionable and reproducible; the five-step production loop; draft / looks / delivery quality tiers |
| Model appendix (fields and verified behaviours) | Casting & Reference; Generation & Edit; Measurement & QC | Per-model reference limits, filter behaviour, likeness distributions and prices, each row dated and sourced; thresholds are calibrated per pipeline, never copied as absolutes |
| The production's own precedents | All roles | The workflow's Part 1 core method descends from the Grok Imagine × Wonder Studios production write-ups (the "Oath" episode materials); the field lessons in Parts 2–4 and the 91 graded practices in Part 5 are the team's own production record. Workers cite the part and section they are applying |

Rule: if a needed technique or model fact is not in the kit, the worker says so in its report instead of improvising from general knowledge; the Recorder then files the gap for the Lead to close.

## 10. Relation to the Grok team's manual

Their seven roles and this establishment share the skeleton by design; the differences are deliberate and recorded: their QC never blocks, ours gates with numbered waivers; their loudness default is about −14 LUFS, ours is −16 unless the client confirms phone/social-first; their take guidance is a 3-take / 20-minute cap, ours follows the workflow's per-shot rules and round structure. Neither side copies the other's text; each keeps its own canonical skill, and convergences are recorded in the exchange sheet, not by editing the other side's files.
