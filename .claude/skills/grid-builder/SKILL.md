---
name: grid-builder
description: This skill is used to construct a grid for Instagram leveraging photos in content/staging. It copies the chosen photos to content/publish and writes the grid plan there in Markdown format.
---
# Grid Builder

## Inputs
- Images (`.png`, `.jpg`, `.jpeg`, `.webp`) in `content/staging`. Ignore `.gitkeep`.
- A persona profile in `persona/` (e.g. `persona/michael-glitzos.md`). It holds information about the user and dialogue examples that coach the posting voice. Use these sections:
  - `Speech patterns` (voice characteristics, verbal tics, dialogue examples) for how captions should sound.
  - `Notes for writer` (interests, tone guidance) for what to write about and how.
  - `Identity` and `Background` for facts captions may rely on.

- Optionally, one or more layouts named by the user, from `persona/<name-slug>/layouts/<layout>.md`. A layout defines the dimension it controls (`columns`, `rows`, or `cells`), a slot map, the tags needed, rules, and shortage behavior. If the user asks for a layout without naming one, list the persona's `layouts/` files and ask which. Never create or edit layout files; the coordinator does that (see `CLAUDE.md`).

If `content/staging` has no images, or `persona/` has no profile, stop and tell the user. Do not guess. If `persona/` has more than one profile, ask which one to use.

## Workflow
1. List the images in `content/staging` and read the persona profile in `persona/`.
2. View each image and note its subject, dominant colors, and composition. If a layout was chosen, also tag each image with what the layout needs (such as lane or subject type, and warm or cool tone).
3. A grid is at most 3x3, so it holds at most 9 images. If `content/staging` has more than 9 images, select the 9 that work best together as a set (variety, balance, strongest images) and tell the user which were left out and why. Do not build more than one grid unless asked.
4. Order the images, 3 per row, with the top row first as it appears on the profile. If the image count isn't a multiple of 3, say so and ask whether to leave a short last row.
   - **No layout (default):** order for visual balance. Avoid placing similar colors, subjects, or compositions next to each other in a row or column.
   - **With a layout:** fill the slots the layout defines instead of the default rule. Two or more layouts may be stacked only if each controls a different dimension; a `cells` layout (checkerboard) can't combine with `columns` or `rows`, so say why and ask the user to pick. Check feasibility first: if the pool can't fill a slot (for example, too few images for a lane), say so before filling and ask before substituting. When rules conflict, `columns` lanes are hard rules and `rows` rules are preferences: relax the row rule first and report which cells were relaxed. Do not apply the default adjacency rule where it contradicts the layout.
5. Copy only the selected images (never more than 9) into `content/publish/` with `cp`. Never move them. Do not overwrite existing files in publish without asking.
6. Write the grid plan to `content/publish/grid.md` using the Output Format below. Links are relative to `grid.md`.
7. Write one caption per image in the persona's voice, modeled on its dialogue examples and tone guidance, listed after the table.
8. Report what was copied, the final order, and any files skipped or left out because of the 9-image limit.

## Output Format
This skill outputs a grid plan in the form of a markdown template.
The Markdown template has a table without a table header. Each row of the table has the
images in a relative link. After the table, there is a summary of captions.

- One table row per 3 images, with at most 3 rows (9 images total). Never output more than 9 images or 3 rows.
- When a layout is used, add one line under the title naming it and what it controls (for example `Layout: lanes, landscape | food | portrait`). Report any relaxed cells there too.
- Alt text describes what the image contains.
- Each image gets a block with the bold filename, an italic one-line description, and the caption.

| <br />        | <br />        | <br />        |
| ------------- | ------------- | ------------- |
| ![description of image 1](./image-1.png) | ![description of image 2](./image2.png) | ![description of image 3](./image-3.png) |

**image-1.png**
_brief description of what the image contains_
[Example of caption 1]

**image-2.png**
_brief description of what the image contains_
[Example of caption 2]

**image-3.png**
_brief description of what the image contains_
[Example of caption 3]

## Constraints
- A grid never exceeds 3x3: at most 9 images in `content/publish`, in the table, and in the caption list.
- A layout never changes the 3x3 cap.
- Never create, modify, or delete layout files. Never modify or delete files in `content/staging`.
- Do not invent facts or hashtags that the persona profile or the image doesn't support.
