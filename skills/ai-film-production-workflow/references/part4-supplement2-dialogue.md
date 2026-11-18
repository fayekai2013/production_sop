# Part 4 — Supplement 2 (incorporated 2026-11-02)

*Integration note (2026-11-02): the owner provided Supplement 2 on 2026-11-02 (learnings from a long multi-character narrative film with a photoreal child lead, several real cast members, stylised costume transformations, cartoon co-stars and TTS character voices) and directed that it be incorporated into this skill. The supplement text below is incorporated verbatim. Precedence: where a Part 4 section says it "extends", "sharpens" or "changes" an earlier section, Part 4 prevails over the earlier wording. Key scope limit stated by the supplement itself: its overrides to the base defaults (lip-sync gating, ±10% pitch-preserved audio-only retime, −14 LUFS option) apply ONLY to dialogue films with speakers on screen; every film without on-screen speakers, including all product films, keeps the base defaults (closed mouths, no time-stretch, −16 LUFS).*

# Supplement 2: Learnings from a long multi-character narrative film

This supplement continues "ai-film-production-workflow". Part 1 there is the core Grok Imagine "The Oath" method. Part 2 holds the field lessons from earlier productions (2.1 to 2.15): budget and pilot gates, 7-block prompts, staging around screens, likeness QC, motion QC, screen UI compositing, cards and subtitles, VO, LUFS, delivery spec, versioning and the QC checklist. Part 3 (Supplement 1, sections 3.1 to 3.16) holds the next round's learnings. This Part 4 adds only what was new in a long family narrative film with a photoreal child lead and several real cast members, stylised costume transformations, cartoon co-stars and TTS character voices. Rules already in Parts 2 and 3 are not repeated. Each section names the section it extends or changes. People are "child lead", "teen sibling", "parent" or "Person A / B". Licensed characters are "cartoon co-stars" or "the masked character".

**Where Part 4 changes earlier defaults.** These overrides apply **only to dialogue films with speakers on screen**. Every film without on-screen speakers, including all product films, keeps the base defaults: closed mouths, no time-stretch and −16 LUFS.
- 2.5.5 says mouths stay closed and there is no lip-sync. In dialogue films, lip-sync is gated (4.10).
- 2.11.1 item 5 says never time-stretch VO. In dialogue films, a ±10% pitch-preserved retime of the **audio only** is allowed, for lip-sync only (4.10).
- 2.11.7 sets −16 LUFS with TP ≤ −1.5 dBFS after AAC. **−16 LUFS stays the default.** Use −14 LUFS only for phone- or social-first delivery that the client confirms (this round did). 4.7 also tightens true peak to ≤ −2 dBTP on every delivered file. Supplement 1's 3.13 ("re-measure loudness anyway (−16 LUFS…)") follows whichever target the project chose.

Read order: 4.1 to 4.5 cover casting and generation, 4.6 covers the edit, 4.7 to 4.10 cover voice and lip-sync, 4.11 to 4.14 cover picture post, and 4.15 to 4.18 cover process and failures. The checklist additions are in 4.19.

---

## 4.1 Locked character sheets are a gate before any shot regeneration (extends 3.1, 2.2.1, Part 1 §3)

**Why.** Shots of the child lead were made from different reference mixes over several rounds. Every shot ended up with a different haircut, as if from different years of his life. The client caught it only at the fine cut, and about 50 shots had to be redone. A sheet built from an old photo also carried an old body type: a parent came back with a bodybuilder physique copied from an earlier stylised sheet.

**Rules.**
- Build one **locked sheet per real person** from **current photos only**: a face turnaround (front, 3/4, profile, back), a body turnaround, an expression panel (neutral plus the 4 or 5 emotions the script needs), and one sheet per costume state.
- Write a **hair lock** and a **build lock** as fixed strings. Put them word for word into every prompt. An example hair lock: "short dark hair, scissor-cut sides (NOT undercut or fade), soft tousled texture, fringe falling onto the forehead".
- **The client approves the sheets before any face is generated for shots.** Queue the shot work in a file ("NO face generation until the sheet is approved") so that workers do not start early.
- Pick sheets by eye against the lock, not by top score. In this round, an alternative face sheet scored higher (front 0.755 vs 0.726) but had the wrong haircut, so it was rejected.
- Approval can be partial. Replace only the rejected panel (for example one expression cell) and keep the approved panels bit-identical.
- Never feed an old or stylised sheet as a reference for a new sheet. It brings back the old body, costume or crowd.
- After approval, use the locked sheet as **image 1** in every keyframe call for that person (4.4).

## 4.2 Audit every photo ever supplied, per person (extends 3.1.2)

**Why.** The client sent photos in batches over a day: early casting photos, an expression set, then "recent" batches. The first sheets used only the newest batch, and the client rejected them twice ("still not like her"). Looking back over every supplied photo found better references, including a full-resolution original of a low-resolution photo already in use.

**Rules.**
- With each new batch, re-audit **all** photos ever supplied, not only the new ones. Write one table per person (the template is below) and a contact sheet.
- Score every detected face against all the others (leave-one-out cluster mean). An outlier means an old photo, heavy retouching or the wrong person.
- In group photos, crop out the target person and **verify the crop by embedding**. In this round a friend in the same photo scored 0.007 against the target cluster.
- Skip childhood photos of a person who has grown up. Check this with the EXIF date and the cluster score; in this round old photos scored about 0.35 against the current face.
- Choose 3 to 5 references: clear, unfiltered, current, neutral or soft closed-mouth, at least one sharp 3/4, and one **full-body photo for body shape**.
- **Over-beautified failure.** ArcFace does not detect beautification: a rejected "influencer-smooth" sheet still scored 0.73. Add "natural, unretouched skin, real asymmetry, no beauty filter" to the prompt, and have the client check by eye.
- **Too-heavy failure.** "Normal build" produced a sheet the client called too heavy. Write the body type explicitly, e.g. "slim dancer build, narrow face, clear jawline, slim neck, long slim arms and shoulders". Compare face and body side by side with the full-body photo.
- **Score leakage.** When a reference photo is also in the scoring anchor set, the score is inflated. Report a **held-out photo** score as well.

| Photo | Date (EXIF) | Cluster mean | Size / sharpness / yaw | Expression, filter, occlusion | USE (ref letter) / comparison only / SKIP | Reason |
|---|---|---|---|---|---|---|

## 4.3 Likeness bars by shot type, and what lowers the score (sharpens 2.5.2, 3.7)

**Rules.** Score keyframes and videos against an anchor set built from the locked sheet plus the expression photos (use the max over anchors). Calibrate the bars on the locked sheet's own cells before you use them.

| Shot | Bar (ArcFace cosine, median over the take) | Calibration in this round |
|---|---|---|
| Close-up / MCU / medium | ≥ 0.55 | The sheet's expressive cells scored 0.48 to 0.55 against the person's anchors |
| Wide / medium-wide | ≥ 0.45 | Face 66 px on the body sheet scored 0.48 to 0.56 |
| Masked face, or costume hair or wig (transformation) | ≥ 0.45; the score is capped by the costume | The sheet's own masked close-up scored 0.453, so treat about 0.45 as the ceiling |
| Back, silhouette, tiny extreme wide | n/a; check hair, costume and build by eye | 3.7 applies |

- **Shouting and open mouths lower the score.** One keyframe scored 0.553, and its video fell to 0.475 because the face drifted during the shout. Blowing-out and open-mouth frames fell to 0.35. A mask over the mouth gave 0.369, against 0.486 for the same shot with the mouth uncovered. Generate the keyframe **closed-mouth** ("lips pressed", "closed-mouth smile"). Let the video or the lip-sync (4.10) open the mouth, and keep the retake **minimal-motion**.
- **The eye check is mandatory.** ArcFace cannot see that a face has become a cartoon or semi-CG face. Several takes that passed the bar read as "cartoon kid" to the client. On every keyframe and video, check three things: photoreal skin, the locked haircut, and **natural eyes** (normal iris and sclera, no glow, no white-out). Effects such as "eyes start to glow gold" caused glowing eyes twice. Move the effect to the rim light, the hair or the body ("gold light rims his face; eyes stay natural brown").
- **Two copies of the lead.** The model sometimes draws the lead twice, or renders a cartoon co-star as the lead's twin. Add "Only one boy/person in the picture" and reject such takes.
- **Wide shots.** Wide shots of the lead failed again and again ("does not look like him"). Prefer a medium-wide shot with the face at least about 1/8 to 1/10 of the frame width, or add a short MCU insert, rather than chasing the score on a tiny face.

## 4.4 Short keyframe prompt and filter-safe wording (extends 2.3.2, 3.15)

**Why.** Long prompts with three or more real photos were blocked by moderation more often. Short chest-up prompts with the locked sheet plus one or two real photos gave the best likeness.

**Template** (5 references or fewer):
```text
image 1 = locked character sheet, front view; images 2-3 = real photos (3/4, yaw ~20-40°);
image 4 = costume crop for this level; [image 5 = co-star or set plate]
Photoreal live-action cinematic film still, NOT cartoon, NOT stylized, NOT a 3D render, real skin texture.
MEDIUM CLOSE SHOT from the chest up, face large in frame. The boy at FRAME LEFT, face three-quarters
toward FRAME RIGHT, <action>, <closed-mouth expression>. Exactly the face of images 1-3 and the haircut
of image 1. Only one boy in the picture. Natural brown eyes, not glowing. Only the world and the
co-stars are cartoon. 16:9, no text.
```
Add this video suffix to every take: *"His face stays photoreal and identical to the first frame, same haircut."* For a retake after drift, add: "Minimal motion, camera fixed, he stays on the left half of the frame, does not turn around."

**Wording that passes content filters (family or child content):**
- Weapons and hits: write "cartoonish toy revolvers", "huge cartoonish grey hammer head blurs into frame" or "cartoon stars". Do not write "giant sledgehammer swings down at the boy"; that was blocked.
- Effects on a child: "breaks apart into floating colourful pixel cubes" passed, while "shatters" and "magically dissolves" were blocked twice.
- **Guard.** This rewording is only for content that is legitimately allowed and was misread by the filter, such as cartoon props in a family film. If the intended content itself is disallowed, do not reword it to get past moderation; drop or change the beat (the same principle as the child-voice block in 4.9).
- When a shot is blocked twice, **change the action, not the adjectives**. For example, "heroes set the cake down beside the boy" replaced "carry the cake toward the boy". Or use calm wording: "stands tall, one small confident nod".
- Crowds: write "stylised cartoon fans, no human fans, no children in the crowd". Semi-real child extras look uncanny, and the client rejected them.
- Log every block in the cost log, even when nothing was returned (3.15).

## 4.5 Transformations, costumes, masks and fights (extends 2.3.1, 3.3, 3.4)

- **Hair during a transformation.** Only in the transformation shots themselves, and in shots of that costume level, does the lead take the **character's** hairstyle. In every other shot, use the locked hair. The face stays photoreal in both cases. Particle or wind effects must not lengthen or blow the hair: write "hair stays his short haircut; the effect swirls around him".
- **One costume per level.** Every shot of the same costume level uses the same costume sheet crop. Run a costume QC pass over every shot of the level, not only the shot that was flagged. In this round, three shots of one level had a plain hood while three others had a trimmed mask.
- **Masks.** A mask that covers the lower face kills both likeness and lip-sync. Write "costume mask pulled up over the head like a cap, leaving the face uncovered", and check every frame that the mask stays in place (the model removed it in one retake). For the masked character, add "the mask stays on" in every shot until the reveal.
- **Facing the opponent.** In a fight shot, the lead's chest, eyeline and weapon or fist all point at the opponent **in the same frame**. If the opponent cannot share the frame, use an over-the-shoulder shot or a side two-shot. The lead never looks into the lens during a fight. Make a list of every fight cut and check it against this rule.
- **Duplicate props.** Count the hero props in each frame. In one shot a second glowing phone appeared on the bed, and in another the lead held the opponent's hammer.
- **Physics beats** (a ball into a net, impacts): use first- and last-frame control. Make an edited last frame where the result is unmistakable ("ball buried deep in the back netting, net bulging in a cone"). Then generate between the two frames and reject any take where the ball bounces out.

## 4.6 Reconcile the status file with the EDL (extends 3.13, 2.2.5)

**Why.** Three workers appended rows to a shared status CSV (cut, worker, status, chosen take, takes, face QC, costume QC, notes). The EDL builder took the latest row for each cut. When that row had no video yet (a queued redo, a hair-check pending, a keyframe only), the builder fell back to an older take. When a cut had no row at all, it fell back to a reused asset. Several cuts therefore stayed on old takes after better takes existed, and some client picks lived only in a hard-coded dict in the builder.

**Rules.**
- Append to the status file with a file lock. Every chosen take gets a row with a playable path. Client picks are rows (`status=client_pick`), not code.
- On every build, print a **reconciliation table**: cut, EDL source, newest status row, newest existing take on disk, and how the source was chosen (latest / fallback / reuse / client pick). **Fail the build** on any cut where the EDL source is not the newest approved take, unless the cut is listed as an exception.
- List each fallback in the version report so that the client knows which cuts are still on old takes.

## 4.7 Dialogue VO levels, mix and the silent-bus guard (changes 2.11.2, 2.11.7)

**Why.** One fine cut shipped with **no audible VO at all**. In the mix script, a temporary variable for line duration reused the name of the bus-length variable. Every VO placement after that point wrote 0 samples, and no check caught it.

**Rules.**
- **Never reuse a short global name** for bus or buffer sizes (`BUSLEN`, not `L`). Assert after **every** placement that audio actually landed, and assert again on the whole bus:
```python
P('vo', y, t0, gain)
w = buses['vo'][int(a*SR):int(b*SR)]
assert len(w) > 0 and np.sqrt((w**2).mean()) > 1e-3, f'VO {vid} not on the bus'
...
assert np.sqrt((buses['vo']**2).mean()) > 1e-3, 'VO bus is silent'
```
- **Normalise each line to −17 LUFS** (pyloudnorm on the speech span; RMS fallback for lines under about 0.45 s). Cap the gain by the peak.
- **Dialogue first.** Use sidechain ducking keyed from the VO bus (80 ms look-ahead, 25 ms attack, 350 ms release). Then, wherever VO is active, push the whole bed down until the VO sits at least **11 dB** above it (at most 18 dB extra). Hold one steady duck level per line so the bed does not pump inside a line.
- **Master** to the project's target (−16 LUFS default; −14 LUFS only for client-confirmed phone or social-first delivery, as in this round), with **TP ≤ −2 dBTP**. Low-pass the master the way AAC does, so that encoded peaks stay under the ceiling. **Re-measure every delivered file.** The 1080p file (AAC 256k) read −2.0 dBTP, while the 720p re-encode (AAC 192k) read −1.6. Each encode adds its own overshoot.
- After every export, listen to samples from at least three points of the film and run the ASR gate (4.8). A version may not be uploaded until both have passed.

## 4.8 ASR gate on the delivered file (sharpens 2.11.3)

**Design** (export fails and the files are renamed `FAILED_*` if any line fails):
- Run ASR on **each line's window in the final delivered mp4** (onset −0.25 s to offset +0.35 s), not on the clean stems. Use Whisper **medium** with `temperature=0.0`, no sampling fallback and `condition_on_previous_text=False`, so the results are deterministic.
- Compare in **pinyin syllables** (toneless), not characters, so that homophones do not count as errors. Use a **semi-global** edit distance, so that a laugh or a neighbouring line inside the window does not count against the line.
- **Romanised output.** Whisper sometimes writes Mandarin in Latin letters ("Chong'a"). Convert a Latin token to pinyin only if it segments *completely* into valid syllables, so that real English words cannot create a match. Convert digits to number words before comparing.
- **Threshold:** `thr = min(cap, max(floor, baseline + margin))` with cap = floor = 0.5 and margin = 0.35, where baseline is the CER measured on the clean line. Re-measure baselines; never accept manifest placeholders. A line must also pass on its **content syllables** with sentence-final particles removed (啊 呀 了 嗎 吧 呢 …), so that filler such as "嗯 啊" cannot pass a line like "誰啊".
- **Weakness on short lines.** At a cap of 0.8, a single shared syllable passed 4-syllable lines. Even at 0.5, a 2-syllable line passes when one syllable matches, and a stray English word once passed a 4-syllable line. Listen to every line of 2 syllables or fewer.
- **Lines the gate judges by level and duration.** Yelps, grunts, screams and processed roars cannot be transcribed. Put them on an explicit list with the reason and the clean active duration. Each must have VO ≥ bed + 9 dB, audible VO ≥ 60% of the clean duration and at least 0.15 s. Any line that cannot be transcribed and is **not** on the list fails; there are no free passes. Every line must also have VO ≥ bed + 6 dB.
- **Fallback when the mix ASR fails.** Transcribe the clean VO stem in the same window. The line passes only if the clean transcript itself passes the threshold *and* the mix transcript matches it (distance ≤ 0.34). In that case the line is hard for the ASR, not masked by the mix.
- If a line fails even when clean, re-render it or reuse a passing take of the **identical text and voice** from another line. Do not loosen the gate.

## 4.9 Child and teen character voices (extends 2.11.3)

- **TTS voice design may refuse child voices.** The vendor's design endpoint returned a safety block for a child voice prompt. Do not try to get around it. Mandarin voice libraries had no real child voices, only adults doing character voices. Never clone a real child's voice without explicit consent and a recording supplied by the client.
- **Measure F0.** A child speaks at about 230 to 300 Hz and an adult man at about 100 to 150 Hz. The first "child" voice sat at 131 Hz on a calm line, which is why it sounded adult.
- **Pitch and formant shift** (rubberband, or the Praat "Change gender" PSOLA) moves an adult character voice toward a child: +4 st of pitch alone still sounds like an adult vocal tract, and +5 st pitch with +3 st formant sounded the most childlike but risks a chipmunk sound. Apply the same chain to every line of that speaker.
- **Once the client has picked a voice, tune that voice; do not swap it.** Make a short ladder from the approved take: settings (stability 0.25 to 0.40, style 0.45 to 0.60, speed 1.05 to 1.08), then pitch and formant +1, +2 and +3 st, then a bright EQ. Keep the voice and seed fixed. Do not retime lip-sync to new takes until the client has chosen.
- **Audition file.** One mp4 with a **labelled title card** before each variant (ID, voice, settings, shift), then line 1, 0.7 s, line 2, 1 s. Every sample at −17 LUFS. Include the current take as variant 0, and attach a table of F0 and ASR CER per variant. Measurements inform the choice; the client decides by ear.

## 4.10 Lip-sync: exhaustive gate, retime, pilot (changes 2.5.5 for dialogue films)

**Gate.** Audit **every** VO line of **every** character whose mouth is visible, cartoon and masked characters included when a face and jaw are detectable.
- A line **PASSES** when |speech onset − mouth-open onset| ≤ **120 ms** and **≥ 70%** of the speech span has mouth motion. Mouth motion is the inner-lip opening ratio per frame from landmarks, with an adaptive threshold per track and runs of at least 0.12 s.
- **EXEMPT, with the reason listed:** narrator or off-screen voice, inner voice, speaker not in frame, back, hand or silhouette only, no detectable face and jaw, or a face found in less than 50% of the line's frames.
- **WAIVED:** only with a written reason in a waivers file that the client can see. Unwaived failures fail the export.
- **MOUTH_NO_VO:** mouth-motion runs of 0.4 s or more with no VO. These are a review flag, because they read as a character talking silently. In this round's final build the counts were 46 PASS, 38 WAIVED, 77 EXEMPT and 37 mouth-motion flags.
- **Multi-face pitfall.** Picking "the best-passing face track" in a two-shot can pass a line on the *other* person's mouth. Identify each face track by ArcFace (median similarity to each cast member's anchors), and score only the speaker's track.

**Fix order.**
1. **Retime the audio.** Shift the line's lead, then apply a forced tempo of **0.9 to 1.1×**, pitch-preserved (rubberband), so that the speech fits the mouth run. This is **audio only and never applied to picture**: the base rule against slow motion or retiming picture to hide a lack of action still stands.
2. **Re-run the ASR gate after the retime.** Retiming shouted lines broke ASR on 3 of them, and the export failed. Revert those lines' lead and tempo first, then re-gate.
3. **Regenerate the take** with a speaking prompt that has the timing ("speaks one short line toward frame right from 0.8 to 2.4 s"). Prefer this to a lip-sync model.
4. **Lip-sync model**, only after a **pilot of 2 to 3 shots** approved by the client, and only if likeness does not drop. A Wav2Lip pilot lowered ArcFace on all 3 shots (medians 0.428 → 0.404, 0.556 → 0.515 and 0.460 → 0.436; minimums down by 0.04 to 0.05), so it was not applied. Measure ArcFace before and after, plus self-similarity, eye-openness difference and mouth-ratio variance, on every frame.

## 4.11 The opening must not be abrupt (extends 2.10.1)

- The client gave this note twice. Never start on a hard cut into action or dialogue.
- Build an **establishing beat** of about 5 s: fade from black over about 1.25 s (smoothstep), with a slow push-in (in this round a 1.000 → 1.050 digital push toward the subject of the scene). Continue the push into the first real cut **at the same speed** (1.050 → 1.095), with a 0.75 s dissolve, so that it reads as one camera move. A digital push on a static plate costs no generation.
- Fade the music, ambience and SFX in with the picture; the bed reaches full level at about 2 s. **The first VO line starts after the fade.**

## 4.12 Phone and device UI overlays (extends 2.9)

- **Orientation follows the hand.** If the device is held upright (portrait), the UI layout is portrait. One fix put a landscape button layout on an upright phone, and the client caught it. The whole UI had to be rotated 90° clockwise to match the glass.
- **Device screen vs floating HUD.** Decide per element whether it lives **on the device glass** (tracked, perspective-warped, occluded by the finger) or **floats in the world** (a game HUD). The first version of an "accept" button floated upright in mid-frame when it belonged on the phone screen.
- **Tracking a hand-held device.** Annotate the frame-0 glass corners by hand from a gridded still. Then run ECC or optical flow on the bezel and body with a mask that **excludes skin** (finger and hand) and the button itself, and fit a homography per frame. Render the corner overlay (2.9.8) and make a before/after sheet.

## 4.13 Text, subtitles, cards and the storyboard (sharpens 2.10.2, 2.10.3, 2.13.2)

- All titles, subtitles, chapter cards, HUDs and logos are added **in post**.
- Place cards using **face boxes and person-segmentation body masks**. Cards must not sit on a character's body, not only off the face.
- **No em dashes** in subtitles; the client called them an AI habit. Use the language's own punctuation. Write numbers as **digits in one fixed format** (e.g. "LV 1", "LV 2", not number words or Roman numerals). Scan for both and report `dash_or_number_violations = 0`.
- **Side-subtitle fallback.** When every centred slot hits a face (tight close-ups), place a narrower wrapped block beside the face on the clear side. Log each such placement.
- **No duplicate text.** If a chapter card and a subtitle say the same thing, keep one and log the dropped duplicate.
- The key title gets the strongest treatment in the film (size, metallic or glow fill, outline, shadow, an entrance animation). Each effect gets its own SFX, and the title must stay clear of faces.
- **Storyboard / review sheet.** Every thumbnail carries the cut number, duration and start–end timecode **printed large on the image**, plus the subtitle text. Rebuild it from the delivered file on every version.
- **Frame fill.** Scan every rendered clip for letterbox, pillarbox or border bands (rows or columns that are dark and flat), and fail the export on any hit. A generated close-up arrived with a dark border that the client noticed.

## 4.14 Protected-pixel composites, e.g. replacing a crowd (extends 2.5.5, 3.10)

When an approved take has a bad background (semi-real child extras in the stands, for example) and the people in it must not change:
1. Edit frame 0 (background only). Generate a short locked-off video from it and use it **only as a source for the background region**.
2. Build a matte of the region (the stands band minus the foreground scenery) **minus a protection mask** around each key person: GrabCut per person, scaled ×1.25, dilated about 7 px and feathered about 5 px.
3. Composite `out = orig·(1−m) + new·m` (soften the new region slightly to match its depth of field). **Assert max difference = 0 on the protected pixels before encoding.**
4. Disclose the composite in the report and list the residual patches.

## 4.15 Client notes in batches: a decision log (extends 3.12, 3.14)

- Log **every** client note as a numbered decision (D1, D2 …) with a **timestamp** in the client's time zone, the cut numbers it affects and a reference screenshot. The file only grows; superseded decisions are marked, never deleted.
- When the client gives notes in batches, **wait for the "that's all"** before changing anything, so that one pass covers related notes. Meanwhile, generalise each note into a **film-wide rule** (e.g. "the lead is photoreal in every shot, at every size") and list every cut it applies to.
- Keep a separate section of **notes given before and then forgotten**. Re-read the whole log before each version. One note (the abrupt opening) was missed and had to be given twice.
- With every delivery, attach a checklist with one row per decision: done / not done / why.

## 4.16 Budget stop lines, shared usage and resumable workers (sharpens 3.15, 2.2.5)

- The client sets a global usage cap. Give each parallel worker a **stop line a few points below it** (e.g. 68% against a 70% cap). Usage is shared across workers and apps, so read the meter before each slice. Workers stop generating at the stop line and finish with local-only work (mix, subtitles, export).
- Each worker caps takes per cut (2 by default, up to 4 on cuts the client flagged) and logs "take cap reached" when it stops at the cap.
- **Resume file.** Every long job keeps a `work/vX.Y_progress.md`. It holds timestamped DONE lines (what, where the output is, key numbers), failures with their cause, and one **NEXT** line. An interrupted worker resumes from it instead of starting over.
- Before changing scripts for a new version, back them up to `_backup_pre_<version>_<YYYYMMDD_HHMM>/`.

## 4.17 Versions and upload (sharpens 2.13.1, 3.13)

- Name versions `vMAJOR.MINOR` (`<Project>_v4.2_1080p.mp4`, `_720p.mp4`, `_storyboard.pdf`). A failed gate renames the outputs to `FAILED_<name>`; these are never uploaded.
- Upload **every** passing version (the 720p review file, the storyboard and any voice or lip-sync samples) to **both** cloud drives the client uses. Keep the files **unshared**, ask before sharing or moving them, and keep the 1080p master on the box. Log every upload in an upload log.
- Upload only after the VO gate, the lip-sync gate, the border scan and the listening checks have all passed.

## 4.18 Common failures and fixes (extends 2.15)

| Symptom | Likely cause | Fix |
|---|---|---|
| A delivered version has no VO | A variable overwrite set the bus length to zero; no assertion | Unique buffer names; assert after each placement and on the bus; ASR gate on the delivered file |
| Hair differs shot to shot | No locked sheet; mixed old and new refs | Lock the sheet (4.1); image 1 = sheet; hair lock string |
| The lead looks like a cartoon but passes ArcFace | The score does not see stylisation | Eye check; "photoreal, NOT cartoon"; real photos as refs 2 and 3 |
| Glowing or white eyes | Power-up effect worded on the eyes | Put the effect on rim light, hair or body; "natural eyes, not glowing" |
| Prompt blocked twice | Violent verb, long prompt, 3+ real photos | Cartoonish props, calmer action, 1–2 photos, short prompt (allowed content only, 4.4 guard) |
| A cut still uses an old take | The latest status row has no video, so the builder fell back | Reconciliation table; fail the build on a stale source |
| The lip-sync retime breaks ASR on a shout | Tempo or lead change smears onset consonants | Re-gate after the retime; revert the line; reuse an identical-text take |
| Lip-sync passes on the wrong face | Best-passing track chosen in a two-shot | Identify tracks by ArcFace; score only the speaker |
| The lip-sync model lowers likeness | Mouth inpaint re-renders the face | Pilot 2–3 shots; apply only if ArcFace does not drop |
| UI on the phone is turned 90° | Layout chosen without checking how the phone is held | Match the UI to the device orientation; tracked quad |
| 720p TP over the limit while the 1080p file passes | AAC re-encode overshoot | Measure TP on every delivered file; keep master TP ≤ −2 dBTP |

## 4.19 Additions to the final QC checklist (extends 2.14, 3.16)

**Replaced base items (dialogue films only).** The base checklist rows "no time-stretched VO" and "−16 LUFS, TP ≤ −1.5" are replaced, for dialogue films only, by the lip-sync retime row and the loudness row below. All other films keep the base rows.

**Casting and sheets**
- [ ] Every photo ever supplied is audited per person (cluster score, date, size, sharpness, yaw, filter); group crops verified by embedding
- [ ] Locked sheets (face, body, expressions, costumes) approved by the client before any shot regeneration; hair and build locks in the fixed strings
- [ ] Over-beautified and wrong-build sheets checked by eye; held-out photo score reported

**Generation**
- [ ] Likeness bars met: close/medium ≥ 0.55, wide ≥ 0.45, masked or costume hair ≥ 0.45 (calibrated); misses listed
- [ ] Eye check on every take: photoreal, locked haircut, natural eyes, only one copy of the lead
- [ ] Character hair only in transformation shots and that costume level; one costume per level; masks never over the mouth
- [ ] Every fight cut: body, eyeline and weapon toward the opponent; no looks into the lens
- [ ] Every content-filter block logged; reworded by action, not by adjectives, and only for allowed content

**Edit and status**
- [ ] Reconciliation table: every EDL source is the newest approved take; fallbacks listed
- [ ] Opening: fade from black plus a push-in establishing beat; audio fades in; first VO after the fade
- [ ] Device UI matches the device orientation and is tracked on the glass; HUD vs device decided per element
- [ ] Cards off faces and bodies; no em dashes; numbers in one format; no duplicate card/subtitle text; side placements logged
- [ ] Border scan = 0 hits; protected-pixel composites have max difference 0 and are disclosed
- [ ] Storyboard thumbnails show the cut number, duration and timecode in large type

**Audio**
- [ ] VO bus assertions pass; listened at 3 or more points of the delivered file
- [ ] Lines at −17 LUFS; VO ≥ bed + 11 dB target (≥ 6 dB hard); master at the project target (−16 default, −14 only if client-confirmed), TP ≤ −2 dBTP measured on **each** delivered file
- [ ] ASR gate on the delivered mp4: 0 fails; level-checked lines listed with reasons; lines of 2 syllables or fewer listened to
- [ ] Lip-sync gate: 0 unwaived failures; retime is audio only, ±10%, re-gated by ASR; waivers written; mouth-motion-without-VO flags reviewed; speaker track identified

**Process**
- [ ] Every client note logged as a timestamped decision; earlier notes re-read; checklist with one row per decision attached
- [ ] Per-worker stop line respected; progress file current; scripts backed up before the version
- [ ] Passing version uploaded unshared to both clouds; failed outputs named `FAILED_*` and not uploaded
