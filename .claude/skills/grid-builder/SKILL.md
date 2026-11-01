---
name: grid-builder
description: This skill is used to construct a grid for Instagram leveraging photos in content/staging. It copies the chosen photos to content/publish and writes the grid plan there in Markdown format.
---
# Grid Builder

## Inputs
- Images (`.png`, `.jpg`, `.jpeg`, `.webp`) in `content/staging`. Ignore `.gitkeep`.
- `voice.md` in the project root, which defines the voice and tone for captions.

If `content/staging` has no images, or `voice.md` is missing, stop and tell the user. Do not guess.

## Workflow
1. List the images in `content/staging` and read `voice.md`.
2. View each image and note its subject, dominant colors, and composition.
3. Order the images for visual balance, 3 per row, with the top row first as it appears on the profile. Avoid placing similar colors, subjects, or compositions next to each other in a row or column. If the image count isn't a multiple of 3, say so and ask whether to leave a short last row.
4. Copy the images into `content/publish/` with `cp`. Never move them. Do not overwrite existing files in publish without asking.
5. Write the grid plan to `content/publish/grid.md` using the Output Format below. Links are relative to `grid.md`.
6. Write one caption per image in the voice defined by `voice.md`, listed after the table.
7. Report what was copied, the final order, and any files skipped.

## Output Format
This skill outputs a grid plan in the form of a markdown template.
The Markdown template has a table without a table header. Each row of the table has the
images in a relative link. After the table, there is a summary of captions.

- One table row per 3 images.
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
- Never modify or delete files in `content/staging`.
- Do not invent facts or hashtags that `voice.md` or the image doesn't support.
