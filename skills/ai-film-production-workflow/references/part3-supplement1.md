*Relocation note (2026-11-10): this file is Part 3 of the original SKILL.md, moved verbatim into `references/`. Its sections extend Part 2 sections: §2.1, §2.2 and §2.14 now live in the main `SKILL.md`; all other Part 2 sections live in `references/part2-field-lessons.md`.*

---

# Part 3 — Supplement 1 (incorporated 2026-11-01)

*Integration note (2026-11-01): the owner provided Supplement 1 on 2026-11-01 (source file: Drive "AI Film Production Workflow Supplement 1.md", learnings from a real production round, Automation Video V4 to V5, 2026-10-31) and directed that it be incorporated into this skill. The supplement text below is incorporated verbatim. Precedence: where a Part 3 section says it "sharpens" or states a "Change to" an earlier section, Part 3 prevails over the earlier wording. In particular, section 3.7 changes 2.4.1: three-quarter faces with yaw ≤ 30° are the default for likeness-critical close shots only, not for every shot. Section 3.1 is a hard gate at casting that applies before G1 plan approval.*

# Supplement 1: Learnings from Automation Video V4 to V5

This supplement continues the skill "ai-film-production-workflow" (Part 1 core method, Part 2 sections 2.1 to 2.15). It adds Part 3. Each section says which original section it extends. Rules already in the original are not repeated unless they are sharpened here. Examples and numbers come from a real production round (V4 to V5, 2026-10-31) but are written generically; person names are replaced by "Person A / B".

Read order: 3.1 is a hard gate at casting. 3.2 to 3.7 apply when planning and staging. 3.8 to 3.11 apply in comp and edit. 3.12 to 3.15 apply to process, delivery and reporting.

---

## 3.1 Reference photo quality gate at casting (extends 2.2.1 G0/G2, 2.2.2 risks, 2.2.4 pilot, 2.5)

**Why.** Likeness is capped by the reference photo. No prompt, pin or face edit can recover identity detail that the photo does not contain. In this round, one cast member's only photo was 244×337 px, frontal and smiling. Her likeness stayed at insightface cosine about 0.22 to 0.31 across the film, against about 0.6 to 0.8 for cast members with good photos. Face-only edits to "fix" her made it worse (each edit lowered the score further). Upscaling the photo (e.g. 4×) does not add identity detail either. The problem was knowable on day one and should never be discovered at final QC.

**Rule.** Before any generation, at the casting and character-sheet stage, check every real person's photos against the spec below, run the measured check, and report failures to the client in writing. The plan (G1) cannot be approved while a photo risk is unresolved and unrecorded.

### 3.1.1 Minimum photo spec (per real person)
- Face at least about 512 px tall in the photo (ideally from a source of 1024 px or more on the face's long side).
- Sharp and in focus on the eyes; no motion blur; no heavy compression.
- Even, soft light; no hard shadow across half the face; no strong colour cast.
- Neutral expression, mouth closed, not smiling. A smile changes cheeks, eyes and mouth shape, and the model then carries it into every shot (bad for pain-point shots, see 2.4.3).
- Four views: frontal, 3/4 left, 3/4 right, profile. The film will show 3/4 and profile angles (3.7), so the model needs them.
- No beauty filters, skin smoothing or face-shape retouching.
- No occlusion: no glasses unless the person always wears them, no hair over the eyes, no hands or mic near the face.
- Hairline and ears visible.
- Recent (same hairstyle, weight and glasses as on the shoot date).
- Wardrobe or signature details the film will use (watch, pin, glasses) photographed too, with the side noted (left or right wrist, lapel).

### 3.1.2 Measured check (run it, do not eyeball)
1. **Face box size**: detect the face (insightface `buffalo_l`) and record the box height in px. Flag below 512 px.
2. **Sharpness**: Laplacian variance on the face crop resized to 512 px tall. Calibrate on known good photos; as a start, flag below about 100.
3. **Pose coverage**: record yaw per photo from the detector pose. Confirm there is at least one photo near 0°, one near ±30 to 45° on each side, and one near 90°.
4. **Consistency**: cosine between the person's own photos. A low value (below about 0.5) means an old photo, heavy retouch or the wrong person.
5. **Test generation**: build the character sheet and 2 to 3 test frames (frontal and 3/4) from the photos. Score them against the photos. Pass if the median cosine on frontal and 3/4 test frames is at least about 0.5 and the photo-vs-sheet ceiling (2.5.1) is at least about 0.6. Flag anything below.
6. Write one row per person: px height, blur score, views present, expression, occlusion, consistency, test median, PASS / RISK / FAIL.

```python
import cv2, numpy as np
from insightface.app import FaceAnalysis
app = FaceAnalysis(name="buffalo_l", providers=["CPUExecutionProvider"])
app.prepare(ctx_id=-1, det_size=(640, 640), det_thresh=0.4)

def photo_check(path, min_face_px=512, min_blur=100.0):
    img = cv2.imread(path); faces = app.get(img)
    if not faces: return dict(file=path, verdict="FAIL", reason="no face")
    f = max(faces, key=lambda f: (f.bbox[2] - f.bbox[0]) * (f.bbox[3] - f.bbox[1]))
    x1, y1, x2, y2 = [int(v) for v in f.bbox]; h = y2 - y1
    crop = img[max(0, y1):y2, max(0, x1):x2]
    crop = cv2.resize(crop, (int(crop.shape[1] * 512 / crop.shape[0]), 512))
    blur = cv2.Laplacian(cv2.cvtColor(crop, cv2.COLOR_BGR2GRAY), cv2.CV_64F).var()
    flags = [n for n, bad in [("face<512px", h < min_face_px), ("blurry", blur < min_blur)] if bad]
    return dict(file=path, face_h=h, blur=round(blur, 1), yaw=round(float(f.pose[1]), 1),
                emb=f.normed_embedding, flags=flags, verdict="RISK" if flags else "PASS")

def consistency(results):                       # cosine between a person's own photos
    E = np.stack([r["emb"] for r in results if "emb" in r])
    S = E @ E.T; return float(np.median(S[np.triu_indices(len(E), 1)])) if len(E) > 1 else None
```

### 3.1.3 When a photo fails
- Tell the client **immediately and in writing**, before the shoot plan is approved. Say which photos are needed and why, e.g.: *"For Person A we only have one small, smiling, frontal photo (244×337 px). At this size the model cannot hold her likeness (test score 0.3 vs 0.6 for others). Please send 4 photos: frontal, 3/4 left, 3/4 right and profile, neutral expression, face at least 512 px tall, even light, no filter, hairline and ears visible, taken recently."*
- Record the risk in the plan (2.2.2 risks) with the measured numbers and the mitigation: wait for photos, reduce that person's screen time, keep them in 3/4 or profile and at medium size, or recast.
- If the client accepts the risk, write the expected likeness range into the plan and the final QC report, so it is a known exception (2.1.11), not a surprise.
- Do not try to rescue a bad photo with chains of face-only edits. Each edit re-renders the face and drifts further from identity. If an edit is needed, do at most one, from the best source frame, and re-score it.

---

## 3.2 Set bible and set continuity (extends 2.3.1 fixed strings, 1 / 4 locations)

**Why.** Across concepts and retakes, the model changed the set: one monitor in one shot, two in the next; flat in one, curved in the next; a page moved sides; a bezel logo appeared. Each change reads as a continuity error at the cut, and a monitor-type change late in the round forced a whole set of shots to be converted.

**Rules.**
- Lock a **set bible** before keyframes: desk layout, monitor count and model (flat or curved, size, bezel colour, no logo), keyboard and mouse sides, prop list and positions (papers, cups, plants), which side of each person the monitor is on, window and light direction.
- Decide monitor count and type once for the whole scene. **Never mix one and two screens, or flat and curved, across adjacent cuts.** If the decision changes, convert every shot of the scene, not just one.
- Make one **set reference image** (an approved wide of the set) and pass it to every image call with a stated purpose (2.3.2: "use ONLY for set, monitor, props and light").
- Add a set line to the fixed strings, repeated verbatim: e.g. *"exactly ONE flat monitor with a thin black bezel and no logo, on the desk to the person's right; the printed page lies left of the keyboard."*
- QC each keyframe against the bible: monitor count, type, side, props, logos. Log it as a column in the per-shot QC table.

---

## 3.3 Screen direction, the 180° line and joins (extends 2.4, 2.10.1)

**Why.** Several opening versions flipped direction between shots: the monitor at frame right in shot N and frame left in shot N+1, eyelines reversing, a person standing at the end of one shot and sitting at the start of the next. The cuts read as jumps and had to be restaged. Mirroring in post would fix direction but moves signature details to the wrong side (3.4).

**Rules.**
- Draw a **floor plan per scene**: people, desk, monitor, camera positions per shot, and the 180° line (the line through the people and the screen they look at). Keep every camera on one side of it unless a shot visibly carries the camera across (a truck or dolly).
- Plan direction across the whole sequence, not shot by shot: who is at frame left or right, which way each person looks, standing or sitting.
- **The last frame of shot N must match the first frame of shot N+1** in position in frame, pose (standing or sitting, hand positions) and direction of look.
- Before generating any video, build a **join strip** for every cut: the END keyframe of shot N and the START keyframe of shot N+1 side by side. Review all strips as one sheet. Fix flips at the keyframe stage, where it is cheap.
- Adjacent shots of the same subject must differ by at least one shot size or by at least 30° of camera angle. Smaller changes read as a jump cut (in this round a cut with a 1.13× scale change and a small shift between nearly identical setups read as "the camera got bumped").
- For a push-in planned as START and END keyframes, the END must be tighter than the START. A wider END makes the move run backwards.
- Cheapest fixes for a found flip, in order: cut on a motion beat so the move carries the eye; insert a neutral cutaway (hands, keyboard, page close-up) to reset geography; restage the shot (needs new keyframes and approval). Mirror only when no signature detail is readable.

---

## 3.4 Wardrobe and prop side logic (extends 2.3.1 signature detail, 2.5.5)

**Why.** A "fix" moved a watch to the wrong wrist. The edit reasoned in screen space ("the near wrist") instead of anatomy. Another frame came back with a watch on both wrists.

**Rules.**
- Store every side-specific detail in anatomical terms in the cast block: "watch on the LEFT wrist", "pin on the LEFT lapel".
- Before an edit, work out the anatomy for the shot: when a person faces frame left, their left side is nearest the camera; when they face frame right, their right side is nearest; from behind, their left is on frame left. Write the result into the prompt in both terms: *"the watch is on his LEFT wrist, which is the far wrist in this view."*
- Also state hand roles, e.g. "left hand on the mouse, right hand on the page", so a side fix does not swap hands.
- After the edit, verify against the character sheet: count the item (exactly one), check the side, check nothing else moved (e.g. a page dropped from the desk during an inpaint).
- Mirroring a shot flips every side-specific detail. Treat a mirror as a continuity change, not a free fix.

---

## 3.5 BEFORE / NOW split screens (extends 2.4, 2.5)

**Rules.**
- Both halves use **the same camera position, lens, head size and eye-line height**, so the only differences are the states.
- The states must differ visibly: posture (slumped vs upright), action (rubbing brow vs explaining), desk (cluttered vs tidy), light (flat vs warm), screen content (overloaded vs calm). **The same pose with only a different expression is not a contrast.**
- Use 3/4 views for both halves to keep likeness.
- Build one half first (e.g. NOW) and derive the other with a minimal edit, so set and framing match.
- Measure: ratio of median head heights (target about 1.00) and difference of median eye-line y (target under about 1.5%). Expect per-frame values to vary because of the acting (in this round 0.92 to 1.15 per frame, median 1.006). Match on the median and **keep the camera locked**; do not head-track the framing to force a per-frame match, it makes the camera float.
- Keep both faces and hair clear of the divider and the labels (3.6, 2.10.2). Score likeness and motion per half; a whole-frame motion number can hide one frozen half.

---

## 3.6 Framing safety: headroom and edges (extends 2.7, 2.10.2)

**Rules.**
- No face or hair may touch or be cut by a frame edge, a split-screen divider or a card. Keep margin. Set it as numbers (e.g. hair at least about 30 px from the top and 50 px from the sides at 1080p) and check it on keyframes and on every video frame.
- **Measure programmatically on every frame**: face box from the detector, expanded upward about 45% and sideways about 15% for hair; record min distance to each frame edge, divider and card. Then check the tightest frames by eye at full resolution.
- Report detector coverage (faces found on N of M sampled frames). Near-profile and fade frames are often missed; cover those visually on contact sheets.
- Fix tight headroom on a keyframe **by background-only outpainting**: extend or shift the canvas, inpaint only the new background band, and restore the original face pixels, so likeness is unchanged. Re-score likeness after the fix (it should be unchanged) and inspect the new band for smears.
- In video prompts add a headroom line when a person rises or leans, e.g. *"keep the full head and hair inside the frame with clear space above at all times."* Standing up in a take is the most common cause of a cut head.
- Reject takes where hair touches the edge, even briefly; use the sibling take.

---

## 3.7 Staging and acting (sharpens 2.4.1, 2.4.2)

**Change to 2.4.1.** The original asks for three-quarter faces with yaw ≤ 30° in every shot. That is the default for likeness-critical close shots only. Actors do **not** need to face the camera in every shot, and a film where everyone faces the lens looks staged.

**Rules.**
- Mix profile, back, over-the-shoulder and moving-camera shots with the 3/4 shots. Keep 3/4 coverage for each person somewhere in the scene, so the viewer learns the face.
- **Two people never do the same action at the same time.** Stagger: one reads, the other types; one looks up as the other looks down.
- Write action as **sequences**, not states: "looks down at the report, finds a line with a finger, looks up at the screen, frowns slightly". Keep acting restrained and natural.
- Build staging and screen content **from the full script and VO**, not shot by shot. Write a story-beat table first: per beat, the VO line and "what the picture must say". Every concept must serve every beat.
- Screens must carry the story: if the VO says "information overload", the screen shows overload (2.9.15). Printed props support the screens; they do not replace them.
- **QC of profile and back shots**: face embeddings score low on profiles even when identity is right (in this round about 0.05 to 0.3 at yaw 65 to 90°, while the same person scored 0.6 to 0.7 at ≤ 20°). Do not "fix" a correct profile with face edits. Judge profile shots by a same-angle reference, hair, ear, build, wardrobe and signature detail, and by the score drift against the clip's own first frame. Report them as "angle-limited", with the yaw.
- Near-frontal angles raise likeness but invite looks into the lens. Add an explicit eyeline target (3.8) whenever the face is within about 20° of camera.

---

## 3.8 Eyelines on content (sharpens 2.4.1, 2.5.5)

- Eyelines must land **on** the screen content or the page, not above it and not into the lens. Name the target: *"eyes on the lower-left table on the screen"*, *"eyes on the second line of the page"*.
- One take in this round had a person look into the lens for about 1.5 s; its sibling take with an added eyeline line passed. Generate at least 2 takes per shot so an eyeline failure has a replacement.
- Check eyelines on every 0.5 s contact frame, not only the keyframes.

---

## 3.9 Cut smoothness and joins (extends 2.10.1, 2.12, 2.13.3)

**Why.** A visible jolt at a join had three causes stacked: an EDL that stored cut times as decimal seconds (rounded to 4 places) so one shot ended a frame late and repeated its last frame while the next shot skipped its first frame; a near jump cut (3.3); and different camera drift in the two takes. The same rounding error hit five other cuts. Separately, one re-comp came out oversaturated, and one shot nearly froze because its screen texture was static.

**Rules.**
- **Cut boundaries are integer frames.** Store `start_frame` and `end_frame` in the EDL; compute `start_frame = round(start_s × fps)` once, and assign frame f to the next shot when f ≥ its start_frame. Never let a source run out and hold its last frame.
- **Prove cut positions**: per-frame MD5 of the assembled lossless master equals the concatenated per-shot comps; the plate-vs-comp difference outside screens is lowest at offset 0 (not ±1).
- **Measure camera drift in "locked-off" takes** before use (ECC or feature homography frame to frame). If per-frame scale exceeds about 1.0003 or translation about 0.3 px, stabilise (inverse ECC) or use another take. Two takes drifting differently make a join jolt.
- **Keep the grade consistent** across joined shots. Compare per-shot stats outside the screens against the approved version: L percentiles (p5/p50/p95), Lab a and b, Lab chroma. Use Lab chroma, not HSV saturation: HSV S rises in deep shadows and misleads.
- **Check motion energy per shot** on the final comp (2.6), including the screen. A static UI texture can make a calm shot read as frozen: count near-static frame pairs; give screens continuous small life (a scrolling list, a loading bar, a cursor) so every beat has 0 static pairs.
- **Join strips on the final film**: for every cut, frames a−1, a | b, b+1 side by side, plus a 1:1 zoom. Check for flash frames, blends, repeated frames and subtitle continuity. Place cuts in VO pauses where possible.
- Keep lossless intermediates (e.g. FFV1) for comps and assemblies; encode once at the end.

---

## 3.10 Occlusion leaks in screen comps (extends 2.9.10)

**Why.** In this round a bezel and screen fill were drawn over a shoulder in a dark jacket (plate difference cannot separate a dark jacket from a dark bezel), the glass quad was drawn wider than the real screen, the UI showed through an overexposed palm, and a held white page merged with the white glass in the brightness mask.

**Rules.**
- Build the occlusion matte as the **union** of the plate-difference matte and a **person segmentation matte** (e.g. u2net human seg) of the raw frames. Add hand-placed anchors or roto for held props (paper) that match the glass colour.
- Never draw the glass fill or bezel outside the real screen boundary; test the quad against the raw plate edge.
- Run an **independent leak check**, separate from the comp's own matte: person pixels (seg, eroded a few px) inside the glass (eroded) where the comp differs from the plate. Target 0 px; investigate anything above about 50 px with zoom crops.
- Segmentation has false positives (a bezel or monitor marked as a person) and sweeps can misalign by a frame and flag moving hands outside the screen. **Verify visually before "fixing"** a reported leak, and confirm the check ran at frame offset 0.
- Sweep every shot where a person overlaps a screen, ranked by risk (hands into the screen, arms across it, pointing), not only the shot that was reported.

---

## 3.11 Screens and readability (extends 2.9.13 to 2.9.15)

- Primary labels must be readable in the delivered frame: effective em height at least about 18 px at 1080p. Measure it after the warp. Small background screens will be texture; rely on colour flags and chips there, and say so in QC.
- Chrome, titles and panel frames are present from frame 1; only the data builds. This avoids a blank-screen flash at the cut.
- Image models may add brand logos to monitors and bezels. Put "no logo on the monitor or bezel" in the set line and check every keyframe.
- A generated screen sometimes comes out dark or off. Retry once; log it.

---

## 3.12 Variants and finished alternatives (extends 2.2.2 decisions)

- When direction is open (layout, staging, monitor type), build **2 to 3 concepts as cheap image mocks** (e.g. concepts A / B / C, START and END keyframes per beat) and let the client pick before any video.
- Write each concept in its own prompt file so parallel workers can build them, with a story-beat table shared by all.
- When the client asks for finished work, deliver **finished alternatives** (e.g. two complete films that differ only in the opening), not rough string-outs. Share untouched segments bit-identically between them and prove which frames differ.
- Hand picks to the assembler as machine-readable data: per beat the source clip, inclusive in and out frames, screen quads and occluders.

---

## 3.13 Delivery and archiving (sharpens 2.12, 2.13)

- **Lock the audio stream** when only picture changes: stream-copy it and verify the audio MD5 (`ffmpeg -i f.mp4 -map 0:a -c copy -f md5 -`) is identical to the previous version. Re-measure loudness anyway (−16 LUFS, true peak per 2.11.7).
- **Lock the exact frame count** (count decoded frames, not just duration). Keep AAC, the size limit (under 100 MB) and the rest of 2.12.
- Produce subtitled and clean versions of every alternative. Check burned-in subtitles by differencing subtitled vs clean per frame; the pattern must match the previous version.
- Prove unchanged ranges with per-frame RGB MD5 before encode (stronger than the MAD check in 2.13.1); expect only encoder noise next to changed GOPs after encode.
- Archive every version's refs, keyframes, takes, prompts (including per-take prompt additions), QC and EDL. Never overwrite; superseded files keep their names and are marked "superseded" in the deliverables table.
- **Keep the EDL in sync with the actual shot versions.** In this round a stale EDL pointed at old shot files. Generate the EDL from the build script, never by hand, and check each EDL path and frame range against the files actually used.
- Interrupted or partial QC outputs are labelled as leftovers so nobody reads them as current.

---

## 3.14 Honesty about fixes and caveats (sharpens 2.1.11, 2.13.4)

- When an earlier fix was wrong (e.g. a watch moved to the wrong wrist, a "leak fix" that was a measurement artefact), say so plainly in the next report: what was wrong, why, and what replaced it.
- Report residual caveats **with the delivery**, each with its number: e.g. a shot under the motion target but never frozen, a low-likeness person and its cause (3.1), detector coverage gaps, a small screen whose text is texture only.
- "Edit changed nothing" and "END is a near-copy of START" are failures. Diff the START and END keyframes and report it; do not present a near-copy as an action.

---

## 3.15 Budget per batch (sharpens 2.2.3, 2.2.4)

- Estimate usage **before each generation batch** (images and video separately), get approval for that batch, and pilot first. Approval for images is not approval for video.
- The usage cap may be shared with other apps on the same plan. Read the current usage and reset time before the batch.
- Unit costs change with plans. **Calibrate after the first small batch** (e.g. 4 images per line): measure the real increase, recompute the estimate, then continue.
- An interrupted or refused call can still consume usage. Log it in the cost log even when it returns nothing.
- Image calls with too many reference images may be refused (in this round 6 refs failed, 5 or fewer worked). Keep to 5 or fewer and choose them deliberately (photo, sheet, set reference).
- Standard: 2 takes per shot plus a small reserve; use the reserve only for a clear failure (frozen clip, morphing face, wrong set). Record which reserve takes were used.

---

## 3.16 Additions to the final QC checklist (extends 2.14)

**Casting**
- [ ] Every real person's photos passed 3.1 (size, sharpness, views, neutral, no filter, no occlusion, recent) or the risk is in the plan and the client was told in writing
- [ ] Test sheet likeness median ≥ about 0.5 on frontal and 3/4 test frames per person

**Set and direction**
- [ ] Set bible locked; monitor count and type identical across the scene; set reference passed to every call
- [ ] Join strips reviewed for every cut before video; no 180° flips; END of N matches START of N+1
- [ ] Adjacent shots differ by one size or ≥ 30°
- [ ] Side-specific details (watch, pin) on the correct anatomical side in every shot

**Staging and framing**
- [ ] No two people doing the same action at once; action written as sequences
- [ ] Profile shots judged by angle-aware QC, not "fixed" with face edits
- [ ] Eyelines on content in every 0.5 s frame
- [ ] Min headroom and edge margins measured on every frame; tightest frames checked by eye
- [ ] Split screens: same camera, head-size ratio and eye-line matched on median; states visibly different

**Comp and edit**
- [ ] Integer-frame EDL; cut positions proved by MD5; no repeated or skipped frames at joins
- [ ] Camera drift measured; grade stats match across joins (Lab chroma)
- [ ] Every shot, including screen textures, has motion; no static pairs in UI beats
- [ ] Independent leak check = 0 px (person seg ∪ plate diff); flagged leaks verified visually

**Delivery**
- [ ] Audio MD5 identical to the previous version for picture-only changes; exact frame count; −16 LUFS; AAC; < 100 MB
- [ ] Subtitled and clean versions of every alternative
- [ ] EDL generated by the build and matching the shot files used
- [ ] Wrong earlier fixes and residual caveats stated in the report

---

