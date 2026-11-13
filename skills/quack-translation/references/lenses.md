# Lenses for translations

Rules:
- Every run uses at least 5 lenses. More are allowed when the context demands it.
- The core 4 are always on.
- A known text type takes its row from the table. Every row has at least 5 lenses.
- A text that matches no row takes the `general prose` row, plus any buffet lens it plainly
  needs.
- The lens set is printed in the report header, with its source (table row or requester).

## Core, always on

| code | lens | question |
|---|---|---|
| M | mistranslation | Where does the target say something different from the source: wrong sense, false friend, flipped negation, changed number, date, amount, unit, name or who-does-what? |
| O | omissions | Which source segment, clause, qualifier, condition or warning is missing from the target, or left untranslated? Walk the segments in order. |
| U | additions | What does the target assert that the source does not: invented content, added promises, explanations, intensifiers or claims? |
| B | blindspots | What does this reach that nobody named: placeholders and markup, screens where it renders, legal or safety weight, other files that reuse the same strings? |

## Buffet

| code | lens | question |
|---|---|---|
| T | terminology | Does every term follow the glossary, termbase, product names and the previous approved version? |
| F | fluency | Would a native reader of the target locale find grammar, spelling, agreement, punctuation or phrasing errors? |
| Y | style and register | Do tone and formality fit the ask and the audience (você/tu, tú/usted, Sie/du, brand voice)? |
| K | locale conventions | Are numbers, decimal and thousands separators, dates, times, currency, units, addresses and quotation marks right for the target locale? |
| I | internal consistency | Is the same source term or repeated segment rendered the same way everywhere, and do cross-references still match? |
| H | markup and placeholders | Are every placeholder, variable, tag, escape, line break and plural or gender form (ICU, gettext) intact and in a valid position? |
| N | length and fit | Does the target fit its space: UI character limits, button widths, subtitle reading speed and line length, SMS segments? |
| C | cultural fit | Is anything offensive, confusing or wrong for the target culture: idioms, humor, images named in text, examples, holidays? |
| A | audience fit | Will the intended reader understand and act on this as the source intends? |
| X | compliance exposure | Does the target change a legal, regulatory, medical or contractual obligation, a defined term, or a required wording? |
| G | regression | Did an update change strings that were already approved, or break text that worked before? |

## Table: known text types

| text type | examples | lenses |
|---|---|---|
| software UI strings | JSON, .po, .xliff, .strings, .arb, resource files, app copy | M O U B T H N K I |
| technical documentation | manuals, help center, README, API docs, release notes | M O U B T F K I |
| legal or contractual | contracts, terms of use, privacy policy, regulatory notices | M O U B T I X K |
| medical or safety | instructions for use, labels, warnings, patient material | M O U B T K X A |
| marketing and brand | landing pages, ads, campaigns, store listings, slogans | M O U B Y F C A |
| subtitles and voice | SRT, VTT, dubbing scripts, captions | M O U B F N Y |
| correspondence | emails, client messages, announcements, support replies | M O U B F Y A |
| general prose | articles, essays, reports, anything else | M O U B F Y I |

Matching: pick the row whose examples or description the text clearly fits. If two rows fit,
take the union. If none fits, or the fit is a stretch, take the `general prose` row, add any
buffet lens the text plainly needs, and say so in the report header.
