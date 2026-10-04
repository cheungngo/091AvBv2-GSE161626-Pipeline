# **Reanalysis of persistent H3K27ac regions after a single DOI dose nominates synaptic and cAMP-linked genes in mouse frontal cortex**

## **Abstract**

The public dataset GSE161626 profiles H3K27ac and RNA from NeuN-positive frontal-cortex nuclei after a single intraperitoneal dose of DOI, at 24 hours, 48 hours, and 7 days. The original study reported that some enhancer-associated H3K27ac changes persisted longer than most transcript changes; the present reanalysis asks which protein-coding genes are near regions classified here as persistently altered. It does not establish enhancer-to-gene regulation or a lasting functional state. The pipeline constructed high-signal H3K27ac regions, tested 7-day versus vehicle differences, clustered four-time-point trajectories, applied a power-law activity-by-contact linker, and tested curated gene lists for over-representation. The curated UP-associated list was enriched for synapse organization, neuron-projection morphogenesis, axon guidance, neuronal-system, and cAMP-signaling annotations. Candidate genes recurring across the supplied results include Nlgn1, Nrxn1, Slitrk-family genes, Dlg4, Shank2, Grin2a, Grin2b, Gria3, Camk2a/b, Cacna2d1, Adcy1, and Pde4b. These are predicted regulatory-neighborhood associations, not validated enhancer targets. “UP” denotes the assigned direction of linked H3K27ac regions, not gene-expression direction. The stored ABC scores exceeded 1 despite the nominal formula’s normalization; Stage 3 recomputed scores from the already filtered pair table, so its values are not canonical ABC fractions. Although the source experiment included RNA-seq, this reanalysis did not complete a day-7 RNA concordance test. The findings nominate a candidate association consistent with lingering regulation near plasticity-related genes. A multiday metaplastic state remains a hypothesis, not a result.

## **Introduction**

### **The clinical interval and the molecular question**

A central question in psychedelic research is how effects that outlast the acute drug experience might be related to biological changes occurring afterward. The acute subjective and pharmacological effects of serotonergic psychedelics are usually discussed on a scale of hours, while clinical and preclinical research has also examined effects that persist longer. This difference in timescale motivates a search for molecular events that might persist beyond drug exposure. It does not, by itself, establish that a biological plasticity window exists or that such a window explains lasting clinical outcomes \[1\].

Structural plasticity offers one possible context for this question. In cellular and animal models, some psychedelics have been associated with changes in neurite growth, dendritic spines, synapse number, or synaptic function \[2\]. Those findings make neuronal plasticity a reasonable topic for follow-up, but they do not show that the same processes explain the persistence of clinical effects. Nor do they establish that a particular molecular marker can identify when an individual brain is more responsive to later experience. The analysis reported here is narrower: it re-links a published mouse H3K27ac time course to nominate nearby protein-coding genes, then asks whether those genes are enriched for neuronal and signaling annotations.

### **The source experiment**

The source study administered DOI once and profiled frontal-cortex neuronal nuclei after vehicle or DOI exposure at 24 hours, 48 hours, and 7 days. It used NeuN-positive nuclei, MOWChIP-seq for H3K27ac, and Smart-seq2 for RNA. The chromatin and RNA measurements were made on a population of sorted nuclei; this was not a single-cell atlas with cell-by-cell molecular profiles. The source study reported persistent enhancer-associated H3K27ac changes alongside a more transient transcriptomic response. It also reported DOI-associated changes in dendritic spines and fear-extinction behavior, with 5-HT2A receptor dependence for those tested outcomes \[3\].

Those source-study findings provide context, not independent replication. The present pipeline does not repeat the receptor knockout or antagonist tests, and it does not reanalyze the spine, electrophysiology, or behavioral measurements. Receptor specificity for the molecular profile therefore comes from the source study’s experimental context; it is not newly demonstrated by the re-linking analysis. Earlier work distinguishing transcriptional responses to hallucinogenic and nonhallucinogenic 5-HT2A agonists likewise provides pharmacological background, not a new receptor-specific result in this reanalysis \[4,5\].

### **Why re-link regions to genes?**

A genomic region’s nearest gene is not necessarily its regulatory target. Activity-by-contact models were developed to improve enhancer–gene nomination by combining an activity measure for a regulatory element with an estimate of its contact with a gene’s promoter. In the canonical ABC model, an element’s activity-by-contact score is normalized against the other candidate elements for that gene, and the model was evaluated against CRISPR perturbation data \[6\]. A predicted score remains a prediction: enrichment analysis cannot establish that a particular region controls a particular gene.

The aim here was therefore to nominate protein-coding genes near H3K27ac regions whose signal followed a sustained trajectory, with an a priori interest in synaptic organization and cyclic-nucleotide signaling. The linker used in this pipeline is ABC-like, not a validated implementation of the canonical model. It substitutes a distance-based contact function for measured contact data, and the regions submitted to it were generated by a high-signal binning procedure rather than a conventional enhancer-calling and promoter-exclusion workflow. These choices limit what can be concluded from any individual enhancer–gene pair.

### **Scope and claims**

This study makes no claim of cell-subtype resolution beyond the use of NeuN-positive nuclei. It does not show that a nominated region regulates its assigned gene, that the gene’s RNA changed at day 7, or that a circuit’s function changed. The source study’s structural and behavioral findings are relevant background, but they are not outcomes of this reanalysis.

The phrase metaplastic state is used, if at all, only as a discussion hypothesis. Metaplasticity refers to a change in the rules or capacity of later plasticity, not simply to a persistent chromatin mark near genes annotated to synapses. The data presented here do not measure whether the threshold for later potentiation or depression changed. The Results first report the curated gene-list enrichments, then present the separate Stage 3 analysis as an audit of gene-level direction and linker behavior.

## **Methods**

### **Dataset and sample handling**

The analysis used bigWig files from GSE161626, a mouse DOI time-course dataset. The source experiment profiled six brain samples per treatment condition and generated two technical replicates for each brain sample for both the chromatin and RNA library workflows \[3\]. The source methods describe NeuN-positive nuclei isolated from frontal cortex by fluorescence-activated cell sorting, followed by MOWChIP-seq and Smart-seq2 \[7-10\].

In the reanalysis code, bigWig filenames were parsed to assign each file to vehicle, 24-hour, 48-hour, or 7-day groups, as well as mouse and replicate identifiers. The signal matrix retained individual bigWig files as separate sample columns. The statistical test then compared columns assigned to 7 days with columns assigned to vehicle; it did not average technical replicates within mouse or include a mouse-level random effect. Because the source experiment generated technical replicates for each brain sample, treating those library columns as independent observations risks pseudoreplication and inflated nominal degrees of freedom. The biological sample size was six mice per condition; the effective independent sample size should not be taken as the number of technical-library columns. A mouse-level aggregation or a model that accounts for mouse identity is needed as a sensitivity analysis.

### **High-signal region definition**

The reanalysis did not use MACS2 peaks or the source paper’s final enhancer set. Instead, each bigWig was divided into non-overlapping 1-kilobase bins across autosomes and chromosome X, and mean signal was calculated for each bin. The top 2% of bins on each chromosome in each selected sample were retained. The selected intervals were pooled across samples, merged with 500-base-pair slack, and regions shorter than 500 base pairs were discarded.

These regions are referred to here as high-signal H3K27ac regions. They are not all necessarily enhancers: the code did not explicitly remove promoters or distinguish enhancers from other H3K27ac-enriched regions. That difference matters because the source study identified enhancer regions through peak calling and excluded regions overlapping promoters, whereas the reanalysis’s percentile-based region construction did neither \[3\].

### **Quantification and 7-day differential testing**

For each region and bigWig, the mean signal across the interval was calculated. The matrix was transformed as log2(signal \+ 1), then quantile-transformed column by column to approximate a normal distribution. The quantity called lfc\_7d in the code is the difference between mean quantile-transformed signal in the 7-day columns and the mean in the vehicle columns. It is not a log2 fold change in the original H3K27ac signal.

Each region was tested with an independent two-sample t-test comparing 7-day and vehicle columns. The reported code uses the default equal-variance form of the test and does not include batch, sex, mouse identity, or other covariates. P values were adjusted by the Benjamini–Hochberg procedure. Regions were retained for the time-course clustering step if adjusted P was below .05 and the absolute difference in quantile-transformed signal exceeded 0.25. Consequently, the statistical evidence should be interpreted in light of the technical-replicate handling and the transformed effect measure.

### **Trajectory clustering and persistence rule**

For regions passing the 7-day filter, mean transformed signal was calculated for vehicle, 24 hours, 48 hours, and 7 days. Each region’s four-point trajectory was standardized across time, and the standardized profiles were assigned to six clusters using k-means. A cluster was labeled persistently UP if its mean profile was above vehicle at 24 hours, 48 hours, and 7 days, and if its 7-day value exceeded 0.3. A cluster was labeled persistently DOWN if it was below vehicle at all three post-dose time points and its 7-day value was below −0.3.

Persistence was therefore assigned at the cluster level. Every region in a qualifying cluster inherited that cluster’s direction, even if an individual region did not itself meet each trajectory criterion. Conversely, an individual region could be excluded if its cluster average failed the rule. The number of persistent regions cannot be reported from the consolidated summaries supplied for this manuscript; it should be retrieved from the original run log. These cluster labels are a re-description generated by this pipeline, not an independent demonstration of persistence beyond the source experiment.

### **Candidate gene assignment and score audit**

Protein-coding transcription start sites were taken from GENCODE mouse release M25. The analysis considered regions within 5 megabases of each gene’s transcription start site. Region activity was defined as mean 7-day signal, not the DOI-versus-vehicle difference. Contact was estimated as a power-law function of distance, with exponent 1.024 and scale 5.9, a minimum distance of 1 kilobase, and an additive pseudocount of 1\. For each gene, activity multiplied by contact was normalized across candidate regions in the 5-megabase window, and pairs with a score above 0.02 were saved.

This formula differs from canonical ABC in several ways. It uses a power-law proxy rather than a measured contact map, omits promoter activity, uses regions that were not separated into enhancers and promoters, and applies a large 5-megabase candidate window. The additive constant also weakens the distance gradient: across much of the window, the power-law term is small relative to the pseudocount.

There is also a material discrepancy between the nominal code and the reported prediction table. The code shown normalizes each region’s activity-by-contact value against all candidate regions for its gene before filtering, which should produce values no greater than 1\. The supplied Stage 3 summary reports stored values up to 39.176. The available files do not identify the cause of that mismatch. Stage 3 therefore reconstructed scores from the AxC values in the saved, already filtered pair table. Because the denominator includes only retained rows, the resulting numbers are not canonical ABC fractions normalized over the full candidate set. The reported correlation between repaired score and distance was −0.021. Accordingly, neither the stored nor repaired score rank is treated here as evidence that a specific region regulates a specific gene \[6\].

### **Gene-set construction and Stage 3 analysis**

The primary downstream analysis used a curated list of 4,362 mouse protein-coding symbols from final\_gene\_list\_final.txt, with symbols ending in “Rik” removed. The earlier downstream script assigned each gene the direction of its highest-scoring enhancer–gene pair, then split the curated list into 2,895 UP and 1,467 DOWN genes. This is the primary gene-set analysis reported below. It does not summarize the direction of all linked regions for each gene; a gene with several linked regions is represented by the direction of its best-scoring pair in that split.

Stage 3 was a separate gene-level reduction of the prediction table, not a nested sensitivity subset of the 4,362-gene primary list. It began with 471,819 enhancer–gene pairs and 21,516 raw target genes. It flagged 3,159 symbols as artifacts under a pattern-based filter covering several gene families and symbol classes, including Rhox, Gm, Olfr, Vmn, Mir, Snord, Snhg, and Rik. For each remaining gene, it calculated the fraction of linked regions in the majority direction and called the gene UP or DOWN only if direction purity exceeded 0.70. The high-confidence set additionally required a minimum enhancer FDR below 0.05, a maximum absolute normalized signal difference of at least 0.25, at least two linked regions, and a best repaired score of at least 0.02 when a repaired score was available.

The Stage 3 procedure produced a 5,112-gene set, comprising 3,794 UP and 1,318 DOWN genes. The result is useful as a distinct quality-control analysis, but it should not be described as a direct replication of the primary 4,362-gene result: the set construction and gene-level direction rules differ. Its leading-edge analysis re-used terms and member genes from the prior all-target Enrichr output, retaining members also found in the Stage 3 UP set. That overlap is not a second enrichment test.

### **Over-representation, ranked enrichment, and RNA analysis**

Over-representation analysis used Enrichr with mouse libraries and an adjusted P threshold of .05 \[11\]. The primary analysis tested the all-target, UP, and DOWN curated sets. Stage 3 separately tested its high-confidence UP and DOWN sets. GSEA was exploratory, with an FDR threshold of .25. The original downstream script ranked genes by best-pair ABC score; that ranking is not used as evidence for pathway direction. Stage 3 instead ranked genes by enhancer-weighted normalized H3K27ac difference. Under that ranking, positive enrichment indicates concentration toward the enhancer-gain end and negative enrichment toward the enhancer-loss end; neither result establishes gene-expression direction or pathway activation or suppression \[12\].

The source experiment included RNA-seq, but no usable joined day-7 RNA means table was produced in this pipeline run \[3\]. The planned comparison—whether target genes had greater absolute RNA change than background and whether enhancer direction agreed with RNA sign—was therefore not performed. Its absence is a limitation, not a negative RNA result.

## **Results**

### **Persistent regions and the scope of the reanalysis**

The analysis reprocessed the time course to identify significant 7-day regions and assign trajectory-based directions. The consolidated summaries supplied here do not report the number of consensus regions, the number passing the day-7 differential filter, or the number assigned to persistent UP and DOWN clusters. Those counts are not inferred from the source study’s peak counts because the region definitions and filtering procedures differ. They should be recovered from the run log before a final submission.

The most defensible biological results therefore come from the reported gene-set analyses, interpreted as associations with regions classified by this pipeline. The source study already reported that a subset of H3K27ac changes persisted through 7 days, so the present reanalysis does not establish that persistence anew. Its added question is which genes appear in the curated target list and whether those lists are enriched for particular annotations.

### **Primary enrichment results**

The curated all-target set showed enrichment for synapse organization (adjusted P \= 3.02 × 10⁻⁴), cell-cell adhesion via plasma-membrane adhesion molecules (adjusted P \= 3.24 × 10⁻³), and nervous-system development (adjusted P \= 4.98 × 10⁻³). Other significant terms included positive regulation of cell-projection organization and neuron-projection morphogenesis, each with adjusted P \= 1.95 × 10⁻², and positive regulation of neuron-projection development (adjusted P \= 3.64 × 10⁻²). These are overlapping ontology descriptions, not independent discoveries. Their shared membership supports a broad annotation theme around neuronal structure and synaptic organization rather than a set of separate, experimentally verified processes.

The direction-split primary analysis placed the clearest reported signal in the UP-associated list. In GO Biological Process, the top terms included neuron-projection morphogenesis (adjusted P \= 1.60 × 10⁻³), synapse organization (adjusted P \= 4.58 × 10⁻³), nervous-system development (adjusted P \= 8.15 × 10⁻³), and axonogenesis (adjusted P \= 2.45 × 10⁻²). Positive regulation of filopodium assembly was also enriched (adjusted P \= 2.53 × 10⁻²). In KEGG, axon guidance was enriched at adjusted P \= 3.15 × 10⁻⁴ and cAMP signaling at adjusted P \= 2.15 × 10⁻³. Rap1 and cGMP–PKG signaling were also significant, but these gene sets overlap the same broad cyclic-nucleotide and signaling gene pool and should not be presented as three independent mechanistic findings. Reactome neuronal-system and axon-guidance terms were significant at adjusted P \= 2.55 × 10⁻².

The primary DOWN list had no significant terms in the reported GO Biological Process, KEGG, Reactome, or MGI phenotype libraries. This asymmetry is part of the result: the curated synaptic and neuronal enrichment was concentrated in the UP-associated set under the best-pair direction rule, while the equivalent reported DOWN over-representation was null. It does not mean that DOI globally increased synaptic function. The “UP” direction refers to the associated region’s H3K27ac signal, not to expression of the linked gene.

Several significant MGI terms require different wording from pathway annotations. “Reduced long-term potentiation” and “abnormal spatial working memory” are phenotype annotations compiled from genetic models. They do not indicate that the treated mice in this reanalysis showed impaired or altered LTP or working memory. The spatial-working-memory term had adjusted P \= 3.79 × 10⁻² in the primary UP set. Such annotations can help identify genes previously connected to relevant phenotypes, but they do not report the outcome of a DOI behavioral experiment.

Selected reported primary ORA findings are summarized in Table 1\.

**Table 1\. Selected primary over-representation analysis results**

| Analysis set | Library | Enriched term | Adjusted P |
| ----- | ----- | ----- | ----- |
| All targets | GO Biological Process | Synapse organization | 3.02 × 10⁻⁴ |
| All targets | GO Biological Process | Cell-cell adhesion via plasma-membrane adhesion molecules | 3.24 × 10⁻³ |
| All targets | GO Biological Process | Nervous-system development | 4.98 × 10⁻³ |
| All targets | GO Biological Process | Positive regulation of cell-projection organization | 1.95 × 10⁻² |
| All targets | GO Biological Process | Neuron-projection morphogenesis | 1.95 × 10⁻² |
| All targets | GO Biological Process | Positive regulation of neuron-projection development | 3.64 × 10⁻² |
| UP | GO Biological Process | Neuron-projection morphogenesis | 1.60 × 10⁻³ |
| UP | GO Biological Process | Synapse organization | 4.58 × 10⁻³ |
| UP | GO Biological Process | Nervous-system development | 8.15 × 10⁻³ |
| UP | GO Biological Process | Axonogenesis | 2.45 × 10⁻² |
| UP | GO Biological Process | Positive regulation of filopodium assembly | 2.53 × 10⁻² |
| UP | KEGG | Axon guidance | 3.15 × 10⁻⁴ |
| UP | KEGG | cAMP signaling pathway | 2.15 × 10⁻³ |
| UP | KEGG | Rap1 signaling pathway | 1.09 × 10⁻² |
| UP | KEGG | cGMP-PKG signaling pathway | 1.09 × 10⁻² |
| UP | Reactome | Neuronal System | 2.55 × 10⁻² |
| UP | Reactome | Axon Guidance | 2.55 × 10⁻² |
| DOWN | GO Biological Process, KEGG, Reactome, MGI phenotype | No significant terms reported | — |

*Abbreviations: GO, Gene Ontology; MGI, Mouse Genome Informatics.*

### **Candidate synaptic modules**

The gene annotations can be organized into candidate components of synaptic structure and signaling. This organization is a way to describe the gene list, not evidence that the predicted regions regulate each named gene.

The first group involves adhesion and synaptic alignment. The curated output includes Nlgn1 and Nrxn1, along with Slitrk-family members, Shank2, Dlg4, Il1rapl1, and Lrrtm3. Neuroligins and neurexins are established trans-synaptic adhesion and signaling proteins, and neuroligin expression can induce presynaptic differentiation in experimental systems \[13,14,15\]. Postsynaptic scaffold proteins such as SHANK and PSD-95/DLG4 contribute to organizing receptor and signaling complexes at synapses \[16\]. The Slitrk family is of particular note because several family members recur in the supplied lists. Experimental work has found that Slitrks can promote synapse formation through interactions with LAR-family receptor protein tyrosine phosphatases, with effects varying by isoform and synapse type \[17\]. In this analysis, multiple Slitrk genes should be treated as a family-level enrichment signal, not as five or six independent mechanistic findings.

Cdh10 and Cdh12 also rank among high enhancer-weighted candidates in Stage 3\. However, the supplied diagnostics and gene assignments indicate that neighboring genes can share regions within the broad linking window. These cadherins are therefore weaker locus-specific claims than a simple gene ranking might suggest. Their presence supports a neighborhood-level association with adhesion-related annotations; it does not establish that the scored regions selectively regulate either cadherin.

A second group concerns glutamatergic transmission and its regulation. The reported lists include Grin2a and Grin2b, encoding GluN2A and GluN2B NMDA-receptor subunits, as well as Gria2 and Gria3, which encode AMPA-receptor subunits. Cacna2d1 and Cacnb4 are calcium-channel-associated genes; Camk2a and Camk2b encode calcium/calmodulin-dependent kinases; and Akap5 encodes a scaffold that organizes signaling proteins near synaptic receptors. These genes make the annotations relevant to receptor composition, calcium signaling, and receptor-associated signaling complexes. Their functions and receptor subunit biology are well established, but the present analysis measures none of the corresponding proteins, currents, trafficking, or phosphorylation states \[18,19\].

The distinction matters for direction. In other mouse prefrontal-cortex preparations, sustained 5-HT2A activation by DOI has been reported to depress AMPA-mediated synaptic currents and promote AMPA-receptor internalization, while other serotonergic preparations report increased NMDA transmission or plasticity under different conditions \[20,21\]. The source DOI study also reported greater LTP at 24 hours, not at day 7 \[3\]. These differing observations reinforce that a region’s H3K27ac gain near a glutamate-receptor gene does not mean more excitation, stronger transmission, or an increase in any specific receptor’s function. Synaptic plasticity includes potentiation and depression, with direction depending on context and protocol \[22,23\].

A third group links calcium-responsive signaling with cyclic-nucleotide regulation. Adcy1 appeared in the primary KEGG cAMP signaling term, together with Pde4b, Prkaca, Creb1, and Rps6ka3. ADCY1 is a calcium/calmodulin-stimulated adenylyl cyclase; genetic and electrophysiological work has connected calcium-stimulated adenylyl cyclase activity to specific forms of LTP and long-term memory. PDE enzymes degrade cyclic nucleotides and can shape the location and duration of cAMP signals \[24-26\]. CREB and related transcription factors are established activity-responsive regulators, but their presence in a gene set does not show that they were activated in these samples \[27,28\].

This cAMP annotation is most specific in the primary curated analysis. It did not reappear as a top over-representation term in the Stage 3 cleaned-UP set, which instead returned taste-transduction and P2Y-receptor terms. The Stage 3 result does not erase the primary cAMP result, but it does mean the two should not be merged into a single “replicated” pathway finding. Rap1 and cGMP–PKG overlap with portions of the same cyclic-nucleotide-associated gene pool. Glp1r is a Gs-coupled receptor in the broader cAMP annotation; its presence does not support a DOI-induced GLP-1 mechanism.

Candidate genes discussed in these modules are grouped in Table 2\.

**Table 2\. Candidate genes recurring in the supplied results and discussed synaptic modules**

| Candidate module | Genes |
| ----- | ----- |
| Adhesion and synaptic scaffold | Nlgn1, Nrxn1, Slitrk1, Slitrk2, Slitrk3, Slitrk4, Slitrk5, Slitrk6, Shank2, Dlg4, Il1rapl1, Lrrtm3, Cdh10, Cdh12 |
| Glutamatergic transmission and calcium signaling | Grin2a, Grin2b, Gria2, Gria3, Cacna2d1, Cacnb4, Camk2a, Camk2b, Akap5 |
| cAMP-linked signaling | Adcy1, Pde4b, Prkaca, Creb1, Rps6ka3 |

*These are candidate genes appearing in the supplied curated or Stage 3 results; the table does not imply validated enhancer-to-gene regulation.*

### **Stage 3 as a quality-control analysis**

Stage 3 exposed substantial ambiguity in the raw gene assignments. The input consisted of 471,819 region–gene pairs and 21,516 raw target genes. After the artifact-symbol flags were applied, the median direction purity among non-artifact genes was 0.632, and 28.9% reached the 0.70 purity threshold. A further 21.6% were near 0.50 purity. The median gene had 22 retained region links. These values describe a broad, highly connected linking architecture in which many genes receive mixed UP and DOWN assignments.

The Stage 3 high-confidence set contained 5,112 genes, with 3,794 UP and 1,318 DOWN. This set is not directly comparable to the primary 4,362-gene list as if it were a filtered subset: the Stage 3 code begins from the raw prediction table and applies different symbol and direction rules. Its gene-level criteria require only that the minimum enhancer FDR for a gene be below .05 and that the maximum absolute region-level difference reach .25. Thus, those criteria do not mean every linked region was significant or directionally concordant. The high-confidence label should be understood as a pipeline-defined filter, not as experimental validation.

The repaired score-versus-distance Spearman correlation was −0.021 (P \= 1.43 × 10⁻⁴¹). The very small correlation is consistent with the weak distance dependence expected when the additive pseudocount dominates much of the power-law range. The small P value is not evidence that the contact function is biologically informative; it reflects the large number of pairwise rows. With the score rebuilt only from retained rows, it cannot be interpreted as a canonical ABC fraction.

The Stage 3 counts and diagnostics are summarized in Table 3\.

**Table 3\. Stage 3 gene-level and score-audit diagnostics**

| Diagnostic | Reported result |
| ----- | ----- |
| Enhancer–gene pairs | 471,819 |
| Raw target genes | 21,516 |
| Artifact-symbol genes flagged | 3,159 |
| Raw direction counts | MIXED: 15,261; UP: 4,408; DOWN: 1,847 |
| Median direction purity among non-artifact genes | 0.632 |
| Non-artifact genes with purity ≥0.70 | 28.9% |
| Non-artifact genes with purity approximately 0.50 | 21.6% |
| Median retained enhancer links per gene | 22 |
| High-confidence target genes | 5,112 |
| High-confidence UP / DOWN genes | 3,794 / 1,318 |
| Repaired score versus distance | Spearman ρ \= −0.021; P \= 1.43 × 10⁻⁴¹ |

*The high-confidence set used the pipeline-defined filters described in Methods. “High-confidence” does not denote experimental validation.*

Stage 3 also revealed annotation signals that require caution. Its UP over-representation included taste transduction, driven by a group of Tas2r genes, and P2Y-receptor signaling. These are gene-family annotations and should not be treated as equally strong cortical mechanisms alongside the primary cAMP or synapse-organization results. The DOWN set was dominated in several terms by keratin genes and H2/MHC-region genes, producing annotations such as keratinization, epithelial differentiation, allograft rejection, antigen presentation, and viral-infection pathways. A 5-megabase linking window can assign one broad acetylated neighborhood to multiple nearby family members. The available summary does not report unique-mapping or locus-level checks that could distinguish such geometry from sample contamination or other technical explanations. These annotations therefore warrant review, not interpretation as DOI-induced epithelial or immune biology.

The Stage 3 signed GSEA similarly placed intermediate-filament and epithelial gene sets toward the negative end of the enhancer-weighted ranking. A negative normalized enrichment score means that members of the set are concentrated toward the down-ranked end of this enhancer-linked statistic. It does not show pathway suppression at the RNA or functional level. In the same analysis, UV-response and interferon hallmarks appeared at the permissive FDR threshold. They are signatures of gene-set membership, not evidence of a UV exposure, cytokine response, or immune mechanism. Selected Stage 3 ORA and GSEA results are shown in Table 4\.

**Table 4\. Selected Stage 3 enrichment results requiring cautious interpretation**

| Analysis | Library | Term | Reported statistic |
| ----- | ----- | ----- | ----- |
| HC\_UP ORA | KEGG | Taste transduction | Adjusted P \= 1.09 × 10⁻² |
| HC\_UP ORA | Reactome | P2Y Receptors | Adjusted P \= 4.59 × 10⁻³ |
| HC\_DOWN ORA | GO Biological Process | Intermediate filament organization | Adjusted P \= 1.39 × 10⁻⁹ |
| HC\_DOWN ORA | GO Biological Process | Epithelial cell differentiation | Adjusted P \= 5.84 × 10⁻⁹ |
| HC\_DOWN ORA | GO Biological Process | Epithelium development | Adjusted P \= 5.97 × 10⁻⁶ |
| HC\_DOWN ORA | KEGG | Herpes simplex virus 1 infection | Adjusted P \= 8.18 × 10⁻¹⁴ |
| HC\_DOWN ORA | KEGG | Allograft rejection | Adjusted P \= 5.66 × 10⁻¹³ |
| HC\_DOWN ORA | Reactome | Keratinization | Adjusted P \= 7.62 × 10⁻¹³ |
| Signed GSEA | GO Biological Process | Intermediate filament organization | NES \= −4.60; FDR \= 0.00 |
| Signed GSEA | GO Biological Process | Epithelial cell differentiation | NES \= −3.23; FDR \= 0.00 |
| Signed GSEA | KEGG | Herpes simplex virus 1 infection | NES \= −5.23; FDR \= 0.00 |
| Signed GSEA | MSigDB Hallmark | UV response down | NES \= \+1.97; FDR \= 1.16 × 10⁻¹ |
| Signed GSEA | MSigDB Hallmark | Interferon gamma response | NES \= \+1.77; FDR \= 2.04 × 10⁻¹ |

*Abbreviations: FDR, false discovery rate; GO, Gene Ontology; GSEA, gene set enrichment analysis; HC\_DOWN/HC\_UP, Stage 3 high-confidence DOWN/UP sets; NES, normalized enrichment score; ORA, over-representation analysis.*

### **RNA, orthology, and network analysis**

No joined day-7 RNA analysis was completed. The Stage 3 summary explicitly reports that the RNA means table was absent. Therefore, this study cannot say whether the UP-associated genes had larger day-7 RNA changes than background or whether enhancer direction agreed with RNA direction. The source paper’s observation that many transcript changes relaxed while some H3K27ac changes persisted makes discordance plausible, but plausibility is not a substitute for testing it.

The mouse-to-human orthology output mapped zero of 5,112 Stage 3 genes. This is a failed mapping result, not evidence that the genes lack human orthologs. The resulting human gene-set file is empty and should not be used for translational or cross-species analyses. The STRING network was also sparse in the Stage 3 summary, with 35 nodes and 23 edges at the chosen confidence threshold. It does not independently validate a synaptic module.

## **Discussion**

### **What this analysis shows**

The primary result is a candidate association between a curated UP-associated gene list and annotations involving synapse organization, neuron-projection morphogenesis, axon guidance, neuronal-system components, and cAMP signaling. The principal numerical findings are the enrichment of synapse organization in the all-target list and the concentration of multiple synaptic and neuronal terms in the UP list. The Stage 3 analysis shows that many raw gene assignments have mixed region directions and that its cleaned-UP list retains some neuronal members in overlaps with previously enriched terms.

This interpretation agrees in broad outline with the source paper’s report of persistent H3K27ac at enhancer-associated regions and its interpretation of chromatin changes near synapse-organization genes. It is not an independent confirmation of that result. The current reanalysis uses a different region definition and linker, and its leading-edge procedure reuses the earlier enrichment’s terms. It does not establish 5-HT2A dependence, transcript change, enhancer function, or circuit outcome. The results are best described as a gene-list association from a computational re-linking analysis.

### **Why “metaplastic state” is not a conclusion**

Metaplasticity is a useful concept when prior activity changes the capacity or rules governing later plasticity. It is not synonymous with a persistent chromatin mark, an enrichment result, or a current change in synaptic strength. Establishing metaplasticity would require a later challenge or plasticity assay demonstrating altered response thresholds or capacity. This reanalysis contains no such assay.

The source paper offers relevant functional context: it reported DOI-associated spine changes and increased cortical LTP at 24 hours, and receptor dependence for specified spine and fear-extinction outcomes \[3\]. Those measurements were made in that study, not in this reanalysis, and the 24-hour electrophysiology does not demonstrate altered LTP at day 7\. The present gene list cannot extend those functional findings by itself.

A cautious model is that a single DOI exposure may leave some regulatory marks near genes whose known functions involve synapses and signaling, after much of the acute transcriptional response has subsided. If those marks affect the response to later activity, they could be relevant to metaplasticity. That is a testable hypothesis, not an observed state. It would require linking specific regions to genes, demonstrating an effect on gene regulation, and measuring responses to a subsequent plasticity-inducing event.

The distinction also prevents a false “more excitation” interpretation. Long-term potentiation and depression are multiple processes, not opposite readouts of one global synaptic-strength dial \[22,23\]. Sustained 5-HT2A stimulation can depress AMPA-mediated transmission in prefrontal slices, while other experimental settings show facilitated NMDA transmission or potentiation \[20,21\]. In the present results, a positive enhancer-linked direction near Grin2b or Gria3 identifies the side of the H3K27ac comparison, not a prediction of increased receptor activity or stronger excitatory transmission.

### **A working model, clearly labeled as a model**

The biological model motivating follow-up begins with acute 5-HT2A signaling. DOI activates 5-HT2A-linked pathways that include G-protein signaling and calcium-dependent processes; these acute events occur on a timescale distinct from the day-7 chromatin measurement \[1\]. Prior work has described DOI-associated effects on synaptic transmission and plasticity in cortical preparations, including distinct effects on NMDA- and AMPA-mediated responses \[20,21\]. The current reanalysis did not measure receptor occupancy, intracellular calcium, phosphorylation, or receptor trafficking.

In a second, activity-linked step, calcium and other signals can regulate transcription and chromatin. Neuronal activity-dependent signaling is known to connect calcium influx with transcriptional programs that include genes involved in synapse development and remodeling \[32\]. H3K27ac is associated with active regulatory elements, but measuring that mark does not identify the causal target gene or show that its RNA changed \[33\]. The source study reported that some enhancer-associated H3K27ac changes persisted after most transcript changes had diminished. That result offers a precedent for chromatin–RNA discordance in this dataset, but this reanalysis did not test discordance directly.

A third step would be a persistent regulatory effect on how neurons respond to later input. The present data cannot distinguish a mark that changes future responsiveness from a residual correlate of the earlier response. Activity-dependent genes and synaptic organizers provide plausible candidates for follow-up, but no later glutamate or neuromodulatory challenge was administered here. Finally, gains and losses across regions could reflect remodeling, compensation, or both. They should not be collapsed into a single directional claim about excitation, synapse number, or circuit rewiring.

Structural plasticity studies in other psychedelic models provide context but not direct support for a DOI-specific mechanism in this dataset. Studies have reported structural and functional plasticity following several psychedelic compounds \[2\], persistent dendritic spine changes after psilocybin \[29\], and effects of a non-hallucinogenic psychedelic analogue in animal and cellular assays \[30\]. These studies support the broader idea that drug effects on structure may outlast acute exposure; they do not demonstrate that the regions or genes nominated here mediate those effects. Proposed TrkB-related mechanisms should likewise not be transferred from other compounds to DOI without a DOI-specific test \[2,31\].

### **Translational relevance: a research question, not a treatment claim**

The translational value of this gene list is that it points to biological nodes that can be tested in future work. It does not provide a biomarker, a dosing recommendation, or evidence that changing any candidate pathway would extend or improve a post-session effect. In particular, there are no human samples, no psychotherapy-timing comparison, and no clinical outcome measures in this reanalysis.

The cAMP-linked group is one practical starting point. ADCY1 connects calcium/calmodulin signaling to cAMP production, while PDE4 enzymes hydrolyze cAMP and can constrain its signaling over time and space \[24-26\]. That makes the Adcy1–Pde4b pairing a plausible target for experimental follow-up: researchers could ask whether perturbing either node changes the duration or consequences of DOI-associated chromatin changes. It does not justify the claim that DOI acts through cAMP as its primary receptor pathway, nor that PDE4 inhibition would extend a plasticity window. CREB and RSK-related genes provide additional candidate readouts, but the current data do not show their phosphorylation or activity.

The glutamate-receptor and calcium-channel candidates raise a parallel question. Grin2a, Grin2b, Gria3, Cacna2d1, and Cacnb4 could be tested for RNA and protein changes, followed by receptor or channel physiology. Because prefrontal 5-HT2A effects can differ across synaptic conditions, these candidates should not be framed as a uniformly excitatory module. If future work considers pharmacological perturbation of PDE4, NMDA or AMPA receptors, or calcium-channel-associated proteins, it should do so as a controlled mechanistic experiment. Nothing in the present analysis supports combining these agents with DOI or using them clinically.

The adhesion candidates have a different translational status. Nlgn1, Nrxn1, and the Slitrk family help organize synaptic contacts, but the reported gene-level associations do not establish druggable targets. Their near-term value is as candidates for molecular validation, including region perturbation and measurement of target-gene response. They might eventually contribute to a biomarker strategy, but the current study identifies no peripheral assay, imaging marker, or human ortholog set that could serve that purpose.

The broader clinical hypothesis is modest: if a single psychedelic exposure produces a measurable interval during which experience-dependent learning is altered, synaptic and regulatory candidates such as these could help guide experiments on timing. The present dataset does not identify that interval in people or show that clinical protocols should be changed. It gives researchers a set of hypotheses to test in follow-up models that include later behavioral learning, perturbation of nominated regions or genes, and carefully timed transcriptomic and physiological measurements.

### **What the DOWN terms do—and do not—mean**

The Stage 3 DOWN set generated strong epithelial, keratin, and MHC-related annotations. Those terms are dominated by gene families and clusters, including many Krt and H2 symbols. They should be reported as annotation signals driven by those gene families, not as DOI-induced keratinization, immune suppression, antigen presentation, or antiviral activity. A broad candidate window can assign nearby regions to multiple members of a family, and the reanalysis did not report the mapping-level checks needed to determine whether this explains the pattern.

The same caution applies to GSEA labels such as herpes simplex virus infection, allograft rejection, UV response, and interferon response. These labels arise from overlap between ranked genes and curated gene sets. They do not demonstrate infection, UV exposure, cytokine release, or a stress response. The results are a quality-control prompt: inspect loci, mapping, and candidate genes before attaching biological meaning.

### **Limitations and the reviewer’s concerns**

The first limitation is replicate handling. The source study reports six brain samples per condition and two technical replicates per brain sample. The reanalysis passes library-level columns to an independent two-sample test without collapsing technical replicates or modeling mouse identity. A mouse-level reanalysis should average technical libraries for each brain sample or use an appropriate model that accounts for that structure. Until then, region-level nominal P values and FDRs may be too optimistic.

The second limitation is the enhancer-to-gene model. The saved table’s maximum score of 39.176 conflicts with the normalization in the pipeline code. Stage 3’s reconstruction from saved AxC rows cannot restore the omitted below-threshold candidates and does not recover a canonical ABC denominator. The near-zero repaired score–distance correlation reinforces the need to recompute links on the complete candidate set, with a specified contact map where available and a clear promoter definition. Even a corrected ABC score would still nominate rather than prove a regulatory relationship; causal claims require perturbation experiments such as CRISPR interference followed by measurement of the proposed target gene \[6,34,35\].

The third limitation is gene-set construction. The primary 4,362-gene list, its best-pair UP/DOWN assignment, and the Stage 3 5,112-gene set are not interchangeable. Stage 3 is not a strict nested validation of the primary list; it begins from the raw prediction table and applies a separate set of filters. The leading-edge recovery then intersects the old enrichment members with Stage 3 UP genes. It is useful as an overlap description, but it does not re-test the old synaptic terms or create an independent replication. Future work should prespecify a single gene-set rule, retain complete candidate pairs for linking, and report the resulting primary and secondary sets distinctly.

The fourth limitation is the absence of RNA integration. Smart-seq2 data were generated in the source experiment, but the present pipeline did not create a joined RNA table or test day-7 expression concordance. The appropriate next analysis is to join the RNA counts or expression matrix to the gene-level candidate set, test the magnitude of RNA change relative to a defined background, and assess direction concordance while accounting for the experimental design. A lack of RNA change would not automatically invalidate a persistent chromatin association, especially given the source paper’s reported transcript–chromatin dissociation. But that possibility should be evaluated rather than assumed.

Additional limitations include the nonstandard high-signal region definition, lack of promoter exclusion, broad 5-megabase window, absence of a new 5-HT2A antagonist or knockout arm, sparse STRING network, and failed mouse-to-human orthology mapping. The orthology file is empty because the mapping step returned zero matches; uppercasing mouse symbols in a GMT file does not create human orthologs. These issues preclude a human companion-diagnostic interpretation.

## **Conclusion**

A single DOI dose is associated, at 7 days, with H3K27ac remodeling in NeuN-positive frontal-cortex nuclei near genes annotated to synaptic adhesion, receptor organization, calcium-channel composition, and cyclic-nucleotide signaling. In the curated primary analysis, the reported synaptic and neuronal enrichment was concentrated in the UP-associated list, with synapse organization, projection morphogenesis, axon guidance, and cAMP signaling among the leading annotations. Stage 3 showed that direction assignments are mixed for many genes and that separate DOWN-set enrichments are dominated by gene-family signals requiring locus review.

The conclusion is a candidate regulatory association, not a completed mechanism. The analysis does not show that a nominated region controls a particular gene, that the target gene’s RNA changed, that receptor or circuit function changed at day 7, or that a multiday metaplastic state was demonstrated. The next steps are to recover missing region counts, reanalyze technical replicates at the mouse level, rebuild enhancer–gene scores on the full candidate set, and join the RNA data. Perturbation of selected regulatory regions, followed by target-gene and functional assays, is needed to test causality. The gene list can guide those experiments; it is not yet a biomarker or clinical recommendation.

## **References**

1. Nichols DE. Psychedelics. *Pharmacol Rev.* 2016;68(2):264-355. doi:10.1124/pr.115.011478

2. Ly C, Greb AC, Cameron LP, et al. Psychedelics promote structural and functional neural plasticity. *Cell Rep.* 2018;23(11):3170-3182. doi:10.1016/j.celrep.2018.05.022

3. de la Fuente Revenga M, Zhu B, Guevara CA, et al. Prolonged epigenomic and synaptic plasticity alterations following single exposure to a psychedelic in mice. *Cell Rep.* 2021;37(3):109836. doi:10.1016/j.celrep.2021.109836

4. González-Maeso J, Yuen T, Ebersole BJ, et al. Transcriptome fingerprints distinguish hallucinogenic and nonhallucinogenic 5-hydroxytryptamine 2A receptor agonist effects in mouse somatosensory cortex. *J Neurosci.* 2003;23(26):8836-8843. doi:10.1523/JNEUROSCI.23-26-08836.2003

5. González-Maeso J, Weisstaub NV, Zhou M, et al. Hallucinogens recruit specific cortical 5-HT2A receptor-mediated signaling pathways to affect behavior. *Neuron.* 2007;53(3):439-452. doi:10.1016/j.neuron.2007.01.008

6. Fulco CP, Nasser J, Jones TR, et al. Activity-by-contact model of enhancer-promoter regulation from thousands of CRISPR perturbations. *Nat Genet.* 2019;51(12):1664-1669. doi:10.1038/s41588-019-0538-0

7. Cao Z, Chen C, He B, Tan K, Lu C. A microfluidic device for epigenomic profiling using 100 cells. *Nat Methods.* 2015;12(10):959-962. doi:10.1038/nmeth.3488

8. Picelli S, Björklund ÅK, Faridani OR, et al. Smart-seq2 for sensitive full-length transcriptome profiling in single cells. *Nat Methods.* 2013;10(11):1096-1098. doi:10.1038/nmeth.2639

9. Picelli S, Faridani OR, Björklund ÅK, Winberg G, Sagasser S, Sandberg R. Full-length RNA-seq from single cells using Smart-seq2. *Nat Protoc.* 2014;9:171-181. doi:10.1038/nprot.2014.006

10. Zhu B, Hsieh YP, Murphy TW, Zhang Q, Naler LB, Lu C. MOWChIP-seq for low-input and multiplexed profiling of genome-wide histone modifications. *Nat Protoc.* 2019;14(12):3366-3394. doi:10.1038/s41596-019-0223-x

11. Chen EY, Tan CM, Kou Y, et al. Enrichr: interactive and collaborative HTML5 gene list enrichment analysis tool. *BMC Bioinformatics.* 2013;14:128. doi:10.1186/1471-2105-14-128

12. Subramanian A, Tamayo P, Mootha VK, et al. Gene set enrichment analysis: a knowledge-based approach for interpreting genome-wide expression profiles. *Proc Natl Acad Sci U S A.* 2005;102(43):15545-15550. doi:10.1073/pnas.0506580102

13. Scheiffele P, Fan J, Choih J, Fetter R, Serafini T. Neuroligin expressed in nonneuronal cells triggers presynaptic development in contacting axons. *Cell.* 2000;101(6):657-669. doi:10.1016/S0092-8674(00)80877-6

14. Südhof TC. Neuroligins and neurexins link synaptic function to cognitive disease. *Nature.* 2008;455(7215):903-911. doi:10.1038/nature07456

15. Südhof TC. Synaptic neurexin complexes: a molecular code for the logic of neural circuits. *Cell.* 2017;171(4):745-769. doi:10.1016/j.cell.2017.10.024

16. Sheng M, Kim E. The postsynaptic organization of synapses. *Cold Spring Harb Perspect Biol.* 2011;3(12):a005678. doi:10.1101/cshperspect.a005678

17. Yim YS, Kwon Y, Nam J, et al. Slitrks control excitatory and inhibitory synapse formation with LAR receptor protein tyrosine phosphatases. *Proc Natl Acad Sci U S A.* 2013;110(10):4057-4062. doi:10.1073/pnas.1209881110

18. Paoletti P, Bellone C, Zhou Q. NMDA receptor subunit diversity: impact on receptor properties, synaptic plasticity and disease. *Nat Rev Neurosci.* 2013;14(6):383-400. doi:10.1038/nrn3504

19. Traynelis SF, Wollmuth LP, McBain CJ, et al. Glutamate receptor ion channels: structure, regulation, and function. *Pharmacol Rev.* 2010;62(3):405-496. doi:10.1124/pr.109.002451

20. Berthoux C, Barre A, Bockaert J, Marin P, Bécamel C. Sustained activation of postsynaptic 5-HT2A receptors gates plasticity at prefrontal cortex synapses. *Cereb Cortex.* 2019;29(4):1659-1669. doi:10.1093/cercor/bhy064

21. Barre A, Berthoux C, De Bundel D, et al. Presynaptic serotonin 2A receptors modulate thalamocortical plasticity and associative learning. *Proc Natl Acad Sci U S A.* 2016;113(10):E1382-E1391. doi:10.1073/pnas.1525586113

22. Citri A, Malenka RC. Synaptic plasticity: multiple forms, functions, and mechanisms. *Neuropsychopharmacology.* 2008;33:18-41. doi:10.1038/sj.npp.1301559

23. Malenka RC, Bear MF. LTP and LTD: an embarrassment of riches. *Neuron.* 2004;44(1):5-21. doi:10.1016/j.neuron.2004.09.012

24. Conti M, Beavo J. Biochemistry and physiology of cyclic nucleotide phosphodiesterases: essential components in cyclic nucleotide signaling. *Annu Rev Biochem.* 2007;76:481-511. doi:10.1146/annurev.biochem.76.060305.150444

25. Houslay MD, Baillie GS, Maurice DH. cAMP-specific phosphodiesterase-4 enzymes in the cardiovascular system: a molecular toolbox for generating compartmentalized cAMP signaling. *Circ Res.* 2007;100(7):950-966. doi:10.1161/01.RES.0000261934.56938.38

26. Wong ST, Athos J, Figueroa XA, et al. Calcium-stimulated adenylyl cyclase activity is critical for hippocampus-dependent long-term memory and late-phase LTP. *Neuron.* 1999;23(4):787-798. doi:10.1016/S0896-6273(01)80036-2

27. Lonze BE, Ginty DD. Function and regulation of CREB family transcription factors in the nervous system. *Neuron.* 2002;35(4):605-623. doi:10.1016/S0896-6273(02)00828-0

28. Silva AJ, Kogan JH, Frankland PW, Kida S. CREB and memory. *Annu Rev Neurosci.* 1998;21:127-148. doi:10.1146/annurev.neuro.21.1.127

29. Shao LX, Liao C, Gregg I, et al. Psilocybin induces rapid and persistent growth of dendritic spines in frontal cortex in vivo. *Neuron.* 2021;109(16):2535-2544.e4. doi:10.1016/j.neuron.2021.06.008

30. Cameron LP, Tombari RJ, Lu J, et al. A non-hallucinogenic psychedelic analogue with therapeutic potential. *Nature.* 2021;589(7842):474-479. doi:10.1038/s41586-020-3008-z

31. Olson DE. Biochemical mechanisms underlying psychedelic-induced neuroplasticity. *Biochemistry.* 2022;61(3):127-136. doi:10.1021/acs.biochem.1c00812

32. Flavell SW, Greenberg ME. Signaling mechanisms linking neuronal activity to gene expression and plasticity of the nervous system. *Annu Rev Neurosci.* 2008;31:563-590. doi:10.1146/annurev.neuro.31.060407.125631

33. Creyghton MP, Cheng AW, Welstead GG, et al. Histone H3K27ac separates active from poised enhancers and predicts developmental state. *Proc Natl Acad Sci U S A.* 2010;107(50):21931-21936. doi:10.1073/pnas.1016071107

34. Fulco CP, Munschauer M, Anyoha R, et al. Systematic mapping of functional enhancer-promoter connections with CRISPR interference. *Science.* 2016;354(6313):769-773. doi:10.1126/science.aag2445

35. Gasperini M, Hill AJ, McFaline-Figueroa JL, et al. A genome-wide framework for mapping gene regulation via cellular genetic screens. *Cell.* 2019;176(1-2):377-390.e19. doi:10.1016/j.cell.2018.11.029

# **Appendix**

## **1\. Study and Analysis Metadata**

**Table 1\. Analysis metadata and thresholds**

| Source | Field | Value |
| :---- | :---- | :---- |
| Stage 3 | Generated | 2026-10-01 15:24:34 |
| Stage 3 | Dataset | GSE161626 — DOI (5-HT2A) persistent H3K27ac enhancers, 7d, mouse |
| Stage 3 | Input table | /content/ABC\_DOI/ABC\_output/EnhancerPredictions\_persistent\_7d.tsv.gz |
| Stage 3 | Enhancer-gene pairs | 471819 |
| Stage 3 | Raw target genes | 21516 |
| Stage 3 | Direction-purity threshold | \> 0.70 |
| Stage 3 | FDR threshold | \< 0.05 |
| Stage 3 | Maximum absolute LFC threshold | \>= 0.25 |
| Stage 3 | Minimum enhancers per gene | \>= 2 |
| Stage 3 | ABC threshold if repaired | \>= 0.02 |
| Stage 3 | ORA FDR threshold | \< 0.05 |
| Stage 3 | GSEA FDR threshold | \< 0.25 |
| Stage 3 | STRING confidence and taxon | \>= 0.70; taxon 10090 |
| Prior downstream summary | Generated | 2026-10-01 11:00:11 |
| Prior downstream summary | Gene source | final\_gene\_list\_final.txt (curated mouse protein-coding) |
| Prior downstream summary | Analysis genes | 4362; UP=2895; DOWN=1467 |
| Prior downstream summary | Genes with ABC | 4362 |
| Prior downstream summary | STRING network | 22 nodes, 12 edges; confidence \>= 0.7; taxon 10090 |
| Prior downstream summary | ORA cutoff | adjusted P \< 0.05 |
| Prior downstream summary | GSEA cutoff | FDR q \< 0.25 |

## **2\. Stage 3 Score Audit**

**Table 2\. ABC score audit**

| Metric | Value |
| :---- | :---- |
| Stored ABC\_score range | \[0.0200, 39.1760\] |
| Repair mode | repaired\_from\_AxC |
| Score column used | ABC\_repaired |
| Repaired ABC maximum | 0.5248665342484397 |
| Interpretation | Stored score exceeded 1; a true within-gene ABC fraction was rebuilt from AxC. |

## **3\. Gene-Level Collapse and Architecture**

**Table 3\. Gene-level collapse**

| Metric | Value |
| :---- | ----- |
| Total genes | 21516 |
| MIXED direction | 15261 |
| UP direction | 4408 |
| DOWN direction | 1847 |
| Artifact symbols | 3159 |
| Mixed-direction genes dropped from UP/DOWN | 13245 |
| High-confidence genes | 5112 |
| High-confidence UP genes | 3794 |
| High-confidence DOWN genes | 1318 |

**Table 4\. Architecture diagnostics**

| Diagnostic | Value | Interpretation |
| :---- | :---- | :---- |
| Direction-purity median | 0.632 | Median direction purity across non-artifact genes. |
| Fraction with purity \>= 0.70 | 0.289 | Fraction meeting the direction-purity threshold. |
| Fraction with purity approximately 0.50 | 0.216 | Potential noise flag if high. |
| Enhancers per gene median | 22.0 | Median number of linked enhancers per gene. |
| Fraction single-enhancer genes | 0.000 | Power-law failure mode not indicated by this diagnostic. |
| Repaired-score versus distance | Spearman rho=−0.021; p=1.43e−41 | Expected negative relationship was observed statistically, but with a very small effect size. |

## **4\. High-Confidence Target Genes**

**Table 5\. Top UP targets by enhancer-weighted LFC**

| Rank | Gene | Weighted LFC | Max absolute LFC | Number of enhancers | Best ABC |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Cdh10 | \+1.082 | 1.384 | 8 | 0.2378 |
| 2 | Samd9l | \+0.690 | 1.748 | 9 | 0.1778 |
| 3 | Atp11c | \+0.655 | 1.556 | 40 | 0.0839 |
| 4 | F9 | \+0.653 | 1.556 | 38 | 0.0874 |
| 5 | Mcf2 | \+0.650 | 1.556 | 42 | 0.0869 |
| 6 | Fgf13 | \+0.641 | 1.556 | 37 | 0.089 |
| 7 | Acot10 | \+0.639 | 1.384 | 13 | 0.1528 |
| 8 | Cdh12 | \+0.639 | 1.384 | 13 | 0.1529 |
| 9 | Sox3 | \+0.621 | 1.556 | 45 | 0.0792 |
| 10 | Slitrk5 | \+0.614 | 1.291 | 17 | 0.1026 |
| 11 | Ldoc1 | \+0.605 | 1.556 | 50 | 0.0785 |
| 12 | Smarcad1 | \+0.604 | 0.948 | 7 | 0.2231 |
| 13 | Hpgds | \+0.604 | 0.948 | 7 | 0.2231 |
| 14 | Cdr1 | \+0.601 | 1.556 | 47 | 0.0768 |
| 15 | Slc1a7 | \+0.598 | 1.589 | 22 | 0.0801 |
| 16 | Podn | \+0.598 | 1.589 | 22 | 0.0801 |
| 17 | Scp2 | \+0.598 | 1.589 | 22 | 0.0801 |
| 18 | Mbnl1 | \+0.595 | 1.451 | 23 | 0.0698 |
| 19 | Fscb | \+0.593 | 1.837 | 15 | 0.1301 |
| 20 | Slitrk6 | \+0.591 | 1.291 | 14 | 0.1288 |
| 21 | Czib | \+0.588 | 1.589 | 23 | 0.0752 |
| 22 | Cpt2 | \+0.588 | 1.589 | 23 | 0.0752 |
| 23 | Rnd3 | \+0.583 | 1.172 | 18 | 0.1085 |
| 24 | Tas2r134 | \+0.583 | 1.172 | 18 | 0.1085 |
| 25 | Rbm43 | \+0.583 | 1.172 | 18 | 0.1085 |
| 26 | Nmi | \+0.583 | 1.172 | 18 | 0.1085 |
| 27 | Tnfaip6 | \+0.583 | 1.172 | 18 | 0.1085 |
| 28 | Rif1 | \+0.583 | 1.172 | 18 | 0.1085 |
| 29 | Neb | \+0.583 | 1.172 | 18 | 0.1086 |
| 30 | Arl5a | \+0.583 | 1.172 | 18 | 0.1086 |

**Table 6\. Top DOWN targets by enhancer-weighted LFC**

| Rank | Gene | Weighted LFC | Max absolute LFC | Number of enhancers | Best ABC |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Zik1 | −0.697 | 1.096 | 6 | 0.4101 |
| 2 | Nlrp4b | −0.697 | 1.096 | 6 | 0.4101 |
| 3 | Zscan4b | −0.697 | 1.096 | 6 | 0.4101 |
| 4 | Zscan4c | −0.697 | 1.096 | 6 | 0.4101 |
| 5 | Zscan4-ps1 | −0.697 | 1.096 | 6 | 0.4101 |
| 6 | Zscan4d | −0.482 | 1.096 | 7 | 0.3386 |
| 7 | Zscan4e | −0.451 | 0.790 | 7 | 0.315 |
| 8 | Zscan4f | −0.449 | 0.790 | 8 | 0.2608 |
| 9 | Zscan4-ps2 | −0.423 | 0.790 | 9 | 0.2144 |
| 10 | Nlrp5 | −0.401 | 0.895 | 24 | 0.107 |
| 11 | Zfp112 | −0.398 | 0.895 | 24 | 0.0792 |
| 12 | Zfp235 | −0.398 | 0.895 | 24 | 0.0812 |
| 13 | Nlrp4e | −0.398 | 0.895 | 24 | 0.1079 |
| 14 | V1rd19 | −0.397 | 0.895 | 24 | 0.0801 |
| 15 | Zfp180 | −0.397 | 0.895 | 24 | 0.0801 |
| 16 | Zfp114 | −0.392 | 0.853 | 24 | 0.082 |
| 17 | Zfp94 | −0.392 | 0.853 | 24 | 0.082 |
| 18 | Zfp61 | −0.392 | 0.853 | 24 | 0.082 |
| 19 | Zfp111 | −0.392 | 0.853 | 24 | 0.082 |
| 20 | Zfp109 | −0.391 | 0.853 | 24 | 0.082 |
| 21 | Gpr108 | −0.390 | 0.874 | 14 | 0.152 |
| 22 | Trip10 | −0.390 | 0.874 | 14 | 0.152 |
| 23 | Vav1 | −0.390 | 0.874 | 14 | 0.152 |
| 24 | Adgre1 | −0.390 | 0.874 | 14 | 0.152 |
| 25 | Zfp93 | −0.389 | 0.853 | 23 | 0.0838 |
| 26 | Abcf1 | −0.385 | 1.052 | 13 | 0.1749 |
| 27 | Gnl1 | −0.381 | 1.052 | 13 | 0.1788 |
| 28 | Prr3 | −0.381 | 1.052 | 13 | 0.1788 |
| 29 | H2-T24 | −0.381 | 1.052 | 13 | 0.1791 |
| 30 | Ppp1r10 | −0.381 | 1.052 | 13 | 0.1791 |

## **5\. Stage 3 ORA Results**

**Table 7\. Significant ORA term counts by cleaned target set**

| Target set | GO Biological Process 2023 | KEGG 2019 Mouse | Reactome 2022 | MGI Mammalian Phenotype Level 4 2021 |
| :---- | :---- | :---- | :---- | :---- |
| HC\_UP | 0 | 8 | 1 | 0 |
| HC\_DOWN | 4 | 24 | 2 | 0 |

**Table 8\. Top HC\_UP ORA terms**

| Library | Term | Adjusted P | Combined score | Representative genes |
| :---- | :---- | :---- | :---- | :---- |
| KEGG 2019 Mouse | Taste transduction | 1.09e−02 | 26.3 | TAS2R131, TAS2R110, TAS2R134, TAS2R113, TAS2R135, TAS2R136, TAS2R115, TAS2R116, TAS2R117, TAS2R139, TAS2R119, P2RY4, SCN9A, P2RY1, TRPM5, SCN3A, GABRA5, TAS2R140, GABRA3, TAS2R120, TAS2R143, TAS2R121, TAS2R144, TAS2R122, TAS2R123, TAS2R102, TAS2R124, TAS2R103, TAS2R125, TAS2R126, TAS2R129, TAS2R109, SCN2A |
| KEGG 2019 Mouse | Tyrosine metabolism | 2.07e−02 | 30.9 | DDC, MAOB, TOMT, MAOA, TYRP1, ADH7, TYR, ADH5, ADH4, ALDH1A3, TPO, ADH1, AOX4, TH, DCT, AOX2, AOX3, AOX1 |
| KEGG 2019 Mouse | Mucin type O-glycan biosynthesis | 2.07e−02 | 36.3 | GALNT7, GALNT14, GALNT5, GALNT13, GALNT16, GALNT3, GALNT15, C1GALT1, C1GALT1C1, B3GNT6, GALNTL5, GCNT3, GCNT4, GALNTL6 |
| KEGG 2019 Mouse | Natural killer cell mediated cytotoxicity | 2.33e−02 | 15.9 | SHC4, IFNA5, IFNA4, IFNA7, IFNA6, IFNA1, IFNA2, ARAF, PIK3R1, IFNA9, PPP3R1, PAK1, CASP3, IFNAB, TNFSF10, MAPK1, CD244A, HRAS, IFNA11, IFNA12, IFNA13, IFNA14, IFNA15, IFNB1, IFNB1, FCER1G, RAET1D, RAET1E, FASL, FCGR4, FAS, LCP2, CD48, CD247, ULBP1, SOS2 |
| KEGG 2019 Mouse | Hepatitis B | 2.33e−02 | 14.4 | RB1, IFNA5, IFNA4, IFNA7, ATF2, DDX3X, IFNA6, IFNA1, IFNA2, ARAF, IFNA9, ELK1, CASP8, CASP3, IFNAB, IKBKG, JAK2, HRAS, JAK1, IFNA11, IFNA12, IFNA13, IFNA14, IFNA15, ATP6AP1, PRKCA, FOS, CCNA1, CCNE2, IRF7, SOS2, TLR3, TLR2, PIK3R1, MAPK8, IRAK1, MAPK1, EGR2, TGFB3, SLC10A1, IFNB1, NFKBIA, MAPK10, MAVS, FASL, FAS, TAB2 |
| KEGG 2019 Mouse | Cytosolic DNA-sensing pathway | 2.33e−02 | 19.8 | IFNA5, IFNA11, IFNA4, IFNA12, IL33, IFNA7, IFNA13, IFNA6, IFNA14, IFNA1, IFNA15, IFNB1, IFNA2, IL18, IFNA9, NFKBIA, MAVS, POLR3A, IFNAB, IRF7, POLR3G, IKBKG, POLR2L |
| KEGG 2019 Mouse | Measles | 2.33e−02 | 14.2 | IFNA5, RAB9, IFNA4, IFNA7, IFNA6, CDKN1B, IFNA1, IFNA2, PIK3R1, IFNA9, IL2RG, MAPK8, CASP8, IRAK1, CASP3, IFNAB, IKBKG, JAK1, IFNA11, IFNA12, IFNA13, IFNA14, IFNA15, IFNB1, MSN, HSPA2, FOS, EIF2S1, NFKBIA, MAPK10, IL1A, MAVS, CDK6, CCNE2, FASL, IL2RA, CYCT, IRF7, FAS, EIF3H, TAB2, RAB9B, TLR2 |
| KEGG 2019 Mouse | Hepatitis C | 3.51e−02 | 12.5 | RB1, IFNA5, IFNA4, IFNA7, RNASEL, IFNA6, IFNA1, CD81, IFNA2, ARAF, PIK3R1, CLDN2, IFIT1, IFNA9, EGFR, CLDN20, CASP8, PPP2R1B, CASP3, IFNAB, MAPK1, IKBKG, HRAS, JAK1, IFNA11, IFNA12, IFNA13, MAP2K1, IFNA14, RSAD2, IFNA15, IFNB1, CFLAR, IFIT1BL1, EIF2S1, NFKBIA, CLDN10, OCLN, MAVS, CDK6, FASL, FAS, SOS2, TLR3 |
| Reactome 2022 | P2Y Receptors R-HSA-417957 | 4.59e−03 | 274.5 | P2RY12, P2RY13, P2RY10, P2RY6, P2RY4, LPAR6, P2RY14, P2RY2, P2RY1, LPAR4 |

**Table 9\. Top HC\_DOWN ORA terms**

| Library | Term | Adjusted P | Combined score | Representative genes or gene families |
| :---- | :---- | :---- | :---- | :---- |
| GO Biological Process 2023 | Intermediate Filament Organization (GO:0045109) | 1.39e−09 | 239.2 | KRT24, KRT23, KRT20, KRT40, KRT33B, KRT33A, KRT28, KRT27, KRT26, KRT25, KRT35, KRT13, KRT34, KRT12, KRT32, KRT10, KRT31, KRT9, KRT19, KRT39, KRT17, KRT16, KRT15, KRT36, KRT14 |
| GO Biological Process 2023 | Epithelial Cell Differentiation (GO:0030855) | 5.84e−09 | 132.6 | EHF, HDAC1, KRT24, KRT23, GLI1, KRT20, KRT40, KRT33B, KRT33A, UPK1A, KRT28, KRT27, AKT2, KRT26, KRT25, CPT1A, KRT35, KRT13, KRT34, KRT12, KRT32, KRT10, OVOL1, KRT31, OVOL3, KRT9, KRT19, KRT39, KRT17, KRT16, WT1, KRT15, KRT36, KRT14 |
| GO Biological Process 2023 | Epithelium Development (GO:0060429) | 5.97e−06 | 72.2 | EHF, KRT24, KRT23, KRT20, KRT40, KRT33B, KRT33A, UPK1A, KRT28, KRT27, KRT26, DVL1, KRT25, CPT1A, TGFB1, KRT35, KRT13, KRT34, KRT12, KRT32, KRT10, KRT31, KRT9, KRT19, KRT39, KRT17, KRT16, WT1, KRT15, KRT36, KRT14, TIMELESS |
| GO Biological Process 2023 | Supramolecular Fiber Organization (GO:0097435) | 9.80e−03 | 25.2 | AVIL, KRT24, KRT23, PLOD3, KRT20, CORO1B, KRT40, KRT33B, CNN2, KRT33A, EFEMP2, KRT28, KRT27, KRT26, KRT25, NLRP5, MARCKSL1, CDSN, KRT35, KRT13, KRT34, KIF24, KRT32, TBCB, KRT10, KRT31, TLE6, KRT9, CCDC88B, CLIP3, KRT19, KRT39, KRT17, KRT16, KATNBL1, MYO1A, KRT15, KRT36, KRT14, CDC42EP2 |
| Reactome 2022 | Keratinization R-HSA-6805567 | 7.62e−13 | 162.1 | KRT24, KRT23, KRT20, CAPNS1, KRT28, KRT27, KRT26, KRT25, KRTAP1-5, KRTAP3-3, KRTAP1-4, CAPN1, KRTAP3-2, KRTAP1-3, KRTAP3-1, KRTAP9-3, KRTAP9-1, KRTAP17-1, KRT35, KRT34, KRT32, KRT31, KRT9, KRT39, KRT36, KRTAP29-1, KLK5, KLK8, KRT40, KRT33B, KRT33A, KRTAP4-2, KRTAP4-1, CDSN, KRTAP4-9, KRTAP16-1, KRTAP4-8, KRTAP4-6, KLK13, KRT13, KLK14, KRT12, KRT10, KLK12, KRT19, KRT17, KRT16, KRT15, KRT14 |
| Reactome 2022 | Developmental Biology R-HSA-1266738 | 1.48e−04 | 26.8 | NAB2, CASC3, TNF, ARHGAP35, PSMD8, RPS15, RPS6KA4, RPS16, CAPNS1, RPS19, AKT2, PSMD3, KRTAP3-3, CAPN1, KRTAP3-2, KRTAP3-1, EPHB4, SCN1B, PDHX, MAP2K2, RPL23, RPS5, KRTAP17-1, KRT9, CACNB1, MYL6, MAG, TYROBP, GRB7, SPTBN4, AGAP2, KLK5, KLK8, KRT40, KRT33B, KRT33A, KRTAP4-2, PIP5K1C, KRTAP4-1, SPTBN2, VASP, RPL41, SMAD4, KRTAP4-9, CDSN, TGFB1, JUP, KRTAP16-1, KRTAP4-8, KRTAP4-6, DRAP1, EFNA2, CDK4, CDK2, ONECUT3, KRT24, KRT23, HIF3A, MED16, KRT20, KRT28, KRT27, KRT26, KRT25, PRX, KRTAP1-5, KRTAP1-4, KRTAP1-3, KRTAP9-3, KRTAP9-1, KRT35, KRT34, PAX6, KRT32, KRT31, MED29, KAT2A, MED24, KRT39, KRT36, RARA, KRTAP29-1, PLPPR3, RELA, RBBP4, PSMB3, ERBB2, AP2S1, POLR2E, POLR2I, PAK4, RPL19, LGI4, PCGF2, KLK13, KRT13, KLK14, KRT12, KRT10, DEK, KLK12, KRT19, KRT17, PSMC4, WT1, KRT16, KRT15, KRT14, FAU, AGRN, FOXA3 |

## **6\. Stage 3 GSEA Results**

**Table 10\. Significant Stage 3 preranked GSEA terms**

| Library | Term | NES | FDR | Direction represented by the rank |
| :---- | :---- | :---- | :---- | :---- |
| GO Biological Process 2023 | Intermediate Filament Organization (GO:0045109) | −4.60 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| GO Biological Process 2023 | Epithelial Cell Differentiation (GO:0030855) | −3.23 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| GO Biological Process 2023 | Epithelium Development (GO:0060429) | −3.21 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| GO Biological Process 2023 | Epidermis Development (GO:0008544) | −2.31 | 2.46e−02 | Negative end of enhancer-weighted LFC ranking |
| GO Biological Process 2023 | Natural Killer Cell Activation Involved In Immune Response | −1.96 | 2.18e−01 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Herpes simplex virus 1 infection | −5.23 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Graft-versus-host disease | −5.10 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Allograft rejection | −4.50 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Type I diabetes mellitus | −4.48 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Antigen processing and presentation | −4.15 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Phagosome | −3.63 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Viral myocarditis | −3.45 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Human T-cell leukemia virus 1 infection | −3.23 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Cellular senescence | −3.18 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Estrogen signaling pathway | −3.01 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Viral carcinogenesis | −2.95 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| KEGG 2019 Mouse | Autoimmune thyroid disease | −2.83 | 0.00e+00 | Negative end of enhancer-weighted LFC ranking |
| MSigDB Hallmark 2020 | UV Response Dn | \+1.97 | 1.16e−01 | Positive end of enhancer-weighted LFC ranking |
| MSigDB Hallmark 2020 | Interferon Gamma Response | \+1.77 | 2.04e−01 | Positive end of enhancer-weighted LFC ranking |

*Positive NES indicates that a pathway is concentrated among genes whose linked enhancers gained H3K27ac. Negative NES describes the negative end of the enhancer-weighted ranking and does not, by itself, establish that DOI suppressed the pathway.*

## **7\. Stage 3 STRING Network**

**Table 11\. STRING network summary**

| Metric | Value |
| :---- | :---- |
| Network size | 35 nodes, 23 edges |
| Confidence threshold | \>= 0.70 |
| Taxon | 10090 |
| Join mode | carriage-return join |

**Table 12\. Stage 3 STRING hubs**

| Gene | Degree | Direction | Weighted LFC |
| :---- | :---- | :---- | :---- |
| Efhc2 | 2 | UP | \+0.296 |
| Dusp21 | 2 | UP | \+0.342 |
| Rps29 | 2 | UP | \+0.426 |
| Gpd2 | 2 | UP | \+0.403 |
| Rpl10l | 2 | UP | \+0.478 |
| Samt1d | 2 | UP | \+0.259 |
| Samt1 | 2 | UP | \+0.258 |
| Kdm5c | 2 | UP | \+0.536 |
| Rpl36al | 2 | UP | \+0.426 |
| Ribc1 | 2 | UP | \+0.528 |
| Samt1c | 2 | UP | \+0.258 |
| Araf | 1 | UP | \+0.378 |
| Kdm6a | 1 | UP | \+0.343 |
| Plpp3 | 1 | UP | \+0.546 |
| Hsd17b10 | 1 | UP | \+0.530 |
| Tspyl2 | 1 | UP | \+0.541 |
| Arf6 | 1 | UP | \+0.426 |
| Gpr143 | 1 | UP | \+0.521 |
| C8b | 1 | UP | \+0.546 |
| Gk | 1 | UP | \+0.548 |
| Pde4b | 1 | UP | \+0.501 |
| Wdr78 | 1 | UP | \+0.551 |
| Dab1 | 1 | UP | \+0.560 |
| Magea3 | 1 | UP | \+0.220 |
| Obp1b | 1 | UP | \+0.424 |

## **8\. Synaptic and Neuronal Leading Edge**

**Table 13\. Synaptic and neuronal terms represented in HC\_UP genes**

| Library | Term | Adjusted P | Total members | Members in HC\_UP | HC\_UP members |
| :---- | :---- | :---- | :---- | :---- | :---- |
| Reactome 2022 | Neuronal System R-HSA-112316 | 2.78e−02 | 118 | 33 | RPS6KA3, CACNA2D1, GABRG3, CACNB4, KCNQ1, SLC22A1, SLC1A1, CACNA1E, SLC1A7, KCNV1, GRIN2A, KCNV2, GRIA3, GRIN2B, LRRTM3, DLGAP1, SLC18A3, KCND2, ARHGEF9, LRRC7, IL1RAPL1, NLGN1, NRXN1, PRKX, AKAP5, GLRA2, SLITRK2, SLITRK4, SLITRK6, SLITRK5, CACNG4, GABRA5, GABRA3 |
| GO Biological Process 2023 | Synapse Organization (GO:0050808) | 3.02e−04 | 56 | 23 | APP, CTNND2, LRRC4, ZDHHC2, NRCAM, APPL1, MUSK, F2R, ANK3, DKK1, DNM3, CACNB4, GPM6A, NLGN1, ZC4H2, NRXN1, SLITRK2, SLITRK6, PAK3, RAB39B, REST, SDK2, ADGRL3 |
| GO Biological Process 2023 | Neuron Projection Morphogenesis (GO:0048812) | 1.95e−02 | 54 | 20 | APP, CTNND2, NRCAM, ATP7A, MEF2A, ANK3, SLC9A6, KIF20B, GPM6A, NLGN1, PLPPR4, SLITRK2, SLITRK4, SLITRK6, MAP6, SLITRK5, CTNNA2, PAK3, MYO16, CNTN4 |
| MGI Mammalian Phenotype Level 4 2021 | Reduced long term potentiation MP:0001473 | 2.18e−02 | 46 | 16 | SLC24A2, APP, FMR1, UBE3A, VLDLR, AKAP5, LRP8, GRIN2A, PAK3, TRPC5, CRBN, GRIN2B, ESR2, FGF14, SP4, ITM2B |
| MGI Mammalian Phenotype Level 4 2021 | Abnormal CNS synaptic transmission MP:0002206 | 3.11e−02 | 40 | 15 | CHRM2, NLGN1, FMR1, RPS6KA3, LRRTM3, PDE4B, IQSEC2, KIDINS220, CACNA2D1, GRIN2B, CACNB4, DLG2, FGF14, IL1RAPL1, CD247 |
| GO Biological Process 2023 | Positive Regulation Of Neuron Projection Development (GO:0010976) | 3.64e−02 | 37 | 15 | ALK, NLGN1, PLPPR5, VLDLR, LRP8, ALKAL2, ARSB, MUSK, KIDINS220, ZDHHC15, LRRC7, PLXNB3, ROR1, PRKD1, TOX |
| KEGG 2019 Mouse | Axon guidance | 2.39e−03 | 64 | 11 | LRRC4, ROBO1, TRPC5, PIK3R1, MYL12B, EFNB2, EFNB1, SLIT3, PAK3, PLXNA4, PLXNB3 |
| KEGG 2019 Mouse | Neurotrophin signaling pathway | 4.84e−02 | 41 | 10 | MAGED1, PIK3R1, RPS6KA3, MAPK8, ARHGDIB, MAP2K1, RIPK2, KIDINS220, MAPK10, SOS2 |
| MGI Mammalian Phenotype Level 4 2021 | Abnormal spatial working memory MP:0008428 | 2.80e−02 | 30 | 9 | GPR88, GRIN2A, PDE4B, IDS, FOXB1, RPS7, IQSEC2, LRRC7, ITM2B |
| MGI Mammalian Phenotype Level 4 2021 | Abnormal long term potentiation MP:0002207 | 2.80e−02 | 17 | 6 | APP, GABRA5, CTNND2, UBE3A, VLDLR, GRIA3 |

## **9\. Prior Downstream ABC Leaderboard**

**Table 14\. Top genes by prior ABC score**

| Rank | Gene | ABC score | Enhancer direction |
| :---- | :---- | :---- | :---- |
| 1 | Rhox9 | 39.176 | DOWN |
| 2 | Rhox5 | 38.5847 | DOWN |
| 3 | Wdr44 | 2.1909 | DOWN |
| 4 | Mcf2 | 1.8667 | UP |
| 5 | Sox3 | 1.7535 | UP |
| 6 | Atp11c | 0.8336 | UP |
| 7 | Gm7073 | 0.8333 | UP |
| 8 | Klhl13 | 0.7558 | DOWN |
| 9 | Pola1 | 0.7423 | DOWN |
| 10 | Btbd35f23 | 0.6493 | DOWN |
| 11 | Pcyt1b | 0.6169 | DOWN |
| 12 | Slitrk4 | 0.5677 | UP |
| 13 | Cul2 | 0.5249 | DOWN |
| 14 | AU015836 | 0.477 | DOWN |
| 15 | Gm5941 | 0.4728 | UP |
| 16 | Btbd35f18 | 0.4411 | DOWN |
| 17 | Rtl8a | 0.4328 | DOWN |
| 18 | Tbx22 | 0.4199 | UP |
| 19 | Rhox13 | 0.401 | DOWN |
| 20 | Cntnap5a | 0.3769 | UP |

## **10\. Prior ORA Summary**

**Table 15\. Significant prior ORA term counts**

| Target set | GO Biological Process 2023 | GO Molecular Function 2023 | GO Cellular Component 2023 | KEGG 2019 Mouse | WikiPathways 2019 Mouse | Reactome 2022 | MSigDB Hallmark 2020 | MGI Mammalian Phenotype Level 4 2021 | TRRUST Transcription Factors 2019 |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| ALL targets | 8 | 0 | 0 | 11 | 0 | 2 | 1 | 22 | 0 |
| UP targets | 10 | Not reported | Not reported | 6 | Not reported | 5 | Not reported | 12 | Not reported |
| DOWN targets | 0 | Not reported | Not reported | 0 | Not reported | 0 | Not reported | 0 | Not reported |

**Table 16\. Prior leading enrichment terms**

| Library | Term | Adjusted P | Combined score |
| :---- | :---- | :---- | :---- |
| GO Biological Process 2023 | Synapse Organization (GO:0050808) | 3.02e−04 | 44.9 |
| GO Biological Process 2023 | Cell-Cell Adhesion Via Plasma-Membrane Adhesion Molecules (GO:0098742) | 3.24e−03 | 29.8 |
| GO Biological Process 2023 | Nervous System Development (GO:0007399) | 4.98e−03 | 20.9 |
| GO Biological Process 2023 | Positive Regulation Of Cell Projection Organization (GO:0031346) | 1.95e−02 | 25.3 |
| GO Biological Process 2023 | Neuron Projection Morphogenesis (GO:0048812) | 1.95e−02 | 22.9 |
| GO Biological Process 2023 | Epithelial Cell Migration (GO:0010631) | 1.95e−02 | 38.4 |
| GO Biological Process 2023 | Positive Regulation Of Neuron Projection Development (GO:0010976) | 3.64e−02 | 24.0 |
| GO Biological Process 2023 | Cell Morphogenesis Involved In Neuron Differentiation (GO:0048667) | 4.75e−02 | 24.3 |
| KEGG 2019 Mouse | cAMP signaling pathway | 1.02e−04 | 31.6 |
| KEGG 2019 Mouse | Axon guidance | 2.39e−03 | 22.0 |
| KEGG 2019 Mouse | Hedgehog signaling pathway | 2.78e−03 | 36.9 |
| KEGG 2019 Mouse | Regulation of lipolysis in adipocytes | 2.78e−03 | 31.9 |
| KEGG 2019 Mouse | cGMP-PKG signaling pathway | 3.09e−03 | 19.0 |
| KEGG 2019 Mouse | Rap1 signaling pathway | 3.09e−03 | 17.6 |
| KEGG 2019 Mouse | Renin secretion | 3.79e−02 | 15.6 |
| KEGG 2019 Mouse | Gastric acid secretion | 4.69e−02 | 14.7 |
| KEGG 2019 Mouse | Neurotrophin signaling pathway | 4.84e−02 | 12.0 |
| KEGG 2019 Mouse | Adrenergic signaling in cardiomyocytes | 4.84e−02 | 11.0 |
| Reactome 2022 | Signal Transduction R-HSA-162582 | 2.78e−02 | 13.3 |
| Reactome 2022 | Neuronal System R-HSA-112316 | 2.78e−02 | 16.5 |
| MSigDB Hallmark 2020 | UV Response Dn | 6.14e−03 | 17.8 |

## **11\. Functional Categories from Prior Analysis**

**Table 17\. Data-driven functional category summary**

| Category | Gene count | Representative genes |
| :---- | :---- | :---- |
| Synaptic/Neurotransm. | 150 | Iqsec2, Il1rapl1, Slitrk2, Robo1, Snca, Trpc5, Fmr1, Plxna2, Rab39b, Trpc6, Pak3, Rgma, En1, Slitrk1, Agrn, Pou4f1, Grk5, Nrp1, Slitrk6, Dvl1, Trib2, Zc4h2, Cdh8, Lrrc4c, Srgap1, Zdhhc2, Epha7, Itgb1, Cacna2d1, Cdh2, Met, Sema6d, Pard6b, Nfatc2, Efna5, Pard3, Nrcam, Dscam, Slitrk3, App, Fgf14, Cacnb4, Efnb1, Rock2, Unc5d, Grik2, Ptprs, Sema3d, Rps6ka3, Zdhhc7, Adcy1, Camk2b, Dbnl, Kcna1, Gnai1, Ntng1, Slit3, Epha6, Cbln3, Cast, Ptch1, Sema4f, Plxna4, Nrxn1, Musk, Lrrc4b, Gpr37, Gpm6a, Lrrc4, Kirrel3, Prkcz, Unc13c, Nfatc3, Camk2d, Plxnb3, Myl12b, Kcnh3, Ntn4, Tnc, Epha3, Adgrl3, Cfl1, Sdk2, Pip5k1c, Lrp4, Rims1, Pik3ca, Cacnb3, Rnd1, Unc13b, Rest, Camk2a, Cd247, Ptprd, Nlgn1, Efnb2, Pdzrn3, Shh, Add2, Mbp, Trpc3, Pax6, Sema5b, Myl9, Ky, Creb1, Gnai3, Pard6g, Ephb1, Pcdhb6, Mapt, Lrrtm3, Wasf1, Spock2, Apbb2, Plcg1, Slc8a3, Epha2, Pik3r1, Limk1, Unc5a, Ank3, Dlg4, Pde4b, Ppp3ca, Dnm3, Syn2, Cacna2d2, Sdk1, F2r, Glra1, Ptk2, Dlg2, Ntng2, Ablim2, Sema4c, Plxnd1, Epha8, Rhoa, Cdc42, Appl1, Arhgef12, Chrm2, Ctnnd2 |
| Signaling Cascades | 78 | Atp1b4, Calml3, Gpr119, Gria3, Vav1, Cacna1f, Adcyap1r1, Pomc, Sstr1, Adcy3, Creb5, Ednra, Adcyap1, Gli3, Cftr, Rock2, Oxt, Adcy1, Camk2b, Gcg, Gnai1, Gabbr2, Ptch1, Fshr, Fxyd1, Ryr2, Mapk10, Camk2d, Grin3a, Calm1, Ghsr, Sox9, Atp2b1, Map2k1, Edn2, Rapgef4, Sstr2, Pik3ca, Akt1, Mc2r, Camk2a, Pld1, Pde3a, Vav3, Gria2, Adcy5, Pde4d, Afdn, Nfatc1, Acox3, Grin2b, Myl9, Fshb, Creb1, Adrb1, Gnai3, Mapk8, Atp1b2, Ptger3, Prkaca, Akt3, Hcn4, Pik3r1, Glp1r, Pde3b, Mapk9, Grin2a, Pde4b, Atp2b2, Nfkb1, F2r, Sst, Rhoa, Tshb, Chrm2, Hcar2, Fxyd2, Atp2b4 |
| Neuroplasticity/Trophic | 70 | Iqsec2, Maged1, Calml3, Bcl2, Traf6, Tanc1, Irs1, Map3k5, Lrrtm1, Gpr88, Ldlr, Ids, Ptprz1, Rps6ka3, Zdhhc7, Camk2b, Map3k1, Ntf3, Disc1, Mapk10, Sos1, Camk2d, Calm1, Kcnh3, Ywhaz, Map2k1, Pik3ca, Gab1, Akt1, Sos2, Map2k5, Camk2a, Ngfr, Arl6ip5, Ripk2, Brinp1, Foxb1, Spred1, Adra2c, Kidins220, Hivep2, Lrrc7, Otc, Sh2b2, Arhgdib, Sort1, Pik3r4, Itm2b, Mapk8, Mapt, Plcg1, Akt3, Stx1a, Sorcs3, Pik3r1, Mapk9, Grin2a, Foxo3, Rps7, Dlg4, Pde4b, Nfkb1, Irak2, Sh2b1, Crkl, Rapgef1, Rhoa, Ngf, Cdc42, Aldh2 |

## **12\. Prior GSEA Results**

**Table 18\. Prior preranked GSEA significant sets**

| Library | Term | NES | FDR |
| :---- | :---- | :---- | :---- |
| MSigDB Hallmark 2020 | Xenobiotic Metabolism | −1.65 | 1.37e−01 |
| MSigDB Hallmark 2020 | Interferon Alpha Response | −1.46 | 1.46e−01 |
| KEGG 2019 Mouse | Staphylococcus aureus infection | −1.90 | 1.65e−01 |

## **13\. Ortholog Mapping**

**Table 19\. Mouse-to-human ortholog mapping**

| Metric | Value |
| :---- | :---- |
| Ortholog source | homologene |
| One-to-one orthologs mapped | 0 / 5112 high-confidence genes |
| Mouse GMT | DOI\_7D\_HC\_mouse.gmt |
| Human GMT | DOI\_7D\_HC\_human.gmt |
| Symbol handling | Mouse symbols were not uppercased into fake human genes. |

## **14\. RNA-seq Validation**

**Table 20\. RNA-seq validation status**

| Analysis | Status |
| :---- | :---- |
| RNA-seq validation, 7d versus vehicle | RNA integration not run; RNA\_means absent. |
| RNA/7d transcription concordance | Not a Stage 3 analysis; RNA\_means.tsv.gz was never written by the pipeline. |
| Required additional input | Separate count matrix. |

## **15\. Caveats and Interpretation**

**Table 21\. Analytical caveats**

| Item | Statement |
| :---- | :---- |
| ABC score repair | The stored ABC score exceeded 1, so a true within-gene ABC fraction was rebuilt from AxC. |
| Prior analysis replacement | The ABC-score leaderboard and the 4,362-gene ORA from the prior pipeline are replaced by the Stage 3 analysis and were not reused as Stage 3 inputs. |
| Mixed-direction genes | Mixed-direction genes were dropped from UP/DOWN analyses. |
| Negative NES | Negative NES identifies the negative end of the signed enhancer-weighted ranking and does not itself establish DOI suppression of a pathway. |
| Direction claims | Negative-NES and MIXED-direction genes carry no directional DOI claim. |
| RNA validation | RNA/7d transcription concordance requires a separate count matrix. |

## **16\. Output Files**

**Table 22\. Stage 3 output files**

| Output category | File | Reported size |
| :---- | :---- | :---- |
| Ortholog GMT | /content/ABC\_DOI/STAGE3/DOI\_7D\_HC\_human.gmt | 0 KB |
| Ortholog GMT | /content/ABC\_DOI/STAGE3/DOI\_7D\_HC\_mouse.gmt | 64 KB |
| Figure | /content/ABC\_DOI/STAGE3/figures/architecture.png | 147 KB |
| Figure | /content/ABC\_DOI/STAGE3/figures/string\_hubs.png | 66 KB |
| Gene-level table | /content/ABC\_DOI/STAGE3/gene\_level.tsv | 3025 KB |
| GSEA table | /content/ABC\_DOI/STAGE3/gsea\_GO\_Biological\_Process\_2023.tsv | 273 KB |
| GSEA GMT | /content/ABC\_DOI/STAGE3/gsea\_GO\_Biological\_Process\_2023/gene\_sets.gmt | 247 KB |
| GSEA report | /content/ABC\_DOI/STAGE3/gsea\_GO\_Biological\_Process\_2023/gseapy.gene\_set.prerank.report.csv | 260 KB |
| GSEA rank file | /content/ABC\_DOI/STAGE3/gsea\_GO\_Biological\_Process\_2023/prerank\_data.rnk | 130 KB |
| GSEA table | /content/ABC\_DOI/STAGE3/gsea\_KEGG\_2019\_Mouse.tsv | 55 KB |
| GSEA GMT | /content/ABC\_DOI/STAGE3/gsea\_KEGG\_2019\_Mouse/gene\_sets.gmt | 52 KB |
| GSEA report | /content/ABC\_DOI/STAGE3/gsea\_KEGG\_2019\_Mouse/gseapy.gene\_set.prerank.report.csv | 53 KB |
| GSEA rank file | /content/ABC\_DOI/STAGE3/gsea\_KEGG\_2019\_Mouse/prerank\_data.rnk | 130 KB |
| GSEA table | /content/ABC\_DOI/STAGE3/gsea\_MSigDB\_Hallmark\_2020.tsv | 11 KB |
| GSEA GMT | /content/ABC\_DOI/STAGE3/gsea\_MSigDB\_Hallmark\_2020/gene\_sets.gmt | 12 KB |
| GSEA report | /content/ABC\_DOI/STAGE3/gsea\_MSigDB\_Hallmark\_2020/gseapy.gene\_set.prerank.report.csv | 11 KB |
| GSEA rank file | /content/ABC\_DOI/STAGE3/gsea\_MSigDB\_Hallmark\_2020/prerank\_data.rnk | 130 KB |
| Gene table | /content/ABC\_DOI/STAGE3/high\_confidence\_genes.tsv | 713 KB |
| Gene table | /content/ABC\_DOI/STAGE3/mixed\_direction\_genes.tsv | 1863 KB |
| Ortholog table | /content/ABC\_DOI/STAGE3/mouse\_human\_orthologs.tsv | 0 KB |
| ORA table | /content/ABC\_DOI/STAGE3/ora\_HC\_DOWN.tsv.gz | 305 KB |
| ORA table | /content/ABC\_DOI/STAGE3/ora\_HC\_UP.tsv.gz | 569 KB |
| STRING hubs | /content/ABC\_DOI/STAGE3/string\_hubs.tsv | 1 KB |
| STRING network | /content/ABC\_DOI/STAGE3/string\_network.tsv | 2 KB |
| Synaptic terms | /content/ABC\_DOI/STAGE3/synaptic\_terms\_in\_HC\_UP.tsv | 1 KB |

**Table 23\. Prior downstream output files**

| Output category | File | Reported size |
| :---- | :---- | :---- |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/ALL\_libraries.tsv.gz | 770 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/GO\_Biological\_Process\_2023.tsv | 1186 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/GO\_Cellular\_Component\_2023.tsv | 125 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/GO\_Molecular\_Function\_2023.tsv | 244 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/KEGG\_2019\_Mouse.tsv | 83 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/MGI\_Mammalian\_Phenotype\_Level\_4\_2021.tsv | 1098 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/MSigDB\_Hallmark\_2020.tsv | 16 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/Reactome\_2022.tsv | 413 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/TRRUST\_Transcription\_Factors\_2019.tsv | 103 KB |
| Enrichr ALL | DOWNSTREAM/enrichr/ALL/WikiPathways\_2019\_Mouse.tsv | 41 KB |
| Enrichr DOWN | DOWNSTREAM/enrichr/DOWN/ALL\_libraries.tsv.gz | 334 KB |
| Enrichr UP | DOWNSTREAM/enrichr/UP/ALL\_libraries.tsv.gz | 517 KB |
| Figure | DOWNSTREAM/figures/combined\_dotplot.png | 164 KB |
| Figure | DOWNSTREAM/figures/functional\_categories.png | 34 KB |
| Figure | DOWNSTREAM/figures/string\_hubs.png | 57 KB |
| Functional categories | DOWNSTREAM/functional\_categories.tsv | 14 KB |
| GSEA rank file | DOWNSTREAM/gsea\_prerank/GO\_Biological\_Process\_2023/prerank\_data.rnk | 107 KB |
| GSEA rank file | DOWNSTREAM/gsea\_prerank/KEGG\_2019\_Mouse/prerank\_data.rnk | 107 KB |
| GSEA rank file | DOWNSTREAM/gsea\_prerank/MSigDB\_Hallmark\_2020/prerank\_data.rnk | 107 KB |
| STRING hubs | DOWNSTREAM/string\_hubs.tsv | 0 KB |
| STRING network | DOWNSTREAM/string\_network.tsv | 1 KB |

