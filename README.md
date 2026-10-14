# TCM Herb Toxicity Dataset

**229 traditional Chinese medicine (TCM) herbs with expert-curated toxicity labels**,
assembled from [TCMToxDB](https://www.sdu-idea.cn/TCMToxDB) — the integrated TCM
toxicology platform developed by Shandong University — and cross-referenced against five
major TCM/bioinformatics databases (HERB, SymMap, TCMID, TCMSP, TCM-ID).

The companion analysis lives in the Kaggle notebook
[TCM Herb Toxicity Prediction](https://www.kaggle.com/code/zhenruiweng/tcm-herb-toxicity-prediction);
the dataset itself is published on
[Kaggle](https://www.kaggle.com/datasets/zhenruiweng/traditional-chinese-medicine)
(Kaggle **Datasets Expert bronze medal**, usability 10.0 / 10.0).

## Why this dataset exists

This dataset is self-processed: it was extracted and curated by the author from the
original TCMToxDB export. The most critical target variable — the **toxicity label** of
each herb — is largely missing in the raw database. Instead of conventional statistical
imputation (mode filling etc.), the missing labels were completed by hand:

1. **Extensively retrieve** ancient Chinese medical classics, materia medica monographs,
   pharmacopeia records and modern toxicology literature for every herb with a missing
   toxicity label;
2. **Summarize** the documented toxic properties of each herb from authoritative medical
   materials;
3. **Manually supplement three-level toxicity labels** (Severely Toxic / Toxic / Slightly
   Toxic) one by one to complete the missing target values.

In other words: every row's target value is either the original database annotation or a
literature-backed manual annotation — no statistical filling anywhere.

## Quick facts

| | |
|---|---|
| Herbs | **229** (unique pinyin/CN/EN names) |
| Columns | **31** (herb identity, TCM theory, target, database IDs, ingredient records) |
| Target | `Toxicity` — 4 levels, 100% coverage |
| Source | [TCMToxDB](https://www.sdu-idea.cn/TCMToxDB) (Shandong University), self-processed |
| License | Apache-2.0 (same as the Kaggle page) |

### Toxicity distribution

| Label | Count | Share |
|---|---:|---:|
| Toxic | 91 | 39.7% |
| Nontoxic | 77 | 33.6% |
| Slightly Toxic | 39 | 17.0% |
| Extremely Toxic | 22 | 9.6% |

A side observation for modeling: `Clinical_manifestations` is filled for 99 herbs
(43%), and is *not* concentrated in the toxic classes — 51 of the 99 documented herbs are
labeled Nontoxic. Toxicity documentation and clinical manifestation documentation are two
different things, and the dataset preserves that distinction.

## Column dictionary

### Herb identity (4 columns)

| Column | Coverage | Description |
|---|---:|---|
| `Herb_pinyin_name` | 100% | Chinese pinyin of the herb name |
| `Herb_cn_name` | 100% | Herb name in Chinese characters |
| `Herb_en_name` | 100% | English herb name |
| `Herb_latin_name` | 90% | Pharmaceutical Latin name |

### TCM theory attributes (5 columns)

| Column | Coverage | Description |
|---|---:|---|
| `Properties` | 57% | Taste and nature (e.g. "Warm; Pungent; Bitter") |
| `Meridians` | 55% | Meridian tropism — the body channels the herb acts on |
| `UsePart` | 75% | Part of the plant used (root, leaf, …) |
| `Function` | 88% | Therapeutic function (TCM action summary) |
| `Indication` | 95% | Conditions the herb is used to treat |

### Target & safety (2 columns)

| Column | Coverage | Description |
|---|---:|---|
| `Toxicity` | 100% | **Target.** Toxic / Slightly Toxic / Extremely Toxic / Nontoxic |
| `Clinical_manifestations` | 43% | Documented adverse/clinical manifestations |

### Therapeutic classification (1 column)

| Column | Coverage | Description |
|---|---:|---|
| `Therapeutic_en_class` | 75% | Standardized English therapeutic class (44 classes, e.g. "Antipyretic Detoxicate Drugs") |

### Herb-level database cross-references (5 columns)

IDs link each herb to its entry in the five reference databases, enabling joins with
ingredient/target databases:

| Column | Coverage | Database |
|---|---:|---|
| `HERB_id` | 100% | [HERB](http://herb.ac.cn/) |
| `SymMap_id` | 61% | [SymMap](http://www.symmap.org/) |
| `TCMID_id` | 99% | [TCMID](http://www.megabionet.org/tcmid/) |
| `TCMSP_id` | 59% | [TCMSP](https://tcmspweb.com/) |
| `TCM_ID_id` | 73% | [TCM-ID](https://www.bidd.group/TCMID/) |

### Ingredient-level records (14 columns)

For the 61 herbs whose *ingredients* are individually documented, the ingredient
name/aliases/formula/SMILES/molecular weight and the ingredient's own IDs across the
databases. These columns are sparse by construction (7%–27%) — missing means "no
ingredient record was collected for this herb", not "data loss".

`Name`, `Alias`, `Formula`, `Simles`, `Weight`, `CAS_ID`, `PubChem_ID`, `DrugBank_ID`,
`HERB_ID`, `SymMap_ID`, `TCMID_ID`, `TCMSP_ID`, `TCM-ID_ID`, `HERB_ID_single`.

## Notes for users

- **`Simles`** is the SMILES column — the column-name typo is preserved verbatim from the
  published Kaggle version so the two copies stay drop-in compatible.
- `TCMID_id` / `TCMSP_id` / `TCM_ID_id` are stored as floats in the raw CSV (a pandas
  export artifact of numeric IDs); cast if you need exact string IDs.
- The manual label vocabulary in the Kaggle description reads "Severely Toxic / Toxic /
  Slightly Toxic"; the corresponding label values present in the data are
  "Extremely Toxic / Toxic / Slightly Toxic".
- No data was dropped for missingness: rows with absent TCM-theory attributes are kept
  with empty values, which is itself informative (literature silence about a herb).

## Files

```
├── data/TCM.csv    # the complete dataset (229 rows × 31 columns, UTF-8)
├── README.md
└── LICENSE         # Apache-2.0
```

## Citation / acknowledgment

- Upstream source: **TCMToxDB** — [https://www.sdu-idea.cn/TCMToxDB](https://www.sdu-idea.cn/TCMToxDB),
  Shandong University. This dataset is a self-processed extract; please cite the upstream
  database for any scientific use.
- Kaggle mirror: [Traditional Chinese Medicine Toxicity Dataset](https://www.kaggle.com/datasets/zhenruiweng/traditional-chinese-medicine).

## License

[Apache-2.0](LICENSE) — identical to the license declared on the Kaggle dataset page.
