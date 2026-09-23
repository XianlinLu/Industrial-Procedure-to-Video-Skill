---
name: canvas-manual-to-video
description: Original production-grade skill for industrial operation videos. Turn uploaded equipment, assembly, or maintenance images and user instructions into an approved canvas manual, then generate one traceable video shot per step with direct source-image connections.
---

# ProcessShot: From Industrial Images to Instructional Video

Work in the node canvas selected by the user or currently available. Use the image, String/text, and video nodes that the actual canvas supports. Do not assume a platform-specific node type, API, or generation setting exists. If the canvas is inaccessible, identify the missing connection or permission, preserve the asset inventory and manual draft, and do not claim that nodes or links were created.

## Match the user's language

Automatically detect the language of the user's current operating instructions and use it for all user-visible canvas work. This includes descriptive node names, the manual, image index, step descriptions, shot-prompt headings and bodies, status labels, open questions, approval requests, and delivery summaries. For example, an English request produces English canvas content, and a Japanese request produces Japanese canvas content. Preserve stable identifiers such as `IMG-01`, `MANUAL-v1`, and `SHOT-01`, but localize their descriptive labels. Translate the prompt-template headings below into the working language instead of copying the English headings verbatim.

An explicit language request overrides automatic detection. If the request mixes languages, use the language of the operating instructions; if that is still unclear, use the dominant language of the user's latest message. Keep model numbers, part numbers, standards, trademarks, and source labels in their original form when translation could change their meaning. Narration follows the user's separately requested narration language; otherwise it uses the same working language. If the working language changes before approval, update all editable canvas text consistently before asking for approval. Do not expose private chain-of-thought; this rule applies to visible working content and concise progress or decision notes placed on the canvas.

## Prepare the source images

1. Inspect the existing canvas first. Reuse source images and completed nodes to avoid duplicates or repeat generation. Inspect every uploaded image and reconcile the number of image nodes. Preserve the original assets. Give each image node a stable, unique name such as `IMG-01 | Visible Component or Operation State`. Describe only what is visibly supported; mark uncertain details `Needs confirmation` rather than guessing equipment, parts, actions, or sequence.
2. Determine the operation order from the user's instructions. Use the user's explicit top-level steps. If the instructions are not divided into steps, draft a step sequence based on action changes and put it in the manual for approval. Do not force the step count to equal the image count. One image may support multiple steps, and one step may use several images.

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

Write one prompt per approved step in the structure below. Translate every section heading and sentence pattern into the detected working language. Replace every bracketed placeholder with information supported by that step; do not send placeholders to the video node. Use the canvas's actual image-reference syntax, such as `@image-node`, and connect those same image nodes directly to the video node. Keep consecutive actions within one step in the same shot rather than creating extra shots.

```text
Video Content and Visuals
Preserve the equipment appearance, component details, relative positions, and overall orientation shown in (@images assigned to this step). Generate an approximately [duration]-second [user-requested or source-consistent visual style] operation demonstration. [Camera treatment; prefer a fixed camera if none is specified.] Treat source-image step numbers, arrows, and black or red annotation lines as references only; exclude them from the final frame unless the user asks to retain them.

Operation Sequence
[Describe the confirmed actions in order, including supported tool-to-part contact points, movement direction, pace, and visible completion state.]

[Insert the optional Narration and Sound Effects section only when requested.]

Visual Constraints
Unless explicitly requested, show no subtitles, titles, corner labels, step numbers, diagram lines, brand marks, watermarks, or other text overlays. Preserve the key objects, operation direction, and before/after states. [Add user-specific constraints.]
```

When the user requests narration and/or sound effects, insert an audio section between `Operation Sequence` and `Visual Constraints`. Title it `Narration and Sound Effects` when both are requested, or `Narration` / `Sound Effects` when only one is requested. When narration is enabled, write the same approved shot script used in its `AUD-XX` node and require synchronization with that audio. Follow the approved voice profile and narration language. Describe only sounds that the shot's actual contacts and movements would produce, synchronized to the visuals. Do not reuse sounds from an unrelated example. If narration was declined, omit narration text and narration nodes. If neither narration nor sound effects are requested, remove the entire section, including its heading and placeholder. Do not add background music or other voices unless requested.

Engineering 3D rendering, a fixed camera, and a specific duration are example parameters, not universal requirements. Distinguish graphic annotations in a source diagram from real equipment details or operationally necessary markings. Before generation, verify that the prompt and image links refer to the same step, the movement direction matches the approved manual, and no unrelated tool, part, or action was copied from an example.

## Produce and approve the first shot

After manual approval and completion of the narration decision branch, create and generate only `SHOT-01 | Step Name` for step 01. If narration is enabled, the voice audition must already be approved and `AUD-01` must be generated and duration-validated. Use the shot prompt structure above and the user's specified style, aspect ratio, duration, and other settings. Choose consistent available settings for unspecified parameters and record them in the manual or node. Connect every assigned image node directly to the shot's video node. Connect only its matching approved shot-audio output when narration is enabled. If supported, also link the relevant manual step. Check the image links, asset contents, action sequence, prompt, audio mapping, and audio-duration total before generation.

Check that the first shot plays, follows its step, and preserves the key objects and operation states. Show it to the user and wait for explicit approval. If revisions are requested, correct and regenerate the first shot. Do not generate steps 02 through N before that approval.

## Generate the remaining shots and verify delivery

Only after first-shot approval, generate steps 02 through N from the approved manual. Create exactly one separate video shot node per step and connect every image assigned to that step directly to its video node. Reuse the approved visual and technical settings unless the manual specifies an exception. Track the step number, source-image nodes, generation status, and output node for each shot. Retry only failed or revised shots; do not resubmit successful generation jobs.

After all remaining shot videos finish, present the completed shot set for review and wait for explicit user approval before any subsequent assembly, export, publishing, or other next step. Apply requested corrections to the affected shots and repeat this review gate. Do not treat generation completion as approval.

Before delivery, verify:

- Every image node has a unique, accurate name, and the manual node opens to the approved version.
- N numbered manual steps map to N video shots with matching order and actions, without omissions or extras.
- Every shot has actual links from its assigned source images, with no wrong, broken, or merely mentioned links.
- When narration is enabled, every `SHOT-XX` uses only its matching `AUD-XX`, every narration uses the approved voice reference, and each submitted audio total is at most 30.0 seconds.
- When narration is disabled, no voice-reference, audition, narration-generation, or narration link remains in the workflow.
- Videos play and show their assigned steps. Report failures, unusable assets, or open questions instead of calling incomplete work delivered.

If the platform cannot create a real image-to-video connection, pause generation of the affected shot and explain the limitation. If approved steps or image mappings change, update and reapprove the manual. If the change affects the first shot's key visual conventions, remake and reapprove that shot before batch generation resumes. Report the manual version, total shot count, image mapping, and completion status. Do not combine separate shots into one final film unless the user asks.
