# QC Report — SKILL.md Assembly (Story & Screenwriting Theory Bot)

- Verifier: independent QC (verifier role; did not participate in writing the Skill)
- Object: `<package>/skills/story-screenwriting-theory/SKILL.md` — 384,663 B, 2,296 lines
- Standard: `build/assembly_intent.md` (completeness judged against intent, not against any copy)
- Date: 2026-11-09
- Method note: wherever the dispatch asked for sampling (3 cards per package, 2 breakdown rows per package, 10 paths), this pass ran the full population instead — all 1,114 index entries, all 212 breakdown rows, and all 85 distinct paths in the file were machine-compared against the source bodies on disk. Sampling figures below therefore read as full-set results.

## Overall verdict: **Pass with minors** — blocker 0, minor 2, observation 2

---

## Item 1. Index coverage re-counted from disk — Pass (1 minor, see m1)

- Source recount: 58 `theory_cards_en.md` packages under `theory/`, card-heading count per package re-extracted from the card faces. Total **1,114** — matches the claimed total exactly.
- SKILL §4 recount: 58 package headings with entry counts; **per-package counts match the disk recount for all 58 packages, zero mismatches**; §4 card entries total **1,114**.
- Full-entry comparison (all 1,114, not a sample): card ID, title, evidence grade, flags, and path compared field-by-field against the card face (grade from the card-face Evidence grade line; flags = literal presence of EVIDENCE_THIN / SINGLE_SOURCE / PENDING TRIANGULATION / combined basis / transmitted in the card block). Result: **zero ID, grade, flag, or path mismatches**; one title-field defect (m1, CV-C19).
- CK-C17: SKILL entry reads "grade ungraded", which matches the chekhov package `qc_report.md` record ("CK-C17 carries no grade (specification)" / "CK-C17 ungraded (specification)"). Pass.
- WE-01: **0 occurrences** in SKILL.md — the worked example does not appear in the index. Pass.
- Combined-basis flags verified in place where §5.3.3 requires them: CL-C14 and CL-C18 entries both carry "flags: combined basis".

## Item 2. Special assets — Pass

All counts re-derived from the source bodies (not from any inventory file):

| Asset | Source recount | SKILL §4 | Result |
|---|---|---|---|
| Glossary terms (`glossary_en_zh.md` rows) | 58 | 58 entries, EN/ZH/grade and semantics first sentence match | Pass |
| Action verbs (MH-C22 30 + MH-C23 30) | 60 | 60, identical order | Pass |
| Murch register | R01–R16 continuous | R01–R16, rule text / evidence tag / card match the register | Pass |
| Mechanism map | 18 nodes (AM-N01–N18), 35 edges (AM-E01–E35) | 18 / 35, node names and edge relation / grade / status match | Pass |
| Breakdown rows (12 × `breakdown_en.md`) | 212 (documented 126 / reconstructed 86) | 212 entries | Pass |

Breakdown rows, full-set check (all 212, not 2 per package): row IDs and order match per package; per-package counts match (17/21/19/32/14/16/14/14/18/15/18/14); provenance strings are **verbatim for all 212 rows**; documented vs reconstructed counts match per package (e.g. tarkovsky 14/1, hitchcock 14/18) and are never merged or relabelled — the two categories are counted separately everywhere. Row labels: 43 rows carry the full source label; 169 carry the `[cut at char 80; full length N chars]` marker — for all 169, the printed prefix is exactly the source label's first 80 characters (137 at 80 chars; 32 at 79 chars where character 80 is a space, trimmed) and the stated full length equals the source label's true length in every case. Tarkovsky rows carry "grade: B" (all other packages "grade: none") — this is correct, not an inconsistency: `tarkovsky/breakdown_en.md` itself assigns a by-row grade distribution ("B: 15 rows, every row…"), and no other package's breakdown file does.

## Item 3. §5 rules in place — Pass

- Psych triangulation: §5.1 carries **17 rules**, compared one-by-one against §3 "Consolidated Bot usage rules" of `Psych_Triangulation_EN.md` — same order, same adjudication mapping (T-01…T-08), **no ruling direction reversed**. Expansions in the SKILL wording (e.g. the p.177 detail in rule 11, the Gallese clause in rule 13) trace to the source item bodies, not to invention.
- Theory × Reception: §5.2 carries **R1–R8**, compared against "Consolidated Bot rules arising from this triangulation" — all eight present, directions preserved, item citations (T-numbers) consistent.
- The five spot checks all pass:
  1. Obligatory scene, level-separated (§5.1 rule 16): analysis = Egri causal direction (effect/diagnostic), production check = McKee debt model, cross-citation prohibited — exactly the T-07 ruling read in the source body.
  2. Climax ≠ memory point (§5.2 R1): corpus basis stated as **281 scene traces vs 102 quote traces across 984 items (28.6% vs 10.4%, ratio 2.75:1)** — all five figures verified in the source body (T1/T6b), including Omaha's 22 scene rows.
  3. Blade Runner barred as exposure evidence (§5.2 R5): stated with the version/layer collinearity reason (41 items) — matches T7b (INSUFFICIENT EVIDENCE).
  4. MM-13 (§5.2 R3): carried as untested hypothesis only, never supported and never refuted by the corpus — matches T4.
  5. KZ-03 / MD-03 decoupled from quotability (§5.2 R2): quote rates measure detachability, taxonomies are assessed by in-film analysis — matches T3a (IRRELEVANT).
- §5.5 adjudication indexes: Psych T-01–T-08 one-liners and the 22 reception verdicts (SUPPORTS 10 / COUNTEREXAMPLE 4 / IRRELEVANT 4 / INSUFFICIENT EVIDENCE 4) match the source verdict summary table item for item.

## Item 4. Completion-era rulings — Pass

Each §5.3 statement was read against the card face:

- **Five engines (§5.3.1 vs XC-C16):** conflict named as one engine among five (conflict / turn / rhythm / harmony-and-situation / commentary voice, same order and glosses); the knot (XC-C11) recorded as a retention device, not a sixth engine; the card's own limits carried verbatim in substance — "no empirical audience-effects research on any of the five engines was found or claimed in this package; this synthesis is a framework verdict, not an effects finding"; grade B and the SYNTHESIS / not-a-single-source-conclusion status both stated. No strengthening, no weakening.
- **Kuleshov (§5.3.2 vs PK-C10):** two layers kept separate — Layer 1 historical demonstration = unverifiable report, citable only as a workshop story with named variants/status; Layer 2 general proposition = supported only in qualified, conditional form (low-clarity face, film-built sequence, one-event reading), strong legend form failed its closest replication; the (a)–(e) Bot rule reproduced in full; grade B. Faithful.
- **Colour (§5.3.3 vs CL-C11 + source_map A-key):** generate with Itten / test with Albers / effect claims only via the audit cards (CL-C12 method, CL-C13–C20 verdicts); the CL-C12 stimulus-specification test (hue, chroma/saturation, lightness/brightness; association vs felt state vs behaviour) present; the audit A-key limit and the combined-basis markings on CL-C14 / CL-C18 carried. Faithful.
- **Composition strong claims (§5.3.4 vs CM-C14/C15/C16):** rule-of-thirds strong version = contradicted ("strongly contradict", Palmer et al. 2014), weak version retained as serviceable default, centred composition not an error; golden-ratio historical version = does not survive measurement (Markowsky 1992, Livio 2002 converging); preference version = no special ratio found (Green 1995 method-sensitive at most; Davis & Jahnke 1991 unity-ratio preference; Elliott et al. 2015 null). All three at grade B, all stated at card strength — the audited claims are refused, not softened.

## Item 5. Unadjudicated register — Pass

- §5.4 contains the Psych residuals **R-1…R-6** (as U-01…U-06), each matching §4 of `Psych_Triangulation_EN.md` in substance (four parameters UNVERIFIED; gatekeeper open; mood fork open; two Egri forks unadjudicated; Tan thin-apparatus ceiling; no factor validation).
- §5.4 contains all **6 PENDING mechanism-map edges** (U-07…U-12 = AM-E08, AM-E09, AM-E10, AM-E21, AM-E25, AM-E33), matching the source edge table's PENDING set exactly.
- Whole-file context grep for every appearance of these items: all other occurrences are (a) §4 index entries carrying the PENDING TRIANGULATION flag itself, (b) the mechanism-map artefact index listing status PENDING TRIANGULATION, (c) §5.1 rule 2 attaching the R-2 caveat, (d) §5.5 T-01 noting the gatekeeper question "stays open as R-2", (e) honesty clause 7.9. **No occurrence anywhere cites a pending item as settled.**

## Item 6. §2 intake — Pass

- The three questions are present in §2.2 in the Study Plan §9 form: film kind/format (feature, short, advertising film, web film, trailer, or another format — option list identical), expected rhythm, purpose; hard-gate recording rules attached.
- Strata rule (§2.4): advertising runs on the AF base (`adfilm_research_base.md`, AF-01–AF-46 — range verified in the file) plus the AF-S supplement (`ad_industry_sites_awards.md`, AF-S01–AF-S34 — verified); web film runs on `webfilm_research_base.md`. The prohibition on extending feature-film theory by analogy is stated in so many words for both strata, and §2.5 forbids filling a missing foundation by borrowing.
- Source fidelity: the v1.4 building rule quoted in §2.4 is verbatim from Study Plan §8; the the owner ruling quoted for the advertising stratum is verbatim from the head of `adfilm_research_base.md` ("built from scratch out of advertising science's own primary literature and industry research… may not reason by analogy from cinema"); the §8 success-criterion contrast (advertising = remembered + brand attitude; web film = completion + sharing) matches the Study Plan text. No distortion found.

## Item 7. §7 honesty clauses — Pass

- All three required clauses are present and operative: 7.1 never claim a book was read when it was not (status = whatever the Source Index records; public-domain full-read exception named and bounded); 7.7 every citation names its version referent (with the Blade Runner collinearity example and decomposed-statement rule); 7.8 breakdown provenance — documented vs reconstructed never conflated, categories counted separately, unviewed-film disclosure preserved, blank values never filled.
- No contradiction with actual practice in §4/§5: §4 counts documented/reconstructed separately (126/86) and reproduces provenance verbatim; §5.4 marks unadjudicated items as such; §5 rules carry their source files and state that the source governs on conflict.

## Item 8. Completeness against the intent list — Pass (see O1)

- Sections: front matter (name `story-screenwriting-theory` + description) ✓; §0 organisation ✓; §1 position/trigger/handoff ✓; §2 intake hard gate ✓; §3 diagnosis workflow with the canonical R1–R15 route table (names identical to the intent list, and identical between §3 and §4) ✓; §4 full index + artefact indexes ✓; §5 hard constraints incl. unadjudicated register ✓; §6 output specification (Traditional Chinese explanations / English canonical; fixed four-part delivery format) ✓; §7 nine honesty clauses ✓; §8 knowledge-base map and maintenance ✓. Every section carries substantive content, not headings alone.
- Ending: the file ends with §8's final line stating that this file is the canonical English Skill and that its §4 index is regenerated from the card packages with the scripts named in §8 (`build/extract_inventory_full.py`, `build/extract_breakdown_rows.py`) and re-verified after every regeneration — the intent list's planned final line. No trailing placeholder.
- Forbidden strings: `truncated` 0, `TODO` 0, `placeholder` 0 (case-insensitive, whole file). The R15 `[cut at char 80; full length N chars]` strings are the legal index-truncation markers verified in Item 2, not truncation damage.
- Head/middle/tail of the delivered file itself were read for this check (front matter + §0/§1, §4 artefact sections and §5 mid-file, §7 tail + §8 ending).

## Item 9. No fabrication beyond the sources — Pass

- §1: the layer relation (theory layer above `ai-film-production-workflow`, handoff as a filled specification), trigger conditions, and the 1,114-card knowledge-base claim all match the Study Plan framing and the disk recount.
- §3: fixed sequence diagnose → retrieve → options → decide, technique-first prohibition, and the R15-is-evidence-only routing rule are internally consistent with §2/§4 and introduce no factual claims about the base that the base contradicts.
- §6: bilingual division and precedence (English canonical governs) match the Study Plan language rule as cited.
- Paths: **all 85 distinct `~/…` paths referenced in SKILL.md were checked on disk — 85 exist, 0 missing** (covers every §8 path and every §4 entry path, exceeding the 10-path sample).

---

## Minors (fix at the stated point; neither blocks use)

- **m1 — §4, CV-C19 entry (SKILL.md line 321): duplicated grade marker.** The entry reads "…The Jungian Foundation: Archetypes and the Collective Unconscious [Contrast] — grade C — grade C — flags: none…". The card-face heading itself ends in "— grade C" (the only card in the base whose heading carries its grade), and the index generator appended its own grade field. Fix: in that one entry, drop the trailing "— grade C" from the title segment (keep the grade field), so the entry reads "[CV-C19] The Jungian Foundation: Archetypes and the Collective Unconscious [Contrast] — grade C — flags: none".
- **m2 — §4, XC-C16 entry (SKILL.md line 1708): the card's SYNTHESIS flag is not on the entry.** The card face carries "Flag: SYNTHESIS, NOT A SINGLE-SOURCE CONCLUSION", and §0/§4 promise each entry carries its flags on the entry itself; the entry currently reads "flags: none". §5.3.1 does carry the warning in full, so the constraint is operative — this is an index-entry completeness defect only. Fix: change that entry's flags field to "SYNTHESIS, NOT A SINGLE-SOURCE CONCLUSION". (This is the only card in the base with a flag outside the extraction list EVIDENCE_THIN / SINGLE_SOURCE / PENDING TRIANGULATION / combined basis / transmitted.)

## Observations

- **O1 — File size.** 384,663 B = 375.6 KiB, inside the intent estimate of 200–380 KB on a KiB reading (1.2% over only on a decimal-KB reading). The size is fully accounted for: §4 alone is ~300K characters, and every index entry in it was verified against disk with zero padding, duplication (beyond m1's five words), or filler found. Judged: index completeness, not inflation. No action.
- **O2 — R15 cut-marker semantics.** 32 of the 169 cut labels print a 79-character prefix because character 80 of the source label is a space (trimmed). The marker text "cut at char 80" remains accurate as a rule (cut at 80, trailing space trimmed); recorded here so a future reader does not mistake the 79-char prefixes for a defect.

## Verdict restated

**Pass with minors.** Blocker 0. Minor 2 (m1 line 321, m2 line 1708 — both one-line index edits). Observation 2. All nine dispatch items were checked against source bodies on disk; the index, the special assets, the triangulation rules, the completion-era rulings, the unadjudicated register, the intake strata, and the honesty clauses are faithful to their sources.

## Correction record (coordinator, 2026-11-09)

Both minors fixed at the point named, in SKILL.md and in the corresponding build parts (build/v2_index_a.md, build/v2_index_c.md) so the parts and the deliverable do not drift:

- m1 (CV-C19): the card heading itself carries a trailing "— grade C"; the index entry's own grade column duplicated it. Entry now reads title (without the heading's grade tail) — grade C — flags: none. Verified: "grade C — grade C" zero hits in SKILL.md and in build/v2_index_a.md.
- m2 (XC-C16): flags field changed from "none" to the card face's verbatim flag SYNTHESIS, NOT A SINGLE-SOURCE CONCLUSION (SKILL.md L1708; §5.3.1 already carried the flag in full). Verified in SKILL.md and build/v2_index_c.md.

Post-fix state: SKILL.md 384,688 bytes; grep truncated / TODO / placeholder zero hits; final line unchanged (canonical English Skill statement). Verdict stands: Pass with minors, minors cleared. Coordinator read-back of head / middle / tail completed before the verifier pass and repeated on the two corrected lines after the fix.
