# ProcessShot

**An original skill for production-grade industrial video workflows.**

ProcessShot turns site images and user-provided instructions into a traceable manual and a step-by-step instructional video. It is designed for equipment operation, assembly, maintenance, and standard work training. The workflow is platform agnostic: node creation, image connections, and video generation use the capabilities of the active canvas.

## Workflow

1. Inspect and uniquely name every source-image node.
2. Create an editable manual in a String or text node, with an image index and numbered operation steps.
3. Obtain the user's approval of the manual. Each numbered step maps to exactly one video shot.
4. Generate the first shot and obtain the user's approval of its visual and procedural accuracy.
5. Generate the remaining shots only after that approval. Connect each shot to the images assigned to its step.
6. Verify the shot count, step order, image connections, and playable outputs.

Each shot prompt follows `Video Content and Visuals`, `Operation Sequence`, and `Visual Constraints`. Add `Narration and Sound Effects` only when requested, based on the actions in that specific shot. Unclear images, unsupported technical details, and missing source material are flagged for review instead of being stated as facts. Changes to approved steps or image mappings trigger the relevant approval gate again.

## Use

Import this repository as a skill, or place `SKILL.md` and `agents/openai.yaml` in a skill folder. In a compatible node canvas, upload the source images, provide the operation instructions, and invoke `canvas-manual-to-video`.

## Publishing fields

- **Scene tags:** Knowledge, Visual Design
- **Skill name:** ProcessShot
- **One-sentence introduction:** Turns industrial images and instructions into an approved manual and one source-linked video shot per step. [How to Use] Upload images, describe the steps, then approve the manual and first shot. [Scenarios] Equipment operation, assembly, maintenance, training. [Outputs] Manual and video clips.
- **Instructions for use:** Upload images and provide the operation instructions, preferred visual style and duration, and whether narration or sound effects are needed. Name every image node, then create a String/text manual with an image index, numbered steps, assigned sources, and completion states. Obtain approval of the manual before video generation; the step count must equal the shot count. For each step, connect its images to its video node and write a prompt with Video Content and Visuals, Operation Sequence, and Visual Constraints. Add synchronized narration or realistic sound effects only when requested; otherwise omit the entire audio section. Generate the first shot and obtain approval before generating the rest in a batch. Finally verify shot count, order, source connections, and playback.

## Import compatibility

The repository uses supported text file types (`.md` and `.yaml`) and compatible ASCII file and folder names. It contains no `.skillignore` file. `SKILL.md` stays below the importer's 20,000-character limit.
