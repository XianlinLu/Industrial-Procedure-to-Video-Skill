# Industrial-Procedure-to-Video

**An original skill for production-grade industrial video workflows.**

ProcessShot turns site images and user-provided instructions into a traceable manual and a step-by-step instructional video. It is designed for equipment operation, assembly, maintenance, and standard work training. The workflow is platform agnostic: node creation, image connections, and video generation use the capabilities of the active canvas.

## Workflow

1. Inspect and uniquely name every source-image node.
2. Create an editable manual in a String or text node, with an image index and numbered operation steps.
3. Obtain the user's approval of the manual. Each numbered step maps to exactly one video shot.
4. Before video generation, use Lumina's interactive choice dialog to ask whether narration is needed. If it is, create an Audio Generation node, collect the voice preferences step by step, generate a 20–30 second audition, and obtain approval of the voice.
5. Generate a separate narration node for each shot from the approved voice reference. Skip the entire audio branch when narration is declined.
6. Generate the first shot and obtain the user's approval of its visual and procedural accuracy.
7. Generate the remaining shots only after that approval. Connect each shot to the images and matching narration assigned to its step.
8. Verify the shot count, step order, source connections, audio mapping, duration limits, and playable outputs. Wait for final shot-set approval before proceeding.

Each shot prompt follows `Video Content and Visuals`, `Operation Sequence`, and `Visual Constraints`. Add `Narration and Sound Effects` only when requested, based on the actions in that specific shot. Unclear images, unsupported technical details, and missing source material are flagged for review instead of being stated as facts. Changes to approved steps or image mappings trigger the relevant approval gate again.

For Lumina `dreamina-seedance-2-5` in `r2v` mode, ProcessShot validates the combined duration of all audio attached to each video request and keeps it at or below 30.0 seconds, providing margin below the model's 30.2-second limit. The longer audition is used only as a voice reference and is never attached directly to the final shot video.

ProcessShot automatically follows the language of the user's operating instructions across all visible canvas content, including node labels, manuals, shot prompts, review questions, and status summaries. An explicit language choice always takes priority. Stable IDs, model numbers, part numbers, standards, and source labels remain unchanged where translation could alter their meaning.

## Use

Import this repository as a skill, or place `SKILL.md` and `agents/openai.yaml` in a skill folder. In a compatible node canvas, upload the source images, provide the operation instructions, and invoke `canvas-manual-to-video`.

## Publishing fields

- **Scene tags:** Knowledge, Visual Design
- **Skill name:** Industrial-Procedure-to-Video
- **One-sentence introduction:** Turns industrial images and instructions into an approved manual and one source-linked video shot per step. [How to Use] Upload images, describe the steps, then approve the manual and first shot. [Scenarios] Equipment operation, assembly, maintenance, training. [Outputs] Manual and video clips.
- **Instructions for use:** Upload images and provide the operation instructions and preferred visual settings. Name every image node, create a String/text manual with an image index and numbered steps, and obtain approval of the manual. Before video generation, open Lumina's native interactive dialog and ask whether narration is needed. If yes, create an Audio Generation node, collect voice preferences one question at a time, generate a 20–30 second audition, and wait for approval. Use the approved voice reference to create one narration audio node per step; never connect the audition itself to a final video. Keep each `r2v` request's combined audio duration at or below 30.0 seconds. If narration is declined, skip all voice steps. Generate and approve the first shot before creating the rest, then wait for approval of the completed shot set before proceeding.

## Import compatibility

The repository uses supported text file types (`.md` and `.yaml`) and compatible ASCII file and folder names. It contains no `.skillignore` file. `SKILL.md` stays below the importer's 20,000-character limit.
