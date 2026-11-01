---
name: persona-builder
description: Interviews the user about their platforms, background, goals, motivations, and interests, then writes a persona profile and voice guide to the persona folder from templates. Use to start the content process or when no persona exists yet.
---
# Persona Builder

Builds the persona that other skills (such as `grid-builder`) and agents use to write in the user's voice. It works by interviewing the user, then filling in two templates.

## Inputs
- Templates in `.claude/skills/persona-builder/templates/`:
  - `profile.template.md`: who the user is, where they publish, and why.
  - `voice.template.md`: how the user sounds, with dialogue examples.
- The user's answers during the interview. Ask the user directly; do not run this as a subagent.
- Optionally, a bring-your-own excerpts file at `persona/excerpts.txt` (git-ignored). It is plain text with excerpts separated by a blank line:
  - A block of plain text is a static post or caption.
  - A block where each line starts with a speaker label and a colon (`Sam: ...`) is a dialogue. Lines of one exchange are kept together with no blank line between them.

## Outputs
Write to `persona/<name-slug>/`, for example `persona/michael-glitzos/`:
- `profile.md`
- `voice.md`

If `persona/<name-slug>/` already has these files, read them first, show what exists, and ask whether to update or start over. Never overwrite without confirmation.

## Workflow
1. Read both templates. They define the sections to cover and the shape of the output.
2. Ask the user's name and use it for the folder slug (lowercase, hyphenated). Then check whether `persona/excerpts.txt` exists. If it does, ask whether to use it. If it doesn't, mention that they can add one (format in `README.md`) and continue either way.
   - Split the file on blank lines. Treat blocks of 2+ lines that all start with `Label:` as dialogues, and everything else as static posts.
   - For dialogues, ask which label is the user. Use only the user's lines as voice evidence; other speakers' lines are context for how the user responds.
   - Derive voice evidence from the excerpts (tone, sentence length, emoji and hashtag habits, vocabulary, tics, and how voice shifts between posts and conversation) and confirm it with the user instead of asking those voice questions from scratch.
3. Interview in rounds, 2-4 questions each. Use multiple choice (`AskUserQuestion`) where the answers are a known set, and plain questions for open-ended ones. Run the rounds in this order:
   1. **Platforms and audience:** where they publish, handles, how often, who they want to reach.
   2. **Background:** occupation, location, history and expertise relevant to content.
   3. **Goals and motivations:** why they post, what success looks like, what they care about.
   4. **Hobbies and interests:** what they do outside work and how it could show up in content.
   5. **Voice:** tone, formality, humor, emoji and hashtag habits, words they love or avoid.
   6. **Voice samples:** if an excerpts file was used, skip this round and instead have the user pick the excerpts that sound most like them. Otherwise ask for 3-5 real captions, messages, or posts they've written. If they have none, show 3-4 short sample lines in different tones and ask them to react, then have them rewrite one in their own words.
4. Follow up on vague answers ("professional", "authentic") by asking for a concrete example. Skip questions already answered.
5. Derive 3-5 content pillars from the answers and confirm them with the user.
6. Fill in `profile.template.md` and `voice.template.md`, removing the `{{ }}` placeholders and HTML comments. Delete any section the user gave no information for rather than leaving it blank.
7. Show both drafts to the user and apply their corrections.
8. Save the files to `persona/<name-slug>/` and tell the user the paths.

## Constraints
- Only record what the user said or confirmed. Do not invent biography, quotes, or preferences.
- Copy excerpts into `voice.md` verbatim, and only the user's own lines. Do not copy other speakers' lines or names from a dialogue; show them as `[other person]` for context if needed.
- Never commit or move `persona/excerpts.txt`.
- Dialogue examples must be the user's own words or lines they approved. Mark any line Claude drafted that the user approved as such in its context line.
- Do not ask for sensitive details (home address, finances, health, other people's private information). Record anything the user lists as off-limits under `Off-limits`.
- Do not modify the files in `templates/`.
