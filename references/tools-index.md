# Tools Index

Library of ~150 domain tool guides from k-dense. Load the specific tool's SKILL.md on demand when working with that tool/database.

Note: document-skills (docx/pdf/pptx/xlsx) live at `tools/document-skills/<name>/SKILL.md` and duplicate richer versions in `scripts/claude-scientific-writer/` — prefer the `references/office-documents.md` guide first.

## Chemistry & Drug Discovery

| Tool | Path | Purpose |
|---|---|---|
| chembl-database | tools/chembl-database/SKILL.md | ChEMBL bioactive molecules, bioactivity (IC50/Ki), SAR |
| pubchem-database | tools/pubchem-database/SKILL.md | 110M+ compounds: properties, similarity/substructure search |
| zinc-database | tools/zinc-database/SKILL.md | 230M+ purchasable compounds for virtual screening/docking |
| drugbank-database | tools/drugbank-database/SKILL.md | Drug properties, targets, interactions, pathways, ADMET |
| opentargets-database | tools/opentargets-database/SKILL.md | Target-disease associations and tractability for target ID |
| rdkit | tools/rdkit/SKILL.md | Cheminformatics core: SMILES, descriptors, fingerprints, 2D/3D |
| datamol | tools/datamol/SKILL.md | Simplified RDKit wrapper with sensible defaults |
| medchem | tools/medchem/SKILL.md | Drug-likeness (Lipinski/Veber), PAINS and structural alerts |
| molfeat | tools/molfeat/SKILL.md | 100+ molecular featurizers (ECFP, descriptors, ChemBERTa) |
| deepchem | tools/deepchem/SKILL.md | Molecular ML for ADMET/toxicity with pre-built datasets |
| torchdrug | tools/torchdrug/SKILL.md | PyTorch GNNs for molecules, proteins, knowledge graphs |
| pytdc | tools/pytdc/SKILL.md | Therapeutics Data Commons benchmarks (ADME, DTI, splits) |
| diffdock | tools/diffdock/SKILL.md | Diffusion-based protein-ligand pose prediction and screening |
| rowan | tools/rowan/SKILL.md | Cloud quantum chemistry: DFT, pKa, conformers, docking |

## Bioinformatics & Genomics

| Tool | Path | Purpose |
|---|---|---|
| biopython | tools/biopython/SKILL.md | Sequence/file parsing (FASTA/GenBank/PDB), NCBI access, BLAST automation |
| bioservices | tools/bioservices/SKILL.md | Unified Python interface to 40+ bioinformatics services |
| gget | tools/gget/SKILL.md | Fast CLI/Python lookups across 20+ databases (genes, BLAST, enrichment) |
| scikit-bio | tools/scikit-bio/SKILL.md | Diversity metrics, ordination, PERMANOVA, microbiome analysis |
| etetoolkit | tools/etetoolkit/SKILL.md | Phylogenetic trees: manipulation, taxonomy, visualization |
| pysam | tools/pysam/SKILL.md | SAM/BAM/CRAM/VCF/FASTA I/O and coverage for NGS pipelines |
| deeptools | tools/deeptools/SKILL.md | NGS QC, heatmaps and profiles (ChIP/RNA/ATAC-seq) |
| geniml | tools/geniml/SKILL.md | Genomic-interval ML: region embeddings, scATAC-seq |
| gtars | tools/gtars/SKILL.md | High-performance genomic interval/coverage analysis (Rust) |
| arboreto | tools/arboreto/SKILL.md | Gene regulatory network inference (GRNBoost2, GENIE3) |
| lamindb | tools/lamindb/SKILL.md | FAIR biological data management, ontologies, lineage |
| cobrapy | tools/cobrapy/SKILL.md | Constraint-based metabolic modeling (FBA, knockouts, SBML) |
| ensembl-database | tools/ensembl-database/SKILL.md | 250+ species genomes, variants, orthologs, VEP |
| ena-database | tools/ena-database/SKILL.md | European Nucleotide Archive: sequences, reads, assemblies |
| geo-database | tools/geo-database/SKILL.md | NCBI GEO expression datasets (GSE/GSM, microarray/RNA-seq) |
| gene-database | tools/gene-database/SKILL.md | NCBI Gene annotation: RefSeq, GO, locations, phenotypes |
| clinvar-database | tools/clinvar-database/SKILL.md | Variant clinical significance and pathogenicity |
| gwas-database | tools/gwas-database/SKILL.md | GWAS Catalog SNP-trait associations and summary stats |
| cosmic-database | tools/cosmic-database/SKILL.md | COSMIC cancer mutations, Census, mutational signatures |
| kegg-database | tools/kegg-database/SKILL.md | KEGG pathways, gene-pathway mapping, drug interactions |
| reactome-database | tools/reactome-database/SKILL.md | Reactome pathway analysis, enrichment, interactions |

## Protein & Structural Biology

| Tool | Path | Purpose |
|---|---|---|
| esm | tools/esm/SKILL.md | ESM protein language models: design, embeddings, inverse folding |
| alphafold-database | tools/alphafold-database/SKILL.md | 200M+ predicted structures with confidence (pLDDT/PAE) |
| pdb-database | tools/pdb-database/SKILL.md | RCSB 3D structure search and download |
| uniprot-database | tools/uniprot-database/SKILL.md | Protein search, FASTA retrieval, ID mapping |
| string-database | tools/string-database/SKILL.md | Protein-protein interaction networks and enrichment |
| brenda-database | tools/brenda-database/SKILL.md | Enzyme kinetics (Km/kcat), reactions, organism data |

## Single-Cell, Cytometry & Omics

| Tool | Path | Purpose |
|---|---|---|
| scanpy | tools/scanpy/SKILL.md | Standard scRNA-seq pipeline: QC, clustering, differential expression |
| anndata | tools/anndata/SKILL.md | .h5ad annotated-matrix format for single-cell data |
| scvi-tools | tools/scvi-tools/SKILL.md | Deep generative models: batch correction, multimodal integration |
| cellxgene-census | tools/cellxgene-census/SKILL.md | Query 61M+ curated single-cell atlas across tissues/diseases |
| pydeseq2 | tools/pydeseq2/SKILL.md | Bulk RNA-seq differential expression (Python DESeq2) |
| flowio | tools/flowio/SKILL.md | Parse flow-cytometry FCS files to arrays/DataFrames |
| pyopenms | tools/pyopenms/SKILL.md | LC-MS/MS proteomics: features, identification, quantification |
| matchms | tools/matchms/SKILL.md | Mass spectral similarity and metabolite identification |
| hmdb-database | tools/hmdb-database/SKILL.md | 220K+ metabolites: properties, spectra, biomarkers |
| metabolomics-workbench-database | tools/metabolomics-workbench-database/SKILL.md | 4,200+ metabolomics studies, RefMet, MS/NMR data |

## Medical Imaging, Pathology & Biosignals

| Tool | Path | Purpose |
|---|---|---|
| pydicom | tools/pydicom/SKILL.md | DICOM read/write, pixel extraction, anonymization |
| imaging-data-commons | tools/imaging-data-commons/SKILL.md | NCI public radiology/pathology imaging datasets |
| histolab | tools/histolab/SKILL.md | WSI tile extraction, tissue detection, stain normalization |
| pathml | tools/pathml/SKILL.md | Computational pathology: multiplexed IF, segmentation, ML |
| neurokit2 | tools/neurokit2/SKILL.md | Biosignal processing (ECG/EEG/EDA/PPG/EMG), HRV, ERP |
| neuropixels-analysis | tools/neuropixels-analysis/SKILL.md | Neuropixels ephys: spike sorting, curation, QC metrics |

## Clinical, Medical & Health

| Tool | Path | Purpose |
|---|---|---|
| clinicaltrials-database | tools/clinicaltrials-database/SKILL.md | ClinicalTrials.gov search by condition/drug/phase, NCT details |
| clinpgx-database | tools/clinpgx-database/SKILL.md | Pharmacogenomics gene-drug interactions, CPIC guidelines |
| fda-database | tools/fda-database/SKILL.md | openFDA drugs, devices, adverse events, recalls |
| pyhealth | tools/pyhealth/SKILL.md | Healthcare ML on EHR (MIMIC), predictions, medical coding |
| clinical-decision-support | tools/clinical-decision-support/SKILL.md | CDS documents: cohort analyses, treatment recommendations (GRADE) |
| clinical-reports | tools/clinical-reports/SKILL.md | Case/diagnostic/trial reports (CARE, ICH-E3, SOAP) |
| treatment-plans | tools/treatment-plans/SKILL.md | Concise 3-4 page specialty treatment plans (LaTeX/PDF) |
| iso-13485-certification | tools/iso-13485-certification/SKILL.md | Medical-device QMS documentation and gap analysis |

## Literature Search & Review

| Tool | Path | Purpose |
|---|---|---|
| pubmed-database | tools/pubmed-database/SKILL.md | Direct PubMed REST API: Boolean/MeSH queries, E-utilities |
| biorxiv-database | tools/biorxiv-database/SKILL.md | bioRxiv preprint search, metadata, PDF retrieval |
| openalex-database | tools/openalex-database/SKILL.md | 240M+ scholarly works: citations, bibliometrics, trends |
| literature-review | tools/literature-review/SKILL.md | Systematic reviews/meta-analyses with verified citations |
| citation-management | tools/citation-management/SKILL.md | Scholar/PubMed search, metadata validation, BibTeX |
| perplexity-search | tools/perplexity-search/SKILL.md | AI web search (Sonar models) with source citations |
| research-lookup | tools/research-lookup/SKILL.md | Research lookups via Perplexity, auto model selection |

## Documents & Writing

| Tool | Path | Purpose |
|---|---|---|
| scientific-writing | tools/scientific-writing/SKILL.md | IMRAD manuscripts in prose with citations, reporting guidelines |
| peer-review | tools/peer-review/SKILL.md | Structured manuscript/grant reviews (CONSORT/STROBE) |
| scholar-evaluation | tools/scholar-evaluation/SKILL.md | ScholarEval quantitative assessment of scholarly work |
| research-grants | tools/research-grants/SKILL.md | NSF/NIH/DOE/DARPA proposals with agency formatting |
| venue-templates | tools/venue-templates/SKILL.md | Journal/conference/grant LaTeX templates and guidelines |
| latex-posters | tools/latex-posters/SKILL.md | beamerposter/tikzposter research posters |
| pptx-posters | tools/pptx-posters/SKILL.md | HTML/CSS posters exportable to PDF/PPTX |
| scientific-slides | tools/scientific-slides/SKILL.md | Research talk decks (PowerPoint/Beamer) with templates |
| paper-2-web | tools/paper-2-web/SKILL.md | Convert papers to websites, videos, and posters |
| markitdown | tools/markitdown/SKILL.md | Convert PDF/Office/media files to Markdown |
| markdown-mermaid-writing | tools/markdown-mermaid-writing/SKILL.md | Markdown docs with embedded Mermaid diagrams, 24 diagram types |
| open-notebook | tools/open-notebook/SKILL.md | Self-hosted NotebookLM alternative for research notebooks |

## Figures & Visualization

| Tool | Path | Purpose |
|---|---|---|
| matplotlib | tools/matplotlib/SKILL.md | Low-level plotting with full customization |
| seaborn | tools/seaborn/SKILL.md | Statistical plots with pandas integration |
| plotly | tools/plotly/SKILL.md | Interactive charts and dashboards |
| scientific-visualization | tools/scientific-visualization/SKILL.md | Publication-ready multi-panel journal figures |
| scientific-schematics | tools/scientific-schematics/SKILL.md | AI scientific diagrams (pathways, flowcharts, architectures) |
| generate-image | tools/generate-image/SKILL.md | General-purpose AI image generation |
| infographics | tools/infographics/SKILL.md | AI infographics with iterative quality refinement |

## Data Processing & Scientific Computing

| Tool | Path | Purpose |
|---|---|---|
| dask | tools/dask/SKILL.md | Distributed, larger-than-RAM pandas/NumPy workflows |
| polars | tools/polars/SKILL.md | Fast in-memory DataFrames, lazy evaluation |
| vaex | tools/vaex/SKILL.md | Out-of-core DataFrames for billion-row datasets |
| zarr-python | tools/zarr-python/SKILL.md | Chunked N-D arrays for cloud storage, parallel I/O |
| geopandas | tools/geopandas/SKILL.md | Geospatial vector data: spatial joins, mapping, CRS |
| modal | tools/modal/SKILL.md | Serverless cloud containers with GPUs and autoscaling |
| matlab | tools/matlab/SKILL.md | MATLAB/Octave numerics and Python conversion |
| simpy | tools/simpy/SKILL.md | Discrete-event simulation (queues, resources, processes) |
| networkx | tools/networkx/SKILL.md | Graph algorithms, communities, network visualization |
| sympy | tools/sympy/SKILL.md | Symbolic math: algebra, calculus, code generation |
| pymoo | tools/pymoo/SKILL.md | Multi-objective optimization (NSGA-II, Pareto fronts) |

## Machine Learning, AI & Statistics

| Tool | Path | Purpose |
|---|---|---|
| scikit-learn | tools/scikit-learn/SKILL.md | Classical ML: models, tuning, preprocessing, pipelines |
| pytorch-lightning | tools/pytorch-lightning/SKILL.md | Structured multi-GPU/TPU PyTorch training, DDP/FSDP |
| transformers | tools/transformers/SKILL.md | Pre-trained transformer models for NLP/vision/multimodal |
| torch_geometric | tools/torch_geometric/SKILL.md | Graph neural networks (GCN, GAT, link prediction) |
| stable-baselines3 | tools/stable-baselines3/SKILL.md | Standard RL algorithms (PPO, SAC, DQN) |
| pufferlib | tools/pufferlib/SKILL.md | High-performance parallel RL and multi-agent training |
| aeon | tools/aeon/SKILL.md | Time-series ML: classification, forecasting, anomaly detection |
| timesfm-forecasting | tools/timesfm-forecasting/SKILL.md | Zero-shot forecasting with Google TimesFM foundation model |
| shap | tools/shap/SKILL.md | SHAP model explanations and plots for any black-box model |
| umap-learn | tools/umap-learn/SKILL.md | UMAP dimensionality reduction and embeddings |
| statsmodels | tools/statsmodels/SKILL.md | OLS/GLM/mixed/ARIMA models with rigorous inference |
| pymc | tools/pymc/SKILL.md | Bayesian hierarchical modeling, MCMC, variational inference |
| scikit-survival | tools/scikit-survival/SKILL.md | Survival analysis: Cox, RSF, concordance, competing risks |

## Physics, Astronomy & Quantum

| Tool | Path | Purpose |
|---|---|---|
| astropy | tools/astropy/SKILL.md | Astronomy: coordinates, units, FITS, cosmology, WCS |
| fluidsim | tools/fluidsim/SKILL.md | Pseudospectral CFD (Navier-Stokes, geophysical flows) |
| pymatgen | tools/pymatgen/SKILL.md | Materials science: crystals, phase diagrams, band structure |
| cirq | tools/cirq/SKILL.md | Google quantum hardware circuits and noise modeling |
| qiskit | tools/qiskit/SKILL.md | IBM quantum runtime, error mitigation, transpilation |
| pennylane | tools/pennylane/SKILL.md | Hardware-agnostic quantum ML with autodiff (VQE, QAOA) |
| qutip | tools/qutip/SKILL.md | Open quantum system dynamics (Lindblad, decoherence) |

## Web, Economic & Government Data

| Tool | Path | Purpose |
|---|---|---|
| alpha-vantage | tools/alpha-vantage/SKILL.md | Stocks, forex, crypto, commodities, 50+ technical indicators |
| fred-economic-data | tools/fred-economic-data/SKILL.md | 800K+ FRED macroeconomic time series |
| edgartools | tools/edgartools/SKILL.md | SEC EDGAR filings, XBRL, 13F, Form 4, 10-K/Q |
| hedgefundmonitor | tools/hedgefundmonitor/SKILL.md | OFR hedge fund data (Form PF, leverage, repo) |
| usfiscaldata | tools/usfiscaldata/SKILL.md | Treasury fiscal data: debt, spending, interest rates |
| datacommons-client | tools/datacommons-client/SKILL.md | Public statistics: demographics, economy, health, environment |
| uspto-database | tools/uspto-database/SKILL.md | Patent/trademark searches, PEDS, assignments, prior art |

## Lab Automation & Platform Integrations

| Tool | Path | Purpose |
|---|---|---|
| adaptyv | tools/adaptyv/SKILL.md | Cloud lab for protein testing and wet-lab validation |
| benchling-integration | tools/benchling-integration/SKILL.md | Benchling registry, ELN, workflows, Data Warehouse APIs |
| dnanexus-integration | tools/dnanexus-integration/SKILL.md | DNAnexus genomics apps/workflows (dxpy SDK) |
| labarchive-integration | tools/labarchive-integration/SKILL.md | ELN API: entries, attachments, backups |
| latchbio-integration | tools/latchbio-integration/SKILL.md | Latch serverless bioinformatics pipelines |
| omero-integration | tools/omero-integration/SKILL.md | OMERO microscopy data, ROIs, batch processing |
| opentrons-integration | tools/opentrons-integration/SKILL.md | Opentrons OT-2/Flex protocol API |
| pylabrobot | tools/pylabrobot/SKILL.md | Vendor-agnostic lab automation (Hamilton, Tecan, Opentrons) |
| protocolsio-integration | tools/protocolsio-integration/SKILL.md | protocols.io search, create, publish, workspaces |

## Research Workflow & Meta-Skills

| Tool | Path | Purpose |
|---|---|---|
| denario | tools/denario/SKILL.md | Multiagent pipeline: data analysis to LaTeX paper |
| hypogenic | tools/hypogenic/SKILL.md | LLM-driven hypothesis generation and testing on tabular data |
| hypothesis-generation | tools/hypothesis-generation/SKILL.md | Structured testable hypotheses from observations |
| scientific-brainstorming | tools/scientific-brainstorming/SKILL.md | Open-ended research ideation and gap finding |
| scientific-critical-thinking | tools/scientific-critical-thinking/SKILL.md | Evidence quality, bias detection, GRADE/Cochrane grading |
| statistical-analysis | tools/statistical-analysis/SKILL.md | Guided test selection, power analysis, APA reporting |
| exploratory-data-analysis | tools/exploratory-data-analysis/SKILL.md | Auto-detect 200+ file formats, generate EDA reports |
| get-available-resources | tools/get-available-resources/SKILL.md | Detect CPU/GPU/memory before compute-heavy jobs |
| market-research-reports | tools/market-research-reports/SKILL.md | 50+ page consulting-style reports (SWOT, Porter, TAM) |
| offer-k-dense-web | tools/offer-k-dense-web/SKILL.md | Session prompt recommending K-Dense Web for complex workflows |
