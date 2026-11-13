Report template for the quack-translation skill.

Write for someone reading fast, in the requester's language. Plain words, short sentences, no
codes the reader has to look up. The report opens with the answer and closes with the answer
again, this time with the reasons. A reader who stops after the first block still knows what
they are in for.

---

THE ANSWER
<Good to go | Good to go after fixes | Not ready>. <Two or three plain sentences: what this
translation is, whether it says what the source says for the intended reader, and the one
thing that most decides the answer. Add "Fluency only, no source." when that applies.>

WHAT I FOUND
Lenses used: <plain names, comma separated> (<picked by: table row "<row>" | requester>)

<Lens name in plain words, e.g. "Mistranslation: the target says something else">
  1. <Must fix | Should fix | Minor>. <Where: key, line, segment or cue>.
     Source: "<verbatim>"
     Target: "<verbatim>" (<gloss in the requester's language, when useful>)
     What is wrong: <one sentence>.
     Why it matters: <the consequence for the reader or the product, in one sentence>.
     Suggested: "<corrected target text>"
     How I know: <tested it | checked it against <glossary, style guide, source, previous version> | my reading, not tested | not checked, run: <command>>
  2. ...

<Next lens name>
  Nothing found.

<one block per lens used, in the order listed above; a lens with nothing keeps its heading
and says "Nothing found.">

WHAT CHECKED OUT
- <a check that holds, with its count, one line each, e.g. "All 214 keys present, none extra">

THE ANSWER, EXPLAINED
<Same answer word as the top>. <A short paragraph in plain language: why the answer is what it
is, which numbered items drive it, and what to fix first and in what order.>

ABOUT THIS REVIEW
Reviewed: <the target, in the requester's words> at <path or locator>, as of <hash, date or version>
Source: <path or locator>, as of <hash, date or version> | none, fluency only
Languages: <source locale> to <target locale>
References: <glossary, style guide, length limits, previous version> | none given
Asked for: "<the requester's words, verbatim>"
Reviewer made it or read the translator's notes first: no | yes

---

Priority words
- Must fix: the reader would understand or do something different from the source, the
  product breaks, or the text creates legal, safety or offensive exposure the source does not.
  Never "Must fix" on "my reading, not tested" alone.
- Should fix: a real error that does not change the outcome (term, register, locale format,
  consistency, unnatural phrasing), or a translator's claim that does not hold. Anything not
  tied to the ask, a reference or a source segment is "Should fix" at most.
- Minor: preference, wording, punctuation style, tidiness.

---

If the target or the ask cannot be found, write three lines and stop:

    Missing: <the target, the source, the ask, or several>.
    Put it here: <a path, or paste it in the message>.
    What I have so far: <the one thing already known about the ask>.

---

JSON copy, only when the run is a gate inside a goal loop or workflow. Appended after ABOUT THIS
REVIEW as one fenced json block. Same content as the text, nothing extra.

```json
{
  "answer": "good_to_go | good_to_go_after_fixes | not_ready",
  "fluency_only": false,
  "summary": "<the two or three sentences from THE ANSWER>",
  "lenses": { "used": ["mistranslation", "omissions", "additions", "blindspots", "..."],
              "picked_by": "table:<row> | requester" },
  "items": [
    { "n": 1, "lens": "mistranslation", "priority": "must_fix | should_fix | minor",
      "where": "<key, line, segment or cue>", "source": "<verbatim>", "target": "<verbatim>",
      "what": "<what is wrong>", "why": "<consequence>", "suggested": "<corrected target>",
      "how_i_know": "tested | checked | my_reading | not_checked",
      "settle_with": "<command, only when not_checked>" }
  ],
  "counts": { "must_fix": 0, "should_fix": 0, "minor": 0 },
  "checked_out": ["<check that holds, with its count>"],
  "explained": "<the paragraph from THE ANSWER, EXPLAINED>",
  "about": { "reviewed": "<what>", "at": "<locator>", "as_of": "<hash, date or version>",
             "source": "<locator> | none", "languages": "<src> to <tgt>",
             "references": ["<glossary, style guide, ...>"],
             "asked_for": "<verbatim>", "reviewer_made_or_pre_read": false }
}
```

A gate passes on `answer` alone. `counts.must_fix` lets a gate say "no Must fix items"
without parsing the list.
