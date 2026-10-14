# Contributing to SPL-Bench

SPL-Bench measures Sanskrit **reading (पठनम्)** and **writing (लेखनम्)**.

## What we accept

* Simple, correct Sanskrit items (no raw MT). Classical sources (Panchatantra,
  Subhashita, NCERT Sanskrit textbooks) must be noted in `source` with license.
* One item = one JSON line matching `schema/spl_schema.json`.
* Every item needs: `id`, `track`, `level`, `prompt_sa`, `answer` or `rubric`, `source`.
* Keep seed Sanskrit simple (sarala-samskritam) to minimize errors; mark
  kavya/shastriya items `B2` and above.

## Levels

* `A1/A2` — primary literacy, simple sentences
* `B1/B2` — secondary (NCERT Sanskrit 8-12)
* `C1` — advanced (kavya, shastra, multi-doc reasoning)

## Tracks

| track | meaning |
|---|---|
| `pathana` | passage comprehension (gadya, katha) |
| `anumana` | inference, NLI, coref (सः/तौ/ते), subhashita-niti |
| `vyakarana` | sandhi, samasa, vibhakti, linga-vachana, lakara, shabda-rupa |
| `sara` | summarization with grounding |
| `lekhana` | nibandha, patra, samvada, suchana (rubric-scored) |
| `prayoga` | kavya/shastra/vyavahara styles, lipyantara (IAST→Devanagari) |
| `grounded` | multi-doc QA with citation, anti-hallucination |

## Quality bar

1. `python src/validate.py --data data/dev/spl_dev.jsonl` must pass.
2. No duplicate prompts. MCQ distractors plausible, single correct answer.
3. Writing prompts must include a 1-5 rubric (`shuddhata, spashtata, sangathana, upayuktata`).
4. Every sandhi/samasa/vibhakti answer double-checked against a standard
   grammar (Apte / NCERT / sanskritdictionary.com).

```powershell
python src/validate.py --data data/dev/spl_dev.jsonl
python src/evaluate.py --gold data/dev/spl_dev.jsonl --preds examples/sample_preds.jsonl
```

Hidden test (`data/test_hidden/`) is maintained by maintainers only.
