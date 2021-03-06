# Resource

## Overview
This repository contains all scripts and preliminary statistics related to the key analyses in each section in Result of manuscript (MS) ***"Large-scale analysis of 2,152 Ig-seq datasets reveals key features of B cell biology and the antibody repertoire"***. These analyses can be classified into four categories, including gene usage, somatic recombination, somatic hypermutation and public clones. More details can be found in the following sections.

## Gene usage
#### Gene usage overview quantification (Fig. 2A)

`python geneUsageOverview_quantification.py V_input.txt V outDir`

The script, `geneUsageOverview_quantification.py`, implements the overview of V/D/J gene usage. It takes three parameters including tabular usage matrix file, gene type, and output folder. Each column for usage matrix file indicates the sample and each row stands for the gene segment. The usage matrix file looks like below,
```
	SRR8365312	ERR2567197	ERR1812302
IGHV3-23	11.36	7.65	12.65
IGHV3-13	0.35	0.63	0.31
IGHV4-34	2.34	3.85	2.48
IGHV3-33	3.57	4.34	3.98
IGHV1-18	4.87	3.38	3.28
```
It outputs four tabular files and columns for each file are shown below. 

`V_gene_sample_num.txt`
- col 1: the name of gene segment
- col 2: the number of samples having this gene segment

`V_gene_reads_num.txt`
- col 1: the SRR id for sample
- col 2: the number of productive for this sample

`V_gene_num_per_sample.txt`
- col 1: the SRR id for sample
- col 2: the number of gene segment this sample having

`V_gene_usage.txt` (Samples in this file were sorted by the number of productive reads and genes were sorted by the number of samples having it.)
- each column: the SRR id for sample
- each row: the name of gene segment

#### Gene usage overview visualization (Fig. 2A)

`python geneUsageOverview_visualization.py V_gene_usage.txt V_gene_sample_num.txt V_gene_num_per_sample.txt outDir`

The script, `geneUsageOverview_visualization.py`, implements the visualization of overall gene usage. It takes four parameters including three files generated by the script above and one output folder. The output figure looks like, 

![geneusageoverview](figures/V_gene_usage_heatmap_bar.png)

#### Core gene selection (Fig. 2B)

`python obtainCoreGene.py V_accu_fraction.txt outDir`

The script, `obtainCoreGene.py`, implements the core V gene selection and visualization. It takes two parameters which include tabular V gene accumulated usage file and output folder. The tabular file which looks like below has three columns named `Gene_order`, `Run`, and `Clone_fraction`. This script has two outputs, one figure and one file recording the index to stop enrolling genes and its’ slope. The figure looks like below, 

![coregene](figures/V_accu_fraction_median_slope.png)


## Somatic recombination


## Somatic hypermutation
### Scripts
#### Motif mutation profile (Fig. 6A, PWM)

`python motif_mut_freq_cal_for_single_sample.py sample alignments.txt clones.txt`

The script, `motif_mut_freq_cal_for_single_sample.py`, implements motif mutation profile quantification for each sample. It takes three parameters as input, which include the sample id (`sample`) and the path to `alignments.txt` and `clones.txt` output by MiXCR for each sample. Noted that a column `cloneId` is required in `alignments.txt` for determining the relationship between reads and clones. This script outputs for each sample a file named `*_motif_stat.txt` that records the germline count for each motif and the count for each mutunt nucleotide.

`python motif_mut_freq_merge_for_multiple_samples.py pathfile`

The script, `motif_mut_freq_merge_for_multiple_samples.py`, implements the merge of individual statistical files output by `motif_mut_freq_cal_for_single_sample.py`. It takes only a file that records the paths to all individual file as input and output a merged statistical file named `motif_mut_profile_merged.txt`, which looks like

```
Motif	Percentage	Mutated_num	Germline_num	A	C	G	T	A_mut	C_mut	G_mut	T_mut
WRCY	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
RGYW	20.1	55307.52	275123.0	54.05	33.76	0.0	12.19	29892.46	18671.34	0.0	6743.72
SYC	3.0	46490.89	1548139.0	23.76	0.0	37.57	38.66	11048.41	0.0	17468.41	17974.08
GRS	4.02	47448.62	1178900.0	42.32	39.0	0.0	18.69	20078.4	18503.5	0.0	8866.72
C	5.57	140912.61	2527879.0	22.37	0.0	28.84	48.79	31526.0	0.0	40641.47	68745.14
G	10.21	261286.94	2559167.0	45.06	31.78	0.0	23.16	117741.83	83040.56	0.0	60504.55
WA	24.94	38831.51	155687.0	0.0	21.86	48.09	30.04	0.0	8490.01	18675.62	11665.88
TW	15.28	81991.92	536617.0	25.14	53.32	21.53	0.0	20614.11	43721.07	17656.75	0.0
A	8.08	147746.78	1828092.0	0.0	34.17	44.32	21.51	0.0	50478.65	65481.58	31786.55
T	6.54	98286.75	1502607.0	17.56	38.15	44.28	0.0	17261.95	37498.82	43525.98	0.0
```


## Public clone
### Preliminary statistics
* [PUB_all_to_all_repertoire_comparison_matrix.csv.gz](data/PUB_all_to_all_repertoire_comparison_matrix.csv.gz) 
	- The 2152*2152 matrix recording all-to-all repertoire comparisons (Fig. 5A in MS)
* [PUB_pairwise_repertoire_comparison.tab.gz](data/PUB_pairwise_repertoire_comparison.tab.gz) 
	- The table recording pairwise repertoire comparison together with the number of total clones for compared samples (Fig. 5B) 
* [PUB_public_clone_record_from_mixcr_output.tab.gz](data/PUB_public_clone_record_from_mixcr_output.tab.gz) 
	- The file containing all lines in the clones.txt file that records public clones (Fig 5C)
### Scripts
#### All-to-all public clone quantification (Fig. 5A)

`python all2all_public_quantification.py clone_file_path.tab`

The script, `all2all_public_quantification.py`, implements an all-to-all public clone quantification among enrolled samples. It takes only an annotation tabular file, `clone_file_path.tab`, as input. This file has three columns including sample id, project id and path to `clones.txt` that output by MiXCR and it looks like below,
```
Sample	Project	Path
DRR056252	PRJDB4353	DRR056252_MIXCR/clones.txt
DRR056253	PRJDB4353	DRR056253_MIXCR/clones.txt
DRR056254	PRJDB4353	DRR056254_MIXCR/clones.txt
ERR1760498	PRJEB15295	ERR1760498_MIXCR/clones.txt
ERR1812282	PRJEB18926	ERR1812282_MIXCR/clones.txt
ERR1812283	PRJEB18926	ERR1812283_MIXCR/clones.txt
```
It outputs three statistic files, including two all-to-all repertoire comparison matrixes (i. e. `PUB_all_to_all_repertoire_comparison_matrix.csv.gz`) and a tabular file with three columns (clone, shared samples, number of shared samples) which is as below,
```
Clone	Samples	No.of.Samples
IGHV3-7_IGHJ4_TGTGTGAGAAGTCTAGGGATCCACTGG	SRR5063107,SRR5063100	2
IGHV3-64D_IGHJ4_TGTGTGAAAGCTCCTGGTGGCTGGTCCAACCCCTTTGACTACTGG	SRR8365263,SRR8365473	2
IGHV1-69_IGHJ4_TGTGCGAGAGATCGGGAGAACTGGAACTACGTATTTGACTACTGG	SRR8365263,SRR8365261,SRR8365473,SRR8365471	4
IGHV3-30-3_IGHJ4_TGTGCGAGAGGAGATTCCCGATACAGCTATGGCCTAGACCTTGACTACTGG	SRR8365263,SRR8365261	2
```
The two matrixes are different from each other in that one of them neglects clone sharing between samples from the same project while the other does not (see _Method_ in MS above). 

#### All-to-all public clone visualization (Fig. 5A)

`python all2all_public_visualization.py matrix.tab metadata.tab`

The script, `all2all_public_visualization.py`, implements the visualization of all-to-all public clone quantification result. It takes two parameters, the first is the all-to-all repertoire comparison matrix (i. e. `PUB_all_to_all_repertoire_comparison_matrix.csv.gz`) and the second is a tabular metadata for each enrolled sample (i. e. `metadata.tab`). The samples are ordered according to the number of total clones they contain. The output figure looks like

![heatmap](figures/all_to_all_comparison_heatmap.png)

#### Linear model visualization (Fig. 5B)

`python linear_model_visualization.py pairwise_rep_comp.tab`

The script, `linear_model_visualization.py`, implements the visualization of the linear model we observed between repertoires. It takes only the pairwise repertoire comparison tabular file (with the number of total clones for compared samples included, i. e. `PUB_pairwise_repertoire_comparison.tab.gz`) and gives two kinds of output with one being figures demonstrating the linear models and the other being a tabular file recording the parameters of linear models. The output figures show linear models together with individual points and each of them looks like

![linear_model](figures/linear_model_scatter_plot_for_samples_more_than_10000_clones.png)

This tabular file, `linear_model_params.txt`, consists of four columns, including `CloneNumFilter` (clone number filter criteria), `Coef` (coefficient of the linear model), `Intercept` (intercept of the linear model) and `R2` (goodness of fit), which looks like
```
CloneNumFilter  Coef    Intercept       R2
10000   8.235715861523267e-11   3.018497992946571       0.8657346352020397
100000  8.06189835673996e-11    21.389248342230502      0.87066669432678
1000000 6.195753424951246e-11   382.57448702253726      0.8547479623611061
2000000 5.83398705048679e-11    494.30328497267794      0.8604386804475349
3000000 5.086226482846224e-11   907.385414449705        0.9609951642782198
4000000 4.9542897429104056e-11  1076.0557291797643      0.982150157073821
5000000 6.876732151569549e-11   -824.2036315063278      0.9974067292062271
```


#### Quantification and visualization of the correlation between clonality and publicness of public clones (Fig. 5C)

`python clonality_publicness_correlation_visualization.py pub_record.tab metadata.tab`

The script, `clonality_publicness_correlation_visualization.py`, implements the visualization of correlation between clonality and publicness. It takes two parameters, the first is the file containing all lines in the clones.txt file that records public clones (i. e. `PUB_public_clone_record_from_mixcr_output.tab.gz`) and the second is a tabular metadata for each enrolled sample (i. e. `metadata.tab`). The output figure looks like

![correlation](figures/Clonality_against_publicness_and_CDR3_length.png)

## Dependencies
In-house scripts above were written in Python (v3.7) and MATLAB (v). For python, a series of modules are required, which include pandas, csv. For MATLAB, xxx and xxx are required. 

## Others
For the scripts implementing other analyses in the MS, please contact the Lead Contact, Zhenhai Zhang (zhenhaismu@163.com).