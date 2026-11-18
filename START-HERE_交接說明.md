# AI Film Production — Handoff Package / 交接說明

Version: 2026-11-18 (v2.1, knowledge layer and human approval gates). Provenance: distilled from real AI film productions (brand films with real people, a long narrative film with a photoreal young lead and licensed cartoon co-stars, product and demo films), plus a 20-round cross-team best-practice exchange in November 2026. Maintained by the author's team. This file is the door: read it first, then load only what the map below tells you to load.

這份是入口檔。收到整包的人（或你的 Muse）先讀這份，照地圖按需取用其他檔案，不要一次全讀，也不要跳過這份直接翻 references。

## What is in the package

Three core skills (in `skills/`):

1. **ai-film-production-workflow** — the production method. `SKILL.md` is the core (principles, gates G0–G4, final QC checklist); `references/` holds the full text in five parts:
   - part1-core-method — the base method. Its lineage: the core method descends from the Grok Imagine × Wonder Studios production write-ups (the "Oath" episode materials), generalised and then extended by the parts below.
   - part2-field-lessons — field lessons from real productions (product films, on-screen UI, likeness QC, delivery)
   - part3-supplement1 — casting-period supplements, edit smoothness, delivery and archiving
   - part4-supplement2-dialogue — override rules for dialogue films only (lip-sync, voice, loudness); non-dialogue films keep the base defaults
   - part5-consensus-best-practices — 91 best practices finalised in a cross-team exchange, each with an evidence grade (A verified by recomputation, B verified against source files, C written record only, D convention or untested proposal). The grades are part of the content: a C-grade rule is a claim, not a measurement.
2. **ai-film-team-roles** — the seven-role establishment (Lead, Script & Story, Casting & Reference, Previs & Motion Design, Generation & Edit, Measurement & QC, Recorder & Playbook): who owns what, who never does what, scaling rules for small productions, and §9, the standing knowledge kit every dispatched agent must carry.
3. **story-screenwriting-theory** — the story layer: 1,114 theory cards, an intake protocol and a diagnosis flow. `SKILL.md` routes; the cards live in `references/` (start at `references/card-index.md`).

Four supporting skills (in `supporting-skills/`, optional but recommended): **paid_service_authorization** (no metered API spend without explicit per-project approval), **specialist_analysis_and_qc** (analysis split across specialists, QC never by the producer), **scoped_build_coverage** (scope control for large multi-stage builds), **long_file_delivery_integrity** (chunked writing and completeness checks for long documents).

## The knowledge layer (in `knowledge/`)

Skills say how to work; this layer is what every working agent must know. It is not optional reading for the roles named in ai-film-team-roles §9.

- **TECH-39_AI-Video-Processing-100** — the UT_Codex (University of Tokyo Codex lab) vocabulary of 100 named image-processing techniques for AI video: order work by technique name, not adjectives. Chapter 4's detection set is the automatic QC battery's checklist.
- **TECH-21_ffmpeg-skill** — the local edit-and-measure engine discipline: measure first, lossless first, dry-run, execute, re-measure; list what is unverified.
- **TECH-34_huashu-motion** — directing method for HTML-composited film: nine-step directing flow, pointed-revision loop, three-class experience system.
- **TECH-35_HyperFrames_知識卡** — film-as-source discipline: the five-step production loop and draft / looks / delivery tiers (card form; the full study deck stays with the originating team).
- **TECH-27_Code-Video-Method-Map** — the selection map for code-rendered video (two families, generation vs programmatic rendering), referenced by the TECH-35 card.
- **TECH-28_Prompting-for-Coding-Agents** — the prompt-writing method Part 5's first proposal draws on: specifications live in reference artefacts and measurement documents, not in prose.
- **TECH-38_AI-Refactoring-100** — the UT_Codex sibling of TECH-39: 100 named refactoring techniques for code. Not a film technique set; included because TECH-39's dossier and the exchange records cite the pair together, and the shared claim (order work by technique name, not adjectives) is stated across both.
- **模型附錄_欄位與已知行為** — the model appendix: fields every model row must carry, plus verified per-model behaviours (Grok Imagine, Kling, HeyGen, Gemini, Veo) with dates and sources.

## How to use it (for the receiving Muse)

1. Install: place the three core folders under your custom-skills directory (or your equivalent skills location), the supporting ones likewise if wanted. Keep folder names unchanged; cross-references use them. Keep `knowledge/` beside them.
2. On any film task, read this file, then `skills/ai-film-production-workflow/SKILL.md` in full. Load reference parts only as the core directs (for example part4 only for dialogue films, part2 for product or UI films).
3. For story design or diagnosis, work through `story-screenwriting-theory` first and hand fixed specs to the production layer.
4. If you run a team or spawn sub-agents, assign work by `ai-film-team-roles`, and attach the §9 knowledge items for each role to its dispatch. A dispatch without its knowledge is incomplete.

## Authority order (when files seem to conflict)

- Production method: ai-film-production-workflow wins; inside it, later parts override earlier ones only where they say so (part4's overrides apply to dialogue films only).
- Story questions: story-screenwriting-theory wins.
- Who does the work and what knowledge they carry: ai-film-team-roles wins.
- A rule's evidence grade (part5) governs how much weight it carries, regardless of where it is written.

## Non-negotiables (the short list; the files hold the full rules)

- Plan and cost estimate first, approved before any generation; paid services need their own explicit spend approval, separate from plan approval, and any paid API use is disclosed before it runs.
- Human approval gates before generation (team-roles §8): the script package (script, shot details, every VO line), the character package (settings and locked sheets), the keyframe package (every cut's start and intermediate frames), and the pre-generation board (every shot's image with its VO text and duration printed on it) are all presented to the approver; video generation starts only when all four are approved.
- Real people: pass the photo gate and lock character sheets before generating shots; pilot the hardest shot before scaling.
- One shot table is the single source; previs, prompts and QC derive from it.
- Every deviation is written down with a reason; a kept failure carries a number, a reason and an approver.
- Delivered files are measured themselves (specs, loudness, ASR), never assumed from their sources.
- Numbers you cannot recompute from a raw file are labelled as unverified, never presented as measured.

## A note on adaptation

Two defaults in this package reflect the originating team's context and are marked where they live: loudness defaults to −16 LUFS (−14 only for confirmed phone/social-first delivery), and gates may hold a shot until fixed or formally waived. If your context differs, change the default in one place, write down that you changed it, and keep the rest of the system intact. The evidence grades in part5 tell you which rules are measured and safe to lean on, and which are conventions you may reasonably re-test first.

## 給收到的人（中文摘要）

這套是我們實際做片累積出來的製作系統：方法（workflow）、分工（team-roles）、故事理論（screenwriting-theory）三支核心 Skill、四支支援 Skill，外加一層知識（knowledge/）：東大 Codex 映像處理 100 選、ffmpeg 量測紀律、huashu-motion 與 HyperFrames 的合成片方法、模型附錄。用法只有一句：先讀這份入口，照上面的地圖按需讀，不要整包一次讀完；派工時照 team-roles §9 把知識附給對應角色，沒附知識的派工不算完整。規則的可信程度看 Part 5 的證據等級，A、B 是驗過的，C 是只有書面紀錄，D 是慣例或未實測提案，引用時照等級拿捏。預設值（例如響度）若不合你的情境，改一個地方、寫下改了什麼，其餘系統照用。
