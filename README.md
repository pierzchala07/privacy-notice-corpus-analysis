# "Clear and Plain Language"? A Corpus-Based Test of GDPR Privacy Notices Against Plain-Language Benchmarks

A computational corpus linguistics study comparing GDPR-mandated corporate privacy notices against gov.uk plain-language public-information pages.

---

## Research question

Under GDPR Article 12, privacy notices must be communicated *"in a concise, transparent, intelligible and easily accessible form, using clear and plain language."* This project asks the descriptive question: do they? Therefore, whether contemporary corporate privacy notices exhibit the surface features of plain-language writing characteristic of comparable public-information texts.

The notices are compared against a corpus of gov.uk plain-language pages on the same subject matter (data protection, FOI, personal-information charters, ...),  drawn from the UK Government's central digital service. Content published on gov.uk is required to follow the official Writing for GOV.UK guidance and the GOV.UK Style Guide, which prescribe plain-English principles like short sentences, active voice, common vocabulary, etc. The gov.uk corpus therefore functions as a documented example of UK public-sector writing produced under an enforced plain-language standard, rather than as a normative ground truth.


---

## Corpus

| Group | Size | Source |
|---|---|---|
| `notice` | 10 documents | Corporate privacy notices: Airbnb, Amazon, Apple, Booking.com, Google, Microsoft, Netflix, Spotify, TikTok, Twitter/X |
| `plain` | 10 documents | gov.uk pages on data protection, FOI, and personal-information charters |

**The privacy notice texts and  gov.uk plain-language pages are not republished in this repository for copyright reasons.** The full list of sources with URLs is in [`corpus_sources.md`](corpus_sources.md). 

---

## Method

Both groups are cleaned with the same function (heading and bullet normalisation, list-line punctuation, whitespace), then ten features are extracted per document and collapsed to group means:

| Feature | What it measures | Source |
|---|---|---|
| Flesch Reading Ease | Surface readability: higher = easier (target ≈ 60) | Flesch (1949) |
| Flesch-Kincaid Grade | School-grade reading level: higher = harder (target ≈ 8) | Weiss (2007)|
| Avg. sentence length | EU guide Hint 4 - *Keep It Short and Simple*: longer = harder (target ≤ 20 words) | European Commission (2015) |
| Noun density | EU guide Hint 6 - *cut out excess nouns - verb forms are livelier*; high noun + low verb density indicates nominalisation (verbs turned into abstract nouns, a bureaucratic style) | European Commission (2015) |
| Passive voice rate | EU guide Hint 7 - *prefer active verbs*; higher = less direct| European Commission (2015) |
| Formal phrase rate (per 1k) | Counts phrases flagged by EU guide Hint 4 as overly formal (plus an extended legalese list); higher = more formal register | European Commission (2015) |
| Deontic / epistemic modal rate | Obligation (shall, must): high rate signals a regulatory/legal register, text that imposes duties. Hedging (may, might, could, would): high rate signals vagueness / non-commitment | Palmer (2001) |
| Type-Token Ratio | Lexical variety, on a 200-word random content-word sample; higher = more varied vocabulary |  McCarthy & Jarvis (2010) |

**Qualitative / lexical analysis:** 

- **Most frequent content words** - top nouns, verbs, and adjectives per group (stopwords removed), to surface thematic and register differences.
- **Collocations** - PMI-ranked bigrams and trigrams per group, to identify recurring formulaic phrasing (e.g. the repeated legal formulas privacy notices share across companies).
- **KWIC concordances** - each occurrence of typical privacy-notice formulas (*we may collect*, *third parties*, *in accordance with*) shown in context, so a phrase can be read in use rather than counted in the abstract.

---

## Findings

The full per-document feature table is saved as [`corpus_features.csv`](corpus_features.csv); the notebook ([`data_modelling.ipynb`](data_modelling.ipynb)) contains all tables and both charts. Main findings:

![Group means across all features](figures/feature_comparison.png)

| Feature | Notices (mean) | Plain (mean) | Direction |
|---|---|---|---|
| Flesch Reading Ease | 34.5 | 50.6 | notices harder to read |
| Flesch-Kincaid Grade | 13.7 | 9.7 | notices need more years of education to be read comfortably |
| Avg. sentence length | 25.0 | 16.0 | notices longer |
| Noun density | 0.285 | 0.257 | notices more nominalised |
| Passive voice rate | 0.12 | 0.14 | plain *more* passive |
| Formal phrases (per 1k) | 2.10 | 1.44 | notices more formal |
| Deontic modals (per 1k) | 0.38 | 3.37 | plain *more* obligation-marked |
| Epistemic modals (per 1k) | 9.10 | 6.49 | notices hedge more |
| Type-Token Ratio | 0.71 | 0.64 | notices more lexically varied |

On the core readability measures, the privacy notices read markedly harder than the gov.uk plain-language pages: lower Flesch Reading Ease, higher reading grade, longer sentences, and denser nominalisation. They also use more formal phrasing and more epistemic hedging.

Two results run counter to the expectation that notices would score worse on every measure, and are worth examining more closely:

- **Deontic modals are far more frequent in the plain group** (3.37 vs 0.38 per 1k). Obligation-marking (*must*, *shall*) clusters in the gov.uk pages, which describe legal duties directly, rather than in the notices.
- **Passive voice is slightly higher in the plain group** - The likely explanation is topical rather than stylistic: data-rights content centres on what happens to personal data (data is processed, erased, retained,...), where the affected entity, not the agent, is the natural subject.

Per-document readability shows the spread behind the group means:

![Readability per document](figures/readability_per_document.png)

Every document - both groups - sits below the ≈60 plain-language target. The gov.uk pages cluster near it; the notices do not. Two notices (Spotify, Google) reach into the plain-language band, showing the boundary is not absolute.

**Qualitative / lexical analysis:** 

- **Most-frequent content words** - Both groups centre on *information* and *data*, but diverge sharply in register. The notices lean on product-and-platform vocabulary (*services, account, device, platform, content, settings, payment, partners*) and their top verbs describe what the company does (*use, provide, collect, process, disclose*). The plain pages lean on rights-and-process vocabulary (*request, right, protection, court, advice*) and their top verbs address the reader's actions (*ask, need, get, find, tell*). The adjective lists echo the registers: the notices favour qualifying legal modifiers (*certain, applicable, necessary, legitimate, relevant*), the plain pages reader-oriented ones (*sure, free, able, responsible*).
- **Collocations (PMI)** - The notices' top collocations are dense legal-technical formulas: *standard contractual clauses*, *European Economic Area*, *good faith belief*, *enter (an) enforceable contract* - the shared template text of cross-company compliance drafting. The plain pages' collocations are procedural and reader-facing: *get free legal advice*, *subject access request*, *right (to) object (to) processing*, *must tell*.
- **KWIC concordances** - Typical notice formulas are rare or absent in the plain corpus: *we may collect* appears 20 times across the notices and not once in the gov.uk pages; *third parties* 53 times vs 5; *in accordance with* 16 vs 2; *your information* 82 vs 13. Read in context, the notice occurrences are strikingly repetitive - near-identical *we may collect* frames recur across companies — supporting the picture of privacy notices as assembled from a shared stock of legal formulas rather than written afresh for the reader.
---

## Limitations

- **Small corpus (n = 10 per group).** Results are descriptive and indicative, not statistically inferential.
- **TTR is computed on a 200-word random content-word window** - smaller than the conventional 1,000-word window, chosen so the shortest gov.uk documents could be included. The small, randomly-drawn window makes TTR noisier and somewhat sensitive to sample size.
- **Rate-based features (formal phrases, deontic and epistemic modals)** are normalised per 1,000 words. This makes them comparable across document lengths but noisier for short documents, where a single phrase or modal shifts the rate substantially. 
- **Benchmark limitation** - the "plain" group is a documented example of UK public-sector plain-language writing on overlapping subject matter, not a normative ground truth.

---


## Bibliography
European Commission. (2015). How to Write Clearly. https://op.europa.eu/en/home

Flesch, R. (1949). The Art of Readable Writing. Harper & Row. 

McCarthy, P. M., & Jarvis, S. (2010). MTLD, vocd-D, and HD-D: A validation study of sophisticated approaches to lexical diversity assessment. Behavior Research Methods, 42(2), 381–392. https://doi.org/10.3758/BRM.42.2.381

Palmer, F. R. (2001). Mood and Modality. Cambridge University Press.

Regulation (EU) 2016/679 of the European Parliament and of the Council of 27 April 2016 on the Protection of Natural Persons with Regard to the Processing of Personal Data and on the Free Movement of Such Data, and Repealing Directive 95/46/EC (General Data Protection Regulation) (Text with EEA Relevance), 119 OJ L (2016). http://data.europa.eu/eli/reg/2016/679/oj

Style guides. (n.d.). Retrieved 9 June 2026, from https://guidance.publishing.service.gov.uk/writing-to-gov-uk-standards/style-guides/

Tone of voice. (n.d.). Retrieved 9 June 2026, from https://guidance.publishing.service.gov.uk/writing-to-gov-uk-standards/tone-of-voice/

Weiss, B. D. (2007). Health Literacy and Patient Safety: Help Patients Understand. AMA Foundation. https://catalog.nlm.nih.gov/discovery/fulldisplay/alma9914758213406676/01NLM_INST:01NLM_INST
