# TutorVault

> An Obsidian vault wired to Claude Code as a Socratic AI tutor. Every conversation deepens an accumulating textbook of your own.

TutorVault turns an Obsidian vault into a long-lived, AI-tutored learning system. Instead of one-shot Q&A, the tutor (defined in `CLAUDE.md`) follows a strict workflow: generate a syllabus per subject, teach through Socratic dialogue, and synthesize each session into a stable textbook chapter that accumulates an audit trail of your understanding.

## What's in the box

```
TutorVault/
├── CLAUDE.md                 # The tutor system — read by Claude Code on every turn
├── Syllabi/                  # Syllabus - <Subject>.md, hierarchical [ ] checkboxes
├── Notes/                    # Notes/<Subject>/<Topic>.md, stable textbook chapters
├── Resources/Clippings/      # Imported source material (articles, PDFs, web clips)
├── Attachments/              # Images and other Obsidian attachments
└── skills/socratic/          # Optional /socratic skill (bundled copy)
```

## Why this exists

Most AI tutoring is amnesiac — every session starts from zero. TutorVault is the opposite:

- **Syllabi as state.** A `[ ]` / `[x]` checklist per subject is the persistent learning state. The tutor reads it at the start of each session and recommends the next topic based on a prerequisite chain.
- **Notes as accumulating textbook.** Each topic note has a stable body that is *not* overwritten. Every revisit appends a dated `> [!insight]` callout — your understanding history is the audit trail.
- **Resources before LLM curricula.** When you import a "core ideas index" of a field into `Resources/Clippings/`, the tutor seeds the syllabus from it rather than hallucinating one. Otherwise it marks the syllabus as unsourced.
- **Socratic by default.** No theorem dumps. The tutor builds intuition, asks one scaffolded question per turn, and only marks topics complete when you *demonstrate* understanding — not when you've been exposed.

The full ruleset lives in [`CLAUDE.md`](./CLAUDE.md).

## Quick start

1. **Install [Obsidian](https://obsidian.md/)** and **[Claude Code](https://claude.com/claude-code)**.
2. **Clone this repo** anywhere on disk:
   ```bash
   git clone <your-fork-url> TutorVault
   ```
3. **Open the folder as an Obsidian vault** (`Open folder as vault` from the Obsidian start screen).
4. **Launch Claude Code from the vault root.** Claude Code will pick up `CLAUDE.md` automatically.
   ```bash
   cd TutorVault
   claude
   ```
5. **Start learning.** Tell the tutor what subject you want to study, e.g. `I want to learn Linear Algebra`. The tutor will generate `Syllabi/Syllabus - Linear Algebra.md` and begin a Socratic session.

## Optional: install the `/socratic` skill

`CLAUDE.md` is self-contained, so the bundled skill is optional. It adds a stand-alone `/socratic` slash command for one-off dialogue sessions on any topic, with a structured Obsidian note generated at the end.

To install it for Claude Code:

```bash
mkdir -p ~/.claude/skills/socratic
cp skills/socratic/SKILL.md ~/.claude/skills/socratic/
```

Then invoke `/socratic <topic>` from any project, not just this vault.

## Typical workflows

### Start a new subject

```
> I want to learn Buffett-style value investing
```

The tutor:
1. Looks for an authoritative resource in `Resources/Clippings/`. If none, it generates an unsourced syllabus and says so.
2. Writes `Syllabi/Syllabus - Buffett Investing.md` with hierarchical `- [ ]` topics + a Learning Path block.
3. Proposes the first topic whose prerequisites are met. You confirm or pick another.

### Import a clipping

Drop a web article, paper excerpt, or "core ideas index" into `Resources/Clippings/`.

```
> I added Resources/Clippings/value-investing-index.md, can you ingest it?
```

The tutor maps it to a subject, extracts 3–7 sub-concepts, and inserts them as indented `- [ ]` items under the matching topic in the syllabus.

### Revisit a topic

```
> Let's go deeper on Compounding
```

The tutor reads `Notes/Buffett/复利（Compounding）.md`, opens a Socratic session about a corner you haven't fully grasped, and **appends** a dated `> [!insight]` callout under the frontmatter — the stable body is never overwritten.

### Get a quiz

```
> Quiz me on what I've covered in Linear Algebra
```

The tutor pulls questions from the `Socratic 练习题` sections of notes you've marked `[x]`, plus 1–2 cross-topic synthesis questions. Skipping is allowed; skipped items stay `[ ]`.

## Designing your own subject from scratch

The system is most powerful when you seed each subject with an **authoritative "core ideas index"** of the field — a flat list of the 20–50 concepts a domain expert would say constitute the discipline. Add it to `Resources/Clippings/` *before* asking the tutor to generate a syllabus. The tutor will use it as the spine of the syllabus instead of inventing topics from training data.

If you don't have an index, the tutor will generate one and label the syllabus unsourced. Treat it as a strawman to correct, not a truth.

## Note format (the textbook chapter)

Every topic note follows the same canonical structure — the body is your stable textbook chapter, and dated `[!insight]` callouts accumulate above it. See `CLAUDE.md` §5 for the full schema.

```markdown
---
tags: [<subject>, concept]
source: "[[Resources/Clippings/<original clipping>]]"
syllabus: "[[Syllabi/Syllabus - <Subject>]]"
related:
  - "[[<related concept>]]"
last_reviewed: YYYY-MM-DD
---

> [!insight] Socratic 對話精華（YYYY-MM-DD）
> <a new distinction or correction from this session>

# <Topic>

> <one-sentence anchor quote, if any>

## 一句话定义 / One-line definition
...

## 核心要义 / Core Insights
...

## 典型案例 / Cases
...

## 常见误区 / Common Misconceptions
...

## Socratic 练习题 / Quiz
...
```

## Contributing

Pull requests welcome. The most valuable contributions are:

- **Subject seed indexes** — authoritative "core ideas" lists for a field, dropped into `Resources/Clippings/` as starting points others can fork.
- **CLAUDE.md improvements** — sharper trigger rules, better accumulation patterns, cleaner edge-case handling.
- **Example syllabi and notes** — one well-built subject teaches the format better than any documentation.

## License

MIT — see [LICENSE](./LICENSE).
