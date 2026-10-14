# SPL-Bench — Sanskrit prompts (copy-paste)

All scored evaluation prompts/passages/answers are in simple Sanskrit. Do not translate to English at test time.
Exception: `prayoga`-track items may contain documented Hindi/English/IAST spans by design.
(Meta-docs — schema notes, judge settings, `explanation_sa` — may use Hindi/English.)

## MCQ / short answer

```
निर्देशः — केवलं सम्यक् उत्तरं लिखत, स्पष्टीकरणं मा ददत।
गद्यांशः: {passage_sa}
प्रश्नः: {prompt_sa}
विकल्पाः: {choices}
उत्तरम्:
```

## व्याकरण-शुद्धिः

```
निर्देशः — अशुद्धं वाक्यं शुद्धं कृत्वा केवलं शुद्धं वाक्यं लिखत।
अशुद्धम्: {prompt_sa}
शुद्धम्:
```

## सारांशः

```
निर्देशः — अधोलिखितगद्यांशस्य सारांशं लिखत। नामानि, संख्याः, तथ्यानि मा परिवर्तयत, नवीनां सूचनां मा योजयत।
गद्यांशः: {passage_sa}
सारांशः:
```

## लेखनम्

```
निर्देशः: {prompt_sa}
शब्दसीमां पालयत। शुद्ध-स्पष्ट-संगठितसंस्कृते लिखत।
```

## grounded QA (anti-hallucination, MCQ सहित)

```
निर्देशः — उत्तरं केवलं दत्तेभ्यः अंशेभ्यः ददत, स्रोतं [अंश 1]/[अंश 2] उद्धृत्य लिखत। अंशेषु उत्तरं न चेत् लिखत: सूचना उपलभ्या नास्ति।
अंशाः: {passage_sa}
प्रश्नः: {prompt_sa}
विकल्पाः (यदि सन्ति): {choices}
उत्तरम्:
```
