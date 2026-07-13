# Mutational_Signatures

# Project 7: Mutational Signatures in African Ancestry Cancer Patients

**Author:** Karimat Adeola Busari  
**Portfolio:** Computational Cancer Genomics — African and Underserved Populations  
**Status:** Complete (v1.0) | Subgroup analysis in progress

---

## Biological Question

What mutational processes are driving cancer in patients of African ancestry, and how do they compare to COSMIC reference signatures derived predominantly from Western cohorts?

Mutational signatures are recurring patterns of DNA damage that reflect the underlying biological processes — environmental exposures, defective repair pathways, or endogenous cellular mechanisms — that have shaped a tumour's genome. By extracting these signatures using Non-negative Matrix Factorization (NMF) and matching them to the COSMIC v3.3.1 catalogue, we can identify which processes are most active in a given population.

---

## Why This Matters

The COSMIC reference signatures were derived predominantly from European and Asian cancer cohorts. African and African-ancestry populations are severely underrepresented in global cancer genomic databases — meaning the mutational processes most relevant to African patients may be uncharacterised, misclassified, or simply missing from current reference catalogues.

This project is designed as a reusable, population-aware pipeline that can be applied to any MAF-format cancer genomic dataset, with the explicit goal of eventually running on true continental African patient data as it becomes available through initiatives such as TACA (The African Cancer Atlas), H3Africa, and direct academic collaborations.

---

## Data Source

**AACR Project GENIE v19.0** — accessed via [Synapse](https://synapse.org) (syn7222066)

GENIE is a real-world cancer genomic registry from 19 leading cancer centres worldwide, containing somatic mutation data from over 200,000 patients across multiple cancer types.

**Important population note:** GENIE's public cohort does not include African institutional sites. The Black/African American patients in this dataset were treated at Western cancer centres — they are not equivalent to continental African patients. Environmental exposures, genetic ancestry, and cancer type distributions differ substantially. This analysis is therefore a methods proof-of-concept and a characterisation of Black/African American mutational processes, clearly labelled as such.

---

## Methods Summary

| Step | Description |
|------|-------------|
| Data filtering | Merged clinical + MAF files; filtered for Black/African American patients and SNPs only |
| Trinucleotide extraction | pyfaidx querying GRCh37 reference genome; pyrimidine strand normalisation |
| Mutation matrix | 11,269 patients × 96 trinucleotide contexts |
| NMF | scikit-learn NMF, k=5 selected via elbow method (reconstruction error) |
| COSMIC matching | Cosine similarity against COSMIC v3.3.1 SBS signatures (79 reference signatures) |

---

## Key Findings

| Extracted Signature | Best COSMIC Match | Cosine Similarity | Biological Process |
|---------------------|-------------------|-------------------|-------------------|
| Signature 1 | SBS54 | 0.812 | Sequencing artefact / high TMB |
| Signature 2 | SBS15 | 0.901 ⭐ | DNA mismatch repair deficiency |
| Signature 3 | SBS29 | 0.665 | Tobacco chewing exposure |
| Signature 4 | SBS10b | 0.730 | POLE proofreading mutations |
| Signature 5 | SBS5 | 0.730 | Age-related clock-like accumulation |

**Headline finding:** SBS15 (DNA mismatch repair deficiency) is the highest-confidence match (0.901), suggesting MMR deficiency is a significant mutational driver in this cohort. MMR-deficient tumours are eligible for pembrolizumab (immune checkpoint inhibitor) regardless of cancer type — raising questions about whether Black/African American patients with MMR-deficient tumours are being adequately identified and offered immunotherapy.

---

## How to Use This Template

This notebook is designed to be reusable. To run it on your own dataset:

**Step 1:** Open `Project7_Mutational_Signatures_Template.ipynb` in Google Colab

**Step 2:** Update the CONFIG cell (Section 3) with your own file paths:

```python
MAF_PATH = '/path/to/your/mutations.maf'
CLINICAL_PATIENT_PATH = '/path/to/your/clinical_patient.txt'
CLINICAL_SAMPLE_PATH = '/path/to/your/clinical_sample.txt'
REFERENCE_GENOME_PATH = '/path/to/GRCh37.fa'
COSMIC_PATH = '/path/to/COSMIC_v3.3.1_SBS_GRCh37.txt'
OUTPUT_DIR = '/path/to/output/folder/'
POPULATION_FILTER = 'Your population label'
RACE_COLUMN = 'Your ancestry column name'
N_SIGNATURES = 5  # Update after reviewing elbow plot
```

**Step 3:** Run all cells in order

---

## Repository Structure

```
Project7_Mutational_Signatures/
│
├── Project7_Mutational_Signatures_Template.ipynb   # Main reusable pipeline
├── README.md                                        # This file
│
└── outputs/
    ├── mutation_matrix_96.csv       # 11,269 patients x 96 contexts
    ├── signatures.csv               # 5 extracted NMF signatures
    ├── exposures.csv                # Patient signature exposures
    ├── cosmic_similarity_full.csv   # Full cosine similarity matrix
    ├── reconstruction_error.png     # NMF elbow plot
    └── signatures_plot.png          # Signature bar charts
```

---

## Requirements

```
python >= 3.8
pandas
numpy
scikit-learn
pyfaidx
matplotlib
tqdm
synapseclient
```

Install with:
```bash
pip install pandas numpy scikit-learn pyfaidx matplotlib tqdm synapseclient
```

---

## External Resources Required

| Resource | Source |
|----------|--------|
| AACR Project GENIE data | [synapse.org](https://synapse.org) — free registration required |
| GRCh37 reference genome | [UCSC hg19](https://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz) |
| COSMIC v3.3.1 SBS signatures | [cancer.sanger.ac.uk/signatures/downloads](https://cancer.sanger.ac.uk/signatures/downloads/) |

---

## Limitations

1. **Population mismatch:** Black/African American ≠ continental African. Environmental exposures and genetic ancestry differ substantially.
2. **Panel sequencing bias:** GENIE uses targeted gene panels, not whole genome sequencing. Mutation counts are lower and context distributions may be incomplete.
3. **Multi-centre heterogeneity:** Data from 19 centres with different panels introduces technical variability (reflected in SBS54 signal).
4. **Single NMF run:** Robust analysis would involve multiple random initialisations and stability assessment.

---

## Way Forward

When continental African cancer genomic data becomes available, this pipeline can be rerun by updating only the CONFIG cell. Expected differences in true African data include enrichment of HBV-associated signatures (hepatocellular carcinoma), HPV-driven signatures (cervical cancer), and potentially novel uncharacterised processes not yet in the COSMIC catalogue.

---

## Citation

If you use this pipeline, please cite:

- AACR Project GENIE Consortium. AACR Project GENIE: Powering Precision Medicine through an International Consortium. *Cancer Discovery*, 2017.
- Alexandrov et al. The repertoire of mutational signatures in human cancer. *Nature*, 2020.
- Mugo & Contino. Next-generation sequencing studies of somatic breast cancer genomes in women of African ancestry. *npj Breast Cancer*, 2025.

---

## Contact

Karimat Adeola Busari  
[LinkedIn](https://linkedin.com/in/karimatbusari) | 
