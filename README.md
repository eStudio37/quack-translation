# quack-translation

An agent skill that QAs one finished translation against its source text and the request that
produced it, and returns a plain-language report that opens with the answer. It is the
translation variant of `quack`, written for Codex and ChatGPT and usable by any agent that
reads `SKILL.md` skills or `AGENTS.md`.

## What it does

- Collects the target, the source, the language pair and the ask. If one is missing, it says
  what is missing and stops instead of guessing. Without the source it offers a fluency-only
  review and labels it as such.
- Reviews through at least five lenses. Four are always on: mistranslation, omissions,
  additions and blindspots. The rest come from the text type: software UI strings, technical
  documentation, legal, medical or safety, marketing, subtitles, correspondence, general
  prose. Definitions and the table are in `skills/quack-translation/references/lenses.md`.
- Runs cheap mechanical checks before judging: keys and segments, file validity, placeholders
  and tags, plural forms, numbers, untranslated text, glossary terms, length limits.
- Rates each finding as Must fix, Should fix or Minor. A Must fix needs evidence: a check, a
  reference or a quoted source segment. Back-translation alone never proves one.
- Report only. It proposes corrected text and applies none.

## Install

Codex CLI or IDE extension, for all your projects:

```
git clone https://github.com/eStudio37/quack-translation /tmp/quack-translation
mkdir -p ~/.agents/skills
cp -r /tmp/quack-translation/skills/quack-translation ~/.agents/skills/
```

For one repository, copy the folder to `.agents/skills/quack-translation` in that repository
instead. Restart Codex if the skill does not show in `/skills`.

ChatGPT desktop app: add the `skills/quack-translation` folder as a skill from the Skills
sidebar.

Agents without skill support: copy the "Translation QA" section of `AGENTS.md` into your
project's `AGENTS.md` and point `SKILL_DIR` at the copied folder.

Claude Code: copy `skills/quack-translation` to `~/.claude/skills/quack-translation`. The
`agents/openai.yaml` file is ignored there.

## Use

```
$quack-translation Source: docs/en/terms.md. Target: docs/pt-BR/terms.md. Languages: en-US to pt-BR. Audience: customers signing up. References: glossary.csv.
```

In ChatGPT type `@quack-translation`. Plain requests work too: "QA this translation", "check
the pt-BR version against the English".

## The report

Five sections, in this order (template in `skills/quack-translation/assets/report-fast.md`):

1. THE ANSWER: Good to go, Good to go after fixes, or Not ready, plus two or three sentences.
2. WHAT I FOUND: the lenses used, then numbered findings per lens, each with the source,
   the target, the problem and the suggested text.
3. WHAT CHECKED OUT: checks that hold, with counts.
4. THE ANSWER, EXPLAINED: why, which findings drive it, what to fix first.
5. ABOUT THIS REVIEW: target, source, languages, references, the ask verbatim.

Inside an automated loop or workflow, the same report is also appended as one JSON block, so a
script can read the answer.

## Files

- `skills/quack-translation/SKILL.md`: the procedure.
- `skills/quack-translation/agents/openai.yaml`: display name, default prompt and invocation
  policy for Codex and ChatGPT.
- `skills/quack-translation/references/lenses.md`: lens definitions and the text-type table.
- `skills/quack-translation/references/checks.md`: the mechanical checks, with shell and
  Python examples.
- `skills/quack-translation/assets/report-fast.md`: the report template and the JSON shape.
- `AGENTS.md`: a drop-in section for `AGENTS.md`, and notes for working on this repository.

## Limits

- No automated tests of the skill yet.
- One translation against one source per run.
- The reviewer's command of the target language bounds the fluency and register findings.
  Meaning findings are backed by checks or quoted segments.

## License

MIT. See `LICENSE`.
