# SPL-Bench — Sanskrit Pathan-Lekhan Bench

**Sanskrit reading + writing comprehension for AI models.**

MIT-licensed, open, Sanskrit-first for scored content (prompts, passages,
answers). Sister bench to
[HPL-Bench](https://github.com/apoorvsingh090/hpl-bench) (Hindi Pathan-Lekhan
Bench) — same harness design, rebuilt for Sanskrit: sandhi, samasa, vibhakti,
linga-vachana, lakara, shastriya/vyavahara registers, lipyantara.
(Meta-docs — schema notes, judge settings, explanations — may use Hindi/English.)

SPL-Bench tests whether a model can **read simple Sanskrit gadya/katha and
write correct Sanskrit** — from primary `A1` to shastriya `C1`.

## Tracks (v0.1 seed: 50 items, target: 3000)

| track | what it tests | scoring |
|---|---|---|
| `pathana` | gadya/katha comprehension (seed: short passages; target 150–800 words) | MCQ accuracy + short-answer F1 |
| `anumana` | NLI, coref (`सा/तौ/ते`), subhashita-niti, muhavare (`अरण्यरोदनम्`, `कूपमण्डूकः`) | accuracy |
| `vyakarana` | sandhi (रामायणम्), samasa (राजपुरुषः), vibhakti, linga-vachana, lakara (seed: 1 exemplar each; target: paradigms + prakriya) | exact + token-F1 |
| `sara` | summarization, no hallucination (seed passages distinct from pathana) | LLM-judge + token-F1 informative |
| `lekhana` | nibandha, patra, samvada, suchana | rubric 1–5 LLM-judge + human audit |
| `prayoga` | shastriya/vyavahara styles, `त्वम्`-register, IAST→Devanagari, vaidika morphology (kavya: zero in seed, target) | accuracy + style-appropriateness |
| `grounded` | 2-snippet toys in seed (target: multi-doc), cite `[अंश 1]`, abstain `सूचना उपलभ्या नास्ति` | accuracy + abstention rate; long-form via judge |

Levels in seed v0.1.0: `A1:6 / A2:4 / B1:18 / B2:16 / C1:6`. Target adds kavya/shastra C1 + long-doc.

## Quickstart

```powershell
# validate schema
python src/validate.py --data data/dev/spl_dev.jsonl

# score your model (preds: {"id":..., "prediction":...} per line)
python src/evaluate.py --gold data/dev/spl_dev.jsonl --preds examples/sample_preds.jsonl
```

## Run it without a local GPU (Hugging Face cloud)

```powershell
# https://huggingface.co/join → https://huggingface.co/settings/tokens
$env:HF_TOKEN = "hf_..."

python src/run_hf_api.py --gold data/dev/spl_dev.jsonl --out preds.jsonl --model google/gemma-2-2b-it --limit 5
python src/run_hf_api.py --gold data/dev/spl_dev.jsonl --out preds.jsonl --model google/gemma-2-2b-it
python src/evaluate.py --gold data/dev/spl_dev.jsonl --preds preds.jsonl
```

Or locally via Ollama / transformers / vLLM — see `src/run_local.py --help`.
Free-tier note: HF free accounts get ~$0.10/month Inference Providers credit —
enough for the 50-item seed on small models; then pay-as-you-go.

Prompts are fixed Sanskrit templates — see `examples/prompts_sa.md`.
Contributing: `CONTRIBUTING.md`. Hidden test: `data/README.md`.

## Repo layout

```
sanskrit-bench/
  LICENSE  MIT
  README.md
  CITATION.cff
  CONTRIBUTING.md
  requirements.txt
  .gitignore
  schema/spl_schema.json
  data/dev/spl_dev.jsonl        # public dev (50 seed v0.1.0)
  data/canary.txt               # contamination canary, keep out of training
  data/README.md
  data/test_hidden/             # git-ignored, leaderboard only
  src/validate.py               # stdlib schema check
  src/evaluate.py               # stdlib scorer (auto part + aggregates)
  src/run_local.py              # run Ollama / HF-transformers / vLLM locally
  src/run_hf_api.py             # run on HF Inference Providers (no local GPU)
  examples/prompts_sa.md
  examples/judge_rubric_sa.md   # LLM-judge spec + bias controls
  examples/sample_preds.jsonl
```

## Scoring policy

* Auto (deterministic): MCQ/vyakarana accuracy (exact, Sanskrit-insensitive:
  chandrabindu~anusvara, ZWJ/ZWNJ dropped). Short-answer token-F1 informative only.
* Judge (semantic): `sara/lekhana/grounded-long` use `examples/judge_rubric_sa.md`
  with LLM-judge + 5% human audit (10% for C1/lekhana suggested).
* Report per-track + `PATHANA` (pathana+anumana+grounded) vs `LEKHANA`
  (vyakarana+sara+lekhana) + `PRAYOGA` robustness.
* `dev-public / test-hidden` (target 30/70). Canary in `data/canary.txt`.

## Limitations (seed)

* Passages are short toys (≤60 words), not 150–800; kavya coverage is one C1 item.
* n=50 → per-track CIs ±15–30%. Don't leaderboard on seed alone.
* Seed Sanskrit kept deliberately simple (sarala-samskritam); complex
  Paninian derivations are future work with scholar review.

## License

MIT — see `LICENSE`. Data seeds are original examples (`seed:v0.1.0`).
If you contribute textbook-derived items, note source + license in `source`.
