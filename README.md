# Brand Manager

## Workflow

1. **Persona.** `persona-builder` interviews you and writes your profile and voice guide to `persona/<name>/`.
2. **Stage.** Drop your images into `content/staging/`.
3. **Grid.** `grid-builder` picks and orders up to 9 images, copies them to `content/publish/`, and writes `grid.md` with the grid layout and a caption for each image.
4. **Post.** Drag and drop `grid.md` and the images in `content/publish/` into Muse, and ask Muse to create the grid on Instagram. Muse's connector has a daily cap of 5 rows, so plan how many rows you post in a day.
5. **Archive.** Tell Claude you posted the grid. It moves `content/publish/` into the archive and clears `content/staging/`.

## Layouts

By default, `grid-builder` spreads similar images apart. A layout lets you ask for a deliberate pattern instead, like the ones curated accounts use.

- **Create one:** ask Claude to create a layout. It asks which persona to add it to, then writes it to `persona/<name>/layouts/<layout>.md`. Layouts are per persona, so each account can have its own. They stay on your machine because `persona/` is git-ignored.
- **Custom lanes:** treat each column as a swim lane, for example `Landscape | Food Pic | Portrait/Selfie`. Claude asks for the lane of each column.
- **Other patterns:** rows unified by theme or color, warm/cool alternating rows, and a checkerboard.
- **Combine them:** you can stack layouts that control different dimensions, such as lanes (columns) plus warm/cool (rows). Only one layout per dimension is allowed, and a checkerboard can't be combined with lanes or rows.
- **Use one:** name the layout when you ask for a grid. If you ask for a layout without naming one, Claude lists the persona's layouts and asks.
- **Shortages:** if your photos can't fill every slot, such as too few food photos for a food column, Claude says so and asks before substituting. When rules conflict, lanes win and row rules are relaxed.

## Bring your own excerpts

When building a persona, you can give `persona-builder` a file of your own writing instead of (or in addition to) answering the voice questions.

Save it as `persona/excerpts.txt`. The file is listed in `.gitignore`, so it stays on your machine.

**Format**
- Plain text. Separate each excerpt with a blank line (two line feeds).
- A static post or caption is a block of plain text.
- A dialogue is a block where each line starts with a speaker label and a colon. Keep the lines of one exchange together with no blank line between them. Use the same label for yourself every time.

**Example**

```text
Golden hour at the pier. Worth every step of the climb.

Monday coffee, no phone, one good book. Starting the week slow on purpose.

Sam: Did you actually run 10k this morning?
Me: Eight. The last two were negotiations with my knees.
Sam: Still counts.
Me: That's what I told them.

Shipped the thing I've been putting off for three weeks. Turns out it took an afternoon.

Alex: What's your secret for staying consistent?
Me: Lowering the bar until I can't say no. Ten minutes counts.
```
