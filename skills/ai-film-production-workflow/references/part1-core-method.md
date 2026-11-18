# Part 1 — Core method (source: Grok Imagine × Wonder Studios, "Inside Episode 1: The Oath")

Source: the X Article "Inside Episode 1: The Oath" by @imagine (Grok Imagine), linked from https://x.com/imagine/status/2115214583983935517. Quoted phrases are verbatim from the article. Lines marked *(addition)* are not from the article.

## 1. Write Your Script, then List Every Shot (before generating)
- "Before you generate anything, write a shot list."
- "For every shot, note the shot size, lens, camera movement, action and the exact dialogue." Write the action for each character separately, and note the VO text and the duration in seconds *(addition)*.
- "The more specific you are, the more consistent your results will be."
- "Keep your files organized from the start. Name folders by scene, then by A-roll or B-roll", e.g. `SCENE1_FIELD_BROLL` (pattern: `SCENE#_LOCATION_AROLL` / `SCENE#_LOCATION_BROLL`). Give each shot its own folder with its keyframes, prompt and all takes *(addition)*. "Once you're generating dozens of takes per shot, this makes it far easier to find the right clip when you're editing."
- Keep runtime short. The episodes were planned at "about 5 minutes each" for social media. If the source story is thin, you have to imagine the rest, "from the dialogue to the characters' faces."

## 2. Use the Canvas
- "Put everything on one canvas. Every frame, prompt and take lives there, grouped by shot."
- "Lay takes side by side to pick the best one and check continuity across a scene: faces, wardrobe, lighting."

## 3. Make Your Character Sheet, and Build Your Character Prompt
- Sheet: "Keep it simple. A close-up, a full front and a full back, on plain grey." In practice that is one 16:9 image in "THREE VERTICAL PANELS": LEFT "three-quarter facial close-up", CENTER "Full-body FRONT VIEW", RIGHT "Full-body BACK VIEW", on a "Neutral medium-gray seamless studio background, soft diffused studio lighting".
- "If the character ages or changes costume, make a new sheet." In the article, the lead character has three sheets.
- "The character prompt helps keep their face the same in every shot." Expect "the occasional face drift" anyway.
- Character prompt structure, in the article's labels:
  - Opening line: "An ultra-photorealistic cinematic character reference sheet for [NAME], [role, place, period]."
  - `CHARACTER:` age, skin, features, eyes, nose, hair, beard, build, face and expression.
  - `COSTUME:` garments, materials, accessories, with signature details called out (e.g. a brooch that is "prominently visible on his right shoulder"), and "Authentic handmade materials".
  - `LAYOUT:` the three panels. "All three panels must depict the EXACT SAME MAN wearing the EXACT SAME COSTUME, with identical proportions, materials, and accessories."
  - `PHOTOGRAPHY:` "Professional Hollywood costume reference photography", background, lighting, "natural skin texture", "subtle film grain", "Grounded historical realism, not fantasy."
  - Closing restrictions: "STRICTLY PHOTOREALISTIC. No illustrations, no CGI appearance, no text, no labels, no additional characters."
- Real-person portrait projects *(addition, not from the article)*: build each sheet from reference photos of the real person, and check likeness person by person.

## 4. Build Your Locations, and Keep One Look Across All Shots
- List every location and make a reference image for each. (Episode 1 had five: field, palace, shore, ship, camp.)
- "Every prompt names the shot size, time of day, mood, camera, lens and film stock."
- "Start from a mood board or a style reference image, then name the camera, lens and film stock in every prompt."
- Do not name directors. "We did not name any director when prompting". Their look came from "early-2000s period epics".

## 5. Build Your Keyframes, and Use Middle Keyframes
- "Put the character from your sheet into each relevant location to get the keyframes you'll animate."
- "If the still frame isn't right, your animated video won't be right either." Spend time getting the keyframes great "before moving on to animating."
- Use start, middle and end frames when needed. Grok Imagine "lets you add a frame between the start and end frames. You can choose the exact time when it lands." For example, use a middle frame to make a character "enter on cue".

## 6. Write Your Video Prompts in Blocks
- "Use 7 blocks": camera, action, environment, physics, audio, SFX, constraints. In the article's full prompt these are headed:
  `[Camera & Style]` `[Subject Motion / Action]` `[Environment & Atmosphere]` `[Physics & Details]` `[Audio]` `[SFX]` `[Restrictions / Constraints]`
- `[Camera & Style]`: look, format and shot, e.g. "Shot on 35mm film with vintage spherical lenses. Naturalistic 24fps motion, subtle film grain." "ONE CONTINUOUS SHOT. Use the three reference images as sequential visual guides for the action." "EXTREME WIDE SIDE PROFILE. Completely STATIC, LOCKED-OFF CAMERA."
- `[Subject Motion / Action]`: write by screen position (`FRAME LEFT:`, `CENTER:`, `FRAME RIGHT:`, "enter from OFFSCREEN RIGHT") and map it to the keyframe progression: "The shot begins matching IMAGE 1", "progresses naturally through the composition shown in IMAGE 2", "By the end of the shot, the composition matches IMAGE 3".
- `[Environment & Atmosphere]` and `[Physics & Details]`: mark must-see motion with `CRITICAL:` (e.g. "CRITICAL: THE WIND VISIBLY BLOWS THE MEN'S CLOAKS."). Describe weight, contact and material response (e.g. "Heavy, rain-soaked wool moves with believable weight and resistance").
- `[Audio]` / `[SFX]`: state what is and is not wanted, e.g. "NO MUSIC. NATURAL SOUND ONLY. NO DIALOGUE.", then list the specific sounds.
- `[Restrictions / Constraints]`: hard rules as short ALL-CAPS lines, e.g. "ONE CONTINUOUS SHOT. NO CUTS.", "IMAGE 1 IS THE EXACT STARTING COMPOSITION.", "IMAGE 3 IS THE EXACT ENDING COMPOSITION.", "STATIC LOCKED-OFF CAMERA THROUGHOUT.", "NO PAN, TILT, ZOOM, DOLLY, TRACKING, OR CAMERA SHAKE.", "PRESERVE THE EXACT LANDSCAPE AND FRAMING.". Also state each character's position (e.g. "... REMAIN FRAME LEFT."), "... MUST BE FULLY VISIBLE BY THE FINAL FRAME.", and the motion that must be visible (e.g. "FOREGROUND PLANTS MUST SWAY AND BEND IN THE WIND THROUGHOUT THE SHOT.").
- "If a prompt fails, make it shorter. More detail rarely fixes the issue."

## 7. Write the Camera into Every Prompt
- "Use a static camera to set the scene. Go handheld when the fighting starts."
- In a handheld prompt, spell out the shake character and speed. Key phrases from their go-to prompt: "Unstabilized handheld combat camera POV, violent jagged shake", "the horizon tipping off-level with every stride", "focus hunting and snapping", "Chaotic, never aligned, never smooth", "Fast, urgent, real-time speed, no slow motion."

## 8. Pick Takes, then Finish in the Edit
- Generate multiple takes per shot ("dozens of takes per shot") and lay them side by side on the canvas to pick the best one.
- Editing, sound and colour happen in post: "Editing, sound and color took about 18 hours." "Most sound effects came straight from Grok Imagine. We added music and a color grade on top."

## Pre-flight checks *(addition, not from the article)*
- If the work costs money, first write a complete plan (requirements list, composition, control frames, QC, cost) for the user to approve.
- Pilot one shot first. Scale up only after it is confirmed.

---

