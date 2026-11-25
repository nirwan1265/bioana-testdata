# bioana-testdata

Small real data sets for testing [Bioana](https://github.com/nirwan1265/bioana) on real files (BAM, HapMap,
peak calls, Salmon output). Each comes from a published package or paper; cite the source when you use it.
Bioana's tests look here: set `BIOANA_TESTDATA=/path/to/bioana-testdata`, or clone this repo next to `bioana/`
(`tests/testthat/test-real-data.R`).

| folder | what | source and citation | what Bioana checks |
|---|---|---|---|
| `rnaseq_airway/` | RNA-seq BAM subsets of 8 samples (4 airway smooth-muscle cell lines x dexamethasone), the matching GTF subset (20 genes), Salmon output for 2 samples, sample table | Bioconductor package **airway** (LGPL), inst/extdata. Himes BE et al. (2014) RNA-Seq transcriptome profiling identifies CRISPLD2 as a glucocorticoid responsive gene. *PLoS One* 9:e99625. GEO GSE52778 | Rsubread featureCounts on the BAMs gives the package's own gene counts (median ratio 1, r = 0.998) |
| `chip_diffbind_chr18_20mb/` | ER ChIP-seq BAMs of 11 libraries + 5 inputs (MCF7, T47D, ZR75, BT474, tamoxifen-resistant MCF7), cut to chr18:1-20,000,000 from the DiffBind vignette data (chr18); MACS peak files; sample sheet (`tamoxifen.csv`, CR line endings); `dba_norm_comp.RData` = DiffBind's own consensus sites (1,076 on chr18) and DESeq2 DB calls under 6 normalisations | DiffBind vignette data, https://content.cruk.cam.ac.uk/bioinformatics/software/DiffBind/DiffBind_vignette_data.tar.gz. Ross-Innes CS et al. (2012) Differential oestrogen receptor binding is associated with clinical outcome in breast cancer. *Nature* 481:389. Stark R, Brown G, DiffBind (Bioconductor) | reads counted in DiffBind's sites (in the region: 314); Bioana's DB calls are DiffBind's calls (precision > 0.9, same direction at every shared site) |
| `merip_exomepeak2/` | MeRIP-seq IP and input BAMs (4 IP + 3 input control, 1 IP + 1 input treated) and `example.gtf` | Bioconductor package **exomePeak2** (Artistic-2.0), inst/extdata | exon windows from the GTF, IP paired with input, the edgeR IP x treatment model runs; IP enrichment visible. Too small for a differential test (1 treated sample) |
| `maize_tassel/` | 3,093 SNPs on 281 maize inbred lines (HapMap), phenotypes of 279 lines in 2 locations (`mdp_phenotype.txt`: EarHT, dpoll, EarDia, Q1-Q3), line values (`mdp_traits.txt`), kinship, population structure | TASSEL tutorial data set (TASSEL 5, Buckler lab; https://tassel.bitbucket.io/docs/TASSELTutorialData5.zip), the maize 282 association panel (Flint-Garcia SA et al. 2005, *Plant J* 44:1054); same files as GAPIT's demo data | HapMap -> VCF; line means over the 2 locations (r = 0.97 with TASSEL's values); Bioana's GWAS equals rrBLUP called directly (-log10 p to 1e-11) |
| `rrBLUP_4.6.3.tar.gz` | rrBLUP source package (CRAN) | Endelman JB (2011) Ridge regression and other kernels for genomic selection with R package rrBLUP. *Plant Genome* 4:250 | installed in the build environment, where CRAN is blocked |

Also used, but shipped with installed packages rather than here: DSS's example counts
(`system.file("extdata", "cond1_1.txt", package = "DSS")`, 2 vs 2; Park & Wu 2016, *Bioinformatics* 32:1446) and bsseq's
real Bismark coverage and cytosine report (`system.file("extdata", package = "bsseq")`).

Not kept: the full DiffBind chr18 BAMs (536 MB; the 20 Mb cut keeps every read in the sites it covers) and
RNAseqData.HNRNPC.bam.chr14 (420 MB; airway covers RNA-seq from BAMs).
