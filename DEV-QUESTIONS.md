# Questions for the QC-system developer

The generator emits portable regex, but a few behaviours depend on the engine your QC
system actually runs. Please confirm:

1. **Which regex engine/library** does the QC system use? (Python `re`/`regex`, PCRE,
   Java, .NET, Go RE2, JavaScript, …)
2. **Inline flags** — does it support `(?i)`, or must case-insensitivity be set as a
   flag outside the pattern (e.g. `IGNORECASE`)?
3. **Lookahead/lookbehind** — does it support `(?=…)` and `(?!…)`? *(Required for the
   exclusion feature, e.g. match `babi` but not `minyak babi`. Go RE2 does NOT support these.)*
4. **Anchoring** — is matching done as `search` (substring, unanchored) or full `match`
   (anchored)? This affects whether `^…$` anchors are needed.
5. **Default case sensitivity** — are patterns applied case-insensitively by default?
6. **Unicode** — are `\w` / `\b` Unicode-aware, or ASCII-only? (matters for Indonesian
   text and accented characters)
7. **Limits** — confirmed max pattern length is **255**? Any limit on alternation size
   or per-message performance?
8. **Ingestion format** — how are patterns added? One regex per row, OR-combined per
   category, bulk import? What is the exact field format?
