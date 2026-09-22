---
name: canvas-manual-to-video
description: Original production-grade skill for industrial operation videos. Turn uploaded equipment, assembly, or maintenance images and user instructions into an approved canvas manual, then generate one traceable video shot per step with direct source-image connections.
---

# ProcessShot: From Industrial Images to Instructional Video

Work in the node canvas selected by the user or currently available. Use the image, String/text, and video nodes that the actual canvas supports. Do not assume a platform-specific node type, API, or generation setting exists. If the canvas is inaccessible, identify the missing connection or permission, preserve the asset inventory and manual draft, and do not claim that nodes or links were created. Write node names, manuals, and shot prompts in English unless the user explicitly requests another language.

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

## Write a prompt for each shot

Write one prompt per approved step in the structure below. Replace every bracketed placeholder with information supported by that step; do not send placeholders to the video node. Use the canvas's actual image-reference syntax, such as `@image-node`, and connect those same image nodes directly to the video node. Keep consecutive actions within one step in the same shot rather than creating extra shots.

```text
Video Content and Visuals
Preserve the equipment appearance, component details, relative positions, and overall orientation shown in (@images assigned to this step). Generate an approximately [duration]-second [user-requested or source-consistent visual style] operation demonstration. [Camera treatment; prefer a fixed camera if none is specified.] Treat source-image step numbers, arrows, and black or red annotation lines as references only; exclude them from the final frame unless the user asks to retain them.

Operation Sequence
[Describe the confirmed actions in order, including supported tool-to-part contact points, movement direction, pace, and visible completion state.]

[Insert the optional Narration and Sound Effects section only when requested.]

Visual Constraints
Unless explicitly requested, show no subtitles, titles, corner labels, step numbers, diagram lines, brand marks, watermarks, or other text overlays. Preserve the key objects, operation direction, and before/after states. [Add user-specific constraints.]
```

When the user requests narration and/or sound effects, insert an audio section between `Operation Sequence` and `Visual Constraints`. Title it `Narration and Sound Effects` when both are requested, or `Narration` / `Sound Effects` when only one is requested. Write narration line by line from this shot's actual action. Follow the requested language, voice, and pace; if unspecified, use the manual's language and a clear, natural delivery. Synchronize each line with the corresponding action. Describe only sounds that the shot's actual contacts and movements would produce, synchronized to the visuals. Do not reuse sounds from an unrelated example. If neither narration nor sound effects are requested, remove the entire section, including its heading and placeholder. Do not add background music or other voices unless requested.

Engineering 3D rendering, a fixed camera, and a specific duration are example parameters, not universal requirements. Distinguish graphic annotations in a source diagram from real equipment details or operationally necessary markings. Before generation, verify that the prompt and image links refer to the same step, the movement direction matches the approved manual, and no unrelated tool, part, or action was copied from an example.

## Produce and approve the first shot

After manual approval, create and generate only `SHOT-01 | Step Name` for step 01. Use the shot prompt structure above and the user's specified style, aspect ratio, duration, and other settings. Choose consistent available settings for unspecified parameters and record them in the manual or node. Connect every assigned image node directly to the shot's video node. If supported, also link the relevant manual step. Check the image links, asset contents, action sequence, and prompt before generation.

Check that the first shot plays, follows its step, and preserves the key objects and operation states. Show it to the user and wait for explicit approval. If revisions are requested, correct and regenerate the first shot. Do not generate steps 02 through N before that approval.

## Generate the remaining shots and verify delivery

Only after first-shot approval, generate steps 02 through N from the approved manual. Create exactly one separate video shot node per step and connect every image assigned to that step directly to its video node. Reuse the approved visual and technical settings unless the manual specifies an exception. Track the step number, source-image nodes, generation status, and output node for each shot. Retry only failed or revised shots; do not resubmit successful generation jobs.

Before delivery, verify:

- Every image node has a unique, accurate name, and the manual node opens to the approved version.
- N numbered manual steps map to N video shots with matching order and actions, without omissions or extras.
- Every shot has actual links from its assigned source images, with no wrong, broken, or merely mentioned links.
- Videos play and show their assigned steps. Report failures, unusable assets, or open questions instead of calling incomplete work delivered.

If the platform cannot create a real image-to-video connection, pause generation of the affected shot and explain the limitation. If approved steps or image mappings change, update and reapprove the manual. If the change affects the first shot's key visual conventions, remake and reapprove that shot before batch generation resumes. Report the manual version, total shot count, image mapping, and completion status. Do not combine separate shots into one final film unless the user asks.
