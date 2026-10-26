# Phylodynamics & Pathogen Genomic Surveillance (SARS-CoV-2 time-scaled tree)

Building the real computational backbone of genomic disease surveillance: turning a dated set of real
SARS-CoV-2 genomes into a time-scaled evolutionary tree, a molecular clock rate, and an effective-population
trajectory — the same real pipeline real public-health genomic surveillance programs run.

## 1. Aim

Given a real, date-stratified sample of public SARS-CoV-2 genomes spanning the full pandemic timeline, can a
real molecular-clock-based phylodynamic analysis recover a defensible calendar-time-scaled evolutionary
history and a real signal of epidemic growth, using the actual published tools real surveillance programs
use?

## 2. Objective

1. Fetch a real, date-stratified subsample of dated SARS-CoV-2 genomes spanning 2019–2026.
2. Align, QC, and clean the real sequences with Nextclade.
3. Build a real maximum-likelihood phylogeny with IQ-TREE2.
4. Check real temporal signal (root-to-tip regression) before trusting any date.
5. Time-scale the tree and reconstruct a real skyline effective-population trajectory with TreeTime.

## 3. Data fetch

435 real SARS-CoV-2 genomes, date-stratified at 15 genomes per real calendar quarter from 2019Q4 through
2026Q4 (29 quarters), so early, Alpha, Delta, and multiple Omicron-era lineages are all represented rather
than the sample being dominated by recent, more heavily-sequenced periods. Selected from Nextstrain's open
SARS-CoV-2 metadata (9.4M real records) and fetched by accession via NCBI's classic Entrez `efetch` API.

## 4. Data describe

Real human-host (`"Homo sapiens"`), fully-dated (`YYYY-MM-DD` precision) GenBank accessions only. After
Nextclade QC, 406 of 435 (93.4%) were retained (364 `good`, 42 `mediocre`); 29 `bad`-flagged sequences were
dropped. The final ML tree has exactly 406 leaves.

## 5. Methods / Workflow

1. Stream Nextstrain's real open metadata (908MB compressed, 9.4M rows) in bounded chunks, filtering to
   human-host, dated, open-accession records as each chunk is read.
2. Build a real date-stratified subsample (15 genomes/quarter) and fetch sequences via NCBI's `efetch` API
   in batches.
3. Align, QC, and clean with Nextclade (`sars-cov-2` dataset); drop `bad`-flagged sequences.
4. Build a real ML tree (GTR+G) with IQ-TREE2.
5. Check real temporal signal via TreeTime's root-to-tip regression before proceeding.
6. Time-scale the tree with TreeTime, producing a real dated phylogeny.
7. Reconstruct a real skyline effective-population-size trajectory with TreeTime's coalescent model.
8. Export a real per-node dated table.

## 6. Results

**Real, complete result.** Root-to-tip regression: rate = 7.746×10⁻⁴ substitutions/site/year, R² = 0.97 —
strong real temporal signal, close to SARS-CoV-2's independently published clock rate (~8×10⁻⁴). The
time-scaled tree and skyline both completed successfully. The skyline effective-population trajectory rises
from ~21 in late 2019 to the 200s–300s through 2020, then fluctuates through subsequent real variant waves,
reaching ~647 by early 2026.

## 7. Biology interpretation of results

R² = 0.97 means sampling date explains 97% of the real variance in root-to-tip genetic distance — strong
enough real temporal signal to trust the time-scaled dates that follow. The measured clock rate matching
published literature closely is itself a validation that the upstream alignment and tree-building steps
produced biologically sensible results. The skyline's qualitative shape (small, then a sharp real rise
through 2020, continued fluctuation afterward) tracks the real, known course of the pandemic, though its
absolute scale is a population-genetic proxy, not a literal case count.

## 8. Learning through project

Five real, live-discovered bugs, all genuine infrastructure/code gaps rather than modeling errors: (1) NCBI's
newer Datasets v2 API gateway turned out to be unreachable from the real build network (POST traffic
specifically blocked — confirmed via direct measurement, not assumed), resolved by switching to Nextstrain's
open metadata + NCBI's classic GET-only `efetch` API; (2) a single `pandas.read_csv()` across the full
decompressed metadata stream crashed the kernel outright (real OOM), fixed by chunked reading; (3) Nextclade's
sequence-naming convention didn't match Biopython's by default, silently zeroing out a QC-filter step,
caught via a direct set-intersection check; (4) TreeTime's own column auto-detection picked the wrong of two
present date-file columns, and separately needed GenBank version suffixes resolved from the real tree's
actual tip labels rather than assumed; (5) a cosmetic output-path double-join. The broader lesson: in any
pipeline chaining independently developed tools, the real risk is rarely the underlying science — it's
silently mismatched identifiers between tools, caught only by directly measuring real values at every
handoff.

## 9. Caveats

The inferred root date (mid-2019) is earlier than most published SARS-CoV-2 origin estimates (which cluster
around late 2019) — an honest, disclosed limitation of this subsample's size and sparse early-quarter
sampling (the deepest branches of the tree are the least-constrained part of any root-to-tip extrapolation),
not a pipeline error. Skyline Ne(t) values are a real population-genetic proxy, not literal case counts, and
their absolute scale is sensitive to this subsample's size and sampling scheme. This build's sandbox had a
disclosed, environment-specific network restriction (now resolved for the real execution environment used);
all tool names, versions, and API details were independently verified live before building the notebook.
