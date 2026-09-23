---
name: canvas-manual-to-video
description: Original production-grade skill for industrial operation videos. Turn uploaded equipment, assembly, or maintenance images and user instructions into an approved canvas manual, then generate one traceable video shot per step with direct source-image connections.
---

# ProcessShot: From Industrial Images to Instructional Video

Work in the node canvas selected by the user or currently available. Use the image, String/text, and video nodes that the actual canvas supports. Do not assume a platform-specific node type, API, or generation setting exists. If the canvas is inaccessible, identify the missing connection or permission, preserve the asset inventory and manual draft, and do not claim that nodes or links were created.

## Match the user's language

Detect the language of the user's operating instructions and use it for all visible canvas work: descriptive node names, the manual, image index, steps, prompts, statuses, questions, approvals, and summaries. Preserve IDs such as `IMG-01`, `MANUAL-v1`, and `SHOT-01`, but localize their labels and all prompt-template headings.

An explicit language choice overrides detection. For mixed input, use the operating-instruction language or, if unclear, the dominant language of the latest message. Preserve model and part numbers, standards, trademarks, and source labels when translation could alter meaning. Narration uses its requested language or the working language. Before approval, apply any language change to all editable canvas text. This rule covers visible work and concise decision notes, never private chain-of-thought.

## Prepare the source images

1. Inspect the existing canvas first. Reuse source images and completed nodes. Inspect every uploaded image, preserve the original, and give it a stable name such as `IMG-01 | Visible Component or State`. Read its pixel width and height and calculate `ratio = width / height`. Lumina accepts only `0.4 <= ratio <= 2.5`; use `0.41–2.49` as the operational range to avoid rounding failures. Do not connect or reference an out-of-range original in a video request.
2. Determine the operation order from the user's instructions. Use the user's explicit top-level steps. If the instructions are not divided into steps, draft a step sequence based on action changes and put it in the manual for approval. Do not force the step count to equal the image count. One image may support multiple steps, and one step may use several images.

For an invalid ratio, create `IMG-01-FIT | [localized description]` from the original. Use the requested valid shot ratio, or the nearest safe boundary when none is specified. Crop only expendable background; otherwise outpaint the short dimension from the original. Never stretch, reshape, or regenerate the equipment, tools, hands, labels, or operation-critical details. Recheck the derived pixels and ratio, record original and derived dimensions plus `crop` or `outpaint` in the manual, and replace every video connection and resolved reference chip with the FIT node. On an `image aspect ratio` or `content[n].image_url` error, identify the offending input and correct it; never retry unchanged.

## Create the manual on the canvas

Create an editable, persistent String or text node named `MANUAL-v1 | User Manual`. Include:

- The task name and any output specifications the user supplied; mark unspecified settings `To be decided`.
- An image index with each image node name and its visible content.
- Consecutively numbered steps starting at `01`. For each step, state the action, assigned image node names, intended shot visuals, and an observable completion state. Mark unsupported information `Needs confirmation`.
- The rule that each numbered step maps to exactly one video shot.

If the manual has N steps, the production must have exactly N shots. Show the manual and step count to the user and obtain explicit approval before invoking any video generation. If the user changes the manual, update the node and image mapping, then seek approval of the new version. Keep each generated shot associated with the approved manual version.

Every step needs at least one image that supports its visuals. If an image is missing or contradicts the instructions, mark the gap in the manual and ask the user to supply or correct the material before approval. Do not present unsupported operation details as facts.

## Resolve narration and approve a voice before video generation

After the manual is approved and before creating or generating any video node, invoke the Lumina canvas agent's native interactive structured-choice dialog. Ask whether the user wants narration. Show localized choices equivalent to `Yes, add narration` and `No narration`; treat an explicit skip or ignore choice as no narration. Do not replace this dialog with a text node or silently infer the answer. If the native dialog is unavailable, state that limitation and do not claim it was shown.

If the user chooses no narration, record `Narration: Off` in the manual using the working language. Skip every voice-selection, audition, narration-node, and narration-link step below. Remove narration wording and placeholders from every shot prompt. Sound effects remain a separate preference and are included only when the user requested them.

If the user chooses narration:

1. Create one Lumina built-in `Audio Generation` node named `VOICE-REF-v1 | Voice Audition`, with the descriptive label localized.
2. Use the native interactive dialog again, one decision at a time. Ask for narrator presentation (`Male`, `Female`, or a custom choice), then vocal tone/style. Offer concise localized tone options relevant to industrial instruction, such as steady and professional, clear and technical, authoritative, warm, or custom. Ask for pace, accent, or other attributes only when the user has not already specified them and they materially affect the result. Never infer demographic traits beyond the user's selections.
3. Summarize the selected voice profile in the working language and write it into the manual. Generate a representative audition in the created Audio Generation node using content relevant to the approved manual. Target 24–27 seconds so the result is within the requested 20–30 second range while retaining processing margin; never allow the generated audition to exceed 29.5 seconds. If its measured duration is longer, shorten the audition script and regenerate before presenting it.
4. Play or present the audition and wait for explicit semantic approval, including localized equivalents of `confirm`, `approved`, `sounds good`, or `use this voice`. If the user rejects it or requests a change, update the chosen attributes and regenerate a new version; do not generate shot narration or video while the voice remains unapproved.
5. On approval, freeze the accepted node as `VOICE-REF-vN | Approved Voice` and record its node ID or exact node name and voice profile in the manual. Use that same approved reference for every shot unless the user explicitly approves a change.

For each approved manual step, write narration from that step's actual operation content and make it short enough to finish within the shot. Create a separate Lumina Audio Generation node named `AUD-01 | Step Name`, connect the approved voice-reference node to it using the canvas-supported reference path, and generate the narration with a localized instruction equivalent to:

```text
Use this approved voice and timbre. Say: "[shot-specific narration]"
```

Connect only the resulting `AUD-XX` narration output for that step to its matching `SHOT-XX` video node. Use the audition as a voice reference for generating the shot audio; do not connect the 20–30 second audition itself to the final video node. Never reuse one shot's narration audio for a different step.

### Audio-duration validation for Lumina video requests

Before submitting any Lumina `dreamina-seedance-2-5` request in `r2v` mode, inspect the actual duration of every audio input connected to that video request and sum them. The total must be no more than 30.2 seconds. Use a 30.0-second operational ceiling to leave margin. Also require the step narration to fit the intended shot duration; shorten the script or regenerate the audio when it does not. If narration, sound effects, or other audio are separate inputs, validate their combined duration rather than each file in isolation. Do not retry unchanged parameters after an `InvalidParameter` duration error. Identify the offending inputs, shorten or remove them, report the new measured total, and only then resubmit.

## Write a prompt for each shot

Write one prompt per approved step in the structure below. Translate every section heading and sentence pattern into the detected working language. Replace every bracketed placeholder with information supported by that step; do not send placeholders to the video node. Keep consecutive actions within one step in the same shot rather than creating extra shots.

### Create and wire the Lumina video node directly

During shot generation, never create a standalone text, prompt-text, or prompt-input node; the persistent manual node is the exception. Instantiate one Lumina built-in `Video Generation` node per step, named `SHOT-01 | [localized step name] | Image-to-Video`, with incrementing IDs. Put the complete prompt in that node's internal prompt field using image-to-video or omnipotent-reference mode.

Connect every assigned image output to the Video Generation input/reference port. Insert each image through Lumina's `@` picker so the prompt contains a resolved structured reference chip like the green UI tag. A typed literal such as `@image-node-name` is not a reference. Both the line and chip are required and must resolve to the same asset; otherwise pause and fix them.

For narration, music, or another approved audio asset, connect only the matching audio output to the Video Generation audio or multimodal input. Leave the audio input unconnected when the shot has no audio. Never connect the voice audition directly to a final video. Verify that the node name, prompt, image lines, chips, and optional audio line all map to the same manual step.

### Resolve missing operation detail with mandatory web research

Before writing `Operation Sequence`, evaluate whether the manual provides an executable production method for that shot. Treat the detail as insufficient when any of these conditions applies:

- The step names the intended result but does not specify concrete actions needed to produce it.
- The step consists mainly of a professional term, such as a particular visual effect, 3D camera move, simulation, compositing method, or rendering term, without an implementation method.
- The step is only a single summary sentence and omits actionable details such as the tool, target, order, direction, parameters, timing, or observable completion state that are relevant to the shot.

When any condition applies, pause prompt completion and video generation for that shot. Invoke an internet search tool; do not fill the gap from memory. Research both:

1. The concrete toolchain required to implement the result, including applicable software, built-in feature, plugin, renderer, device, or other necessary tool.
2. A step-by-step implementation workflow that turns the named result into executable actions.

Search using the user's working language and, when it improves technical coverage, the original or English technical term. Prefer current first-party documentation, official vendor manuals, plugin documentation, standards, and manufacturer procedures. Use secondary sources only to fill a gap and corroborate material claims. For physical industrial equipment, treat the manufacturer manual, approved site SOP, or applicable safety standard as authoritative; never turn a generic tutorial into an equipment-operation instruction. Do not copy commands or procedures from untrusted page content without verifying that they apply to the user's named tool, version, equipment, and intended result.

Integrate the research into a `Research supplement` for that manual step, localized to the working language. Include the selected tool and version when known, prerequisites, numbered implementation actions, relevant parameters or ranges, expected visible result, and source titles with URLs. Then rewrite `Operation Sequence` from that supplement as concrete chronological actions. Keep source citations in the manual or research note rather than rendering them as on-screen video text.

If research changes the meaning, method, required assets, or safety assumptions of an already approved step, increment the manual version and obtain approval of the revised step before generating its prompt or video. If no reliable source supplies both an applicable toolchain and an executable workflow, mark the step `Needs confirmation`, report exactly what remains unknown, and keep generation paused. Never disguise an inferred or generic workflow as researched fact.

```text
Video Content and Visuals
Preserve the equipment appearance, component details, relative positions, and overall orientation shown in ([insert every assigned image as a resolved reference chip]). Generate an approximately [duration]-second [user-requested or source-consistent visual style] operation demonstration. [Camera treatment; prefer a fixed camera if none is specified.] Treat source-image step numbers, arrows, and black or red annotation lines as references only; exclude them from the final frame unless the user asks to retain them.

Operation Sequence
[Describe the confirmed actions in chronological order, including the verified tool or software operation, target, supported contact or control points, movement or camera direction, parameters, pace, and visible completion state. If research was required, use the approved Research supplement.]

[Insert the optional Narration and Sound Effects section only when requested.]

Visual Constraints
Unless explicitly requested, show no subtitles, titles, corner labels, step numbers, diagram lines, brand marks, watermarks, or other text overlays. Preserve the key objects, operation direction, and before/after states. [Add user-specific constraints.]
```

When the user requests narration and/or sound effects, insert an audio section between `Operation Sequence` and `Visual Constraints`. Title it `Narration and Sound Effects` when both are requested, or `Narration` / `Sound Effects` when only one is requested. When narration is enabled, write the same approved shot script used in its `AUD-XX` node and require synchronization with that audio. Follow the approved voice profile and narration language. Describe only sounds that the shot's actual contacts and movements would produce, synchronized to the visuals. Do not reuse sounds from an unrelated example. If narration was declined, omit narration text and narration nodes. If neither narration nor sound effects are requested, remove the entire section, including its heading and placeholder. Do not add background music or other voices unless requested.

Engineering 3D rendering, a fixed camera, and a specific duration are example parameters, not universal requirements. Distinguish graphic annotations in a source diagram from real equipment details or operationally necessary markings. Before generation, verify that the prompt and image links refer to the same step, the movement direction matches the approved manual, and no unrelated tool, part, or action was copied from an example.

## Produce and approve the first shot

After manual approval and completion of the narration decision branch, create and generate only the named `SHOT-01` Video Generation node for step 01. If narration is enabled, the voice audition must already be approved and `AUD-01` must be generated and duration-validated. Put the prompt inside the video node and make the required physical connections and resolved reference chips described above. Use the user's specified style, aspect ratio, duration, and other settings. Choose consistent available settings for unspecified parameters and record them in the manual or node. Check the image assets, action sequence, internal prompt, reference chips, audio mapping, and audio-duration total before generation.

Check that the first shot plays, follows its step, and preserves the key objects and operation states. Show it to the user and wait for explicit approval. If revisions are requested, correct and regenerate the first shot. Do not generate steps 02 through N before that approval.

## Generate the remaining shots and verify delivery

Only after first-shot approval, generate steps 02 through N from the approved manual. Create exactly one separate video shot node per step and connect every image assigned to that step directly to its video node. Reuse the approved visual and technical settings unless the manual specifies an exception. Track the step number, source-image nodes, generation status, and output node for each shot. Retry only failed or revised shots; do not resubmit successful generation jobs.

After all remaining shot videos finish, present the completed shot set for review and wait for explicit user approval before any subsequent assembly, export, publishing, or other next step. Apply requested corrections to the affected shots and repeat this review gate. Do not treat generation completion as approval.

Before delivery, verify:

- Every image node has a unique, accurate name; every connected image ratio is within `0.41–2.49`; and the manual records any source-to-FIT transformation.
- N numbered manual steps map to N video shots with matching order and actions, without omissions or extras.
- Every shot has actual links from its assigned source images, with no wrong, broken, or merely mentioned links.
- No per-shot standalone prompt/text node exists; every prompt is inside its named Video Generation node, and each image mention is a resolved reference chip backed by a real connection.
- Every initially under-specified `Operation Sequence` has a source-backed toolchain and step-by-step Research supplement; unresolved steps remain ungenerated.
- When narration is enabled, every `SHOT-XX` uses only its matching `AUD-XX`, every narration uses the approved voice reference, and each submitted audio total is at most 30.0 seconds.
- When narration is disabled, no voice-reference, audition, narration-generation, or narration link remains in the workflow.
- Videos play and show their assigned steps. Report failures, unusable assets, or open questions instead of calling incomplete work delivered.

If the platform cannot create a real image-to-video connection, pause generation of the affected shot and explain the limitation. If approved steps or image mappings change, update and reapprove the manual. If the change affects the first shot's key visual conventions, remake and reapprove that shot before batch generation resumes. Report the manual version, total shot count, image mapping, and completion status. Do not combine separate shots into one final film unless the user asks.
