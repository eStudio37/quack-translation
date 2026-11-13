# Cheap checks

Mechanical checks that settle a finding without "my reading". Run them read-only, on copies in
scratch, with the files passed as arguments. Each one can turn up a Must fix. A clean result
goes in WHAT CHECKED OUT with its count.

The shell commands are examples. Where a tool is missing (ChatGPT without a shell, or a host
without `jq` or `msgfmt`), run the same check in Python. For example, key and placeholder
parity for two JSON files:

```python
import json, re
PH = re.compile(r"\{\{?[^{}]+\}?\}|%(?:\d+\$)?[sdif@]|</?[a-zA-Z][^>]*>|\\n")
def flat(d, p=""):
    for k, v in d.items():
        if isinstance(v, dict):
            yield from flat(v, f"{p}{k}.")
        else:
            yield f"{p}{k}", str(v)
src = dict(flat(json.load(open("src.json", encoding="utf-8"))))
tgt = dict(flat(json.load(open("tgt.json", encoding="utf-8"))))
print("missing:", sorted(src.keys() - tgt.keys()))
print("extra:", sorted(tgt.keys() - src.keys()))
for k in src.keys() & tgt.keys():
    if sorted(PH.findall(src[k])) != sorted(PH.findall(tgt[k])):
        print("placeholders differ:", k)
    if src[k].strip() and src[k] == tgt[k]:
        print("untranslated?:", k)
```

## Structure

- **Same keys or segments.** Every source key exists in the target and no extra keys appear.
  JSON: `jq -r 'paths(scalars) | join(".")' src.json | sort > a; jq -r 'paths(scalars) | join(".")' tgt.json | sort > b; comm -3 a b`.
  Prose: compare paragraph or sentence counts, then look at the segments where they diverge.
- **File still valid.** `jq . tgt.json`, `msgfmt -c -o /dev/null tgt.po`, `xmllint --noout tgt.xliff`,
  `plutil -lint tgt.strings`. A file that does not parse breaks the product: Must fix.
- **Subtitles.** Same cue count and timings as the source unless re-timing was asked.

## Content carried over

- **Placeholders and tags.** For each segment, the multiset of `{name}`, `%s`, `%1$d`, `{{x}}`,
  `<b>`, `</a>`, `\n` is identical in source and target. Order may change only when the format
  allows it (named or positional placeholders).
- **Plural and gender forms.** ICU `plural` or `select` blocks keep their keyword and every
  category the target language needs (for example `one`, `few`, `many`, `other` in Polish).
- **Numbers.** Extract every number from source and target per segment and compare after
  locale conversion (1,234.5 in en-US is 1.234,5 in pt-BR). A changed amount is Must fix.
- **Names, URLs, emails, codes.** Product names, URLs, emails, IDs and code samples appear
  unchanged unless the glossary says otherwise.

## Coverage

- **Untranslated.** Target segments identical to the source, or still in the source language,
  except names and codes that should stay.
- **Empty.** Target segments that are empty or only whitespace while the source is not.

## References

- **Glossary.** For each glossary source term found in the source, check that its approved
  target term appears in the matching target segment. Report misses with the segment.
- **Forbidden terms.** Grep the target for terms the style guide forbids.
- **Previous version.** When one exists, diff it against the new target and review every
  changed approved string for regression.

## Fit

- **Length.** Target length against the limit, or against the source times the expansion the
  ask allows. Flag strings over the limit with their length and the limit.
- **Subtitle speed.** Characters per second per cue against the ask, or 17 cps for adults
  when the ask names none, and characters per line against 42.
