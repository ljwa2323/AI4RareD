# DeepRare: tools and reference links

本文档汇总 DeepRare 论文（*Nature* Methods 与各 agent server）及公开代码仓库中出现的**工具与知识源**及其**使用入口链接**。论文宣称集成 **40+** 项：其中不少是 Exomiser 内部数据库/预测器、或不同检索器对同一数据源的多次封装，并非 GitHub 上 40 个独立脚本。

This note lists tools and knowledge sources described in the DeepRare *Nature* paper (Methods / agent servers) and related public implementations. The paper states **more than 40** integrated tools and sources; many entries are **sub-components** of pipelines such as Exomiser or overlapping retrievers, not separate GitHub modules.

Primary references:

- Paper: [An agentic system for rare disease diagnosis with traceable reasoning](https://www.nature.com/articles/s41586-025-10097-9)
- arXiv: [2506.20430](https://arxiv.org/abs/2506.20430)
- Open-source workflow (subset of production): [MAGIC-AI4Med/DeepRare](https://github.com/MAGIC-AI4Med/DeepRare)

---

## 1. Central orchestration (paper)

| Item | Role | Link / notes |
|------|------|----------------|
| Central host LLM | Diagnosis orchestration, synthesis, self-reflection (e.g. DeepSeek-V3 in paper) | Provider-dependent (e.g. [DeepSeek](https://www.deepseek.com/), OpenAI, etc.) |
| Lightweight summarizer LLM | Summarize / filter retrieved documents in knowledge searcher (default GPT-4o-mini in paper) | [OpenAI platform](https://platform.openai.com/) |

---

## 2. Phenotype extraction and entity normalization

| Item | Role | Link / notes |
|------|------|----------------|
| LLM (two-step prompts) | Extract phenotype candidates from free text | Same as host / API provider |
| BioLORD | Map candidates to standard **HPO** terms (cosine similarity) | [ACL 2022 BioLORD](https://aclanthology.org/2022.findings-emnlp.104/); [BioLORD-2023 (PubMed)](https://pubmed.ncbi.nlm.nih.gov/38412333/); [Hugging Face BioLORD-2023](https://huggingface.co/FremyCompany/BioLORD-2023) |
| BioLORD (disease) | Map free-text disease names to **Orphanet / OMIM** | Same model family as above |
| Human Phenotype Ontology (HPO) | Standard phenotype vocabulary (offline retrieval in paper) | [HPO](https://hpo.jax.org/) |

---

## 3. Knowledge searcher: general web

| Item | Role | Link / notes |
|------|------|----------------|
| Bing | Web search (Selenium in paper) | [Bing](https://www.bing.com/) |
| Google | Web search (API in paper) | [Google](https://www.google.com/) |
| Google Cloud / Programmable Search | Typical programmatic Google access (if used in implementations) | [Programmable Search](https://developers.google.com/custom-search) |
| DuckDuckGo | Web search (API in paper) | [DuckDuckGo API](https://api.duckduckgo.com/) (instant answer API; implementations vary) |

---

## 4. Knowledge searcher: literature and metadata

| Item | Role | Link / notes |
|------|------|----------------|
| PubMed | Biomedical literature | [PubMed](https://pubmed.ncbi.nlm.nih.gov/) |
| LangChain PubMedRetriever | PubMed access wrapper (cited in paper) | [PubMedRetriever docs](https://python.langchain.com/api_reference/community/retrievers/langchain_community.retrievers.pubmed.PubMedRetriever.html) |
| Crossref | Scholarly metadata / DOI API | [Crossref](https://www.crossref.org/) / [API docs](https://api.crossref.org/swagger-ui/index.html) |
| Google Scholar | Broad academic search (third-tier “medical literature” in paper) | [Google Scholar](https://scholar.google.com/) |
| arXiv (optional in open repo) | Preprint search | [arXiv](https://arxiv.org/) |

---

## 5. Knowledge searcher: curated rare disease and general medical text

| Item | Role | Link / notes |
|------|------|----------------|
| Orphanet | Rare disease knowledge (offline KB in paper) | [Orphanet](https://www.orpha.net/) |
| OMIM | Genes and genetic disorders (offline KB in paper) | [OMIM](https://www.omim.org/) |
| Wikipedia | General encyclopedic context (LangChain retriever in paper) | [Wikipedia](https://www.wikipedia.org/) |
| LangChain WikipediaRetriever | Wikipedia access wrapper | [WikipediaRetriever docs](https://python.langchain.com/api_reference/community/retrievers/langchain_community.retrievers.wikipedia.WikipediaRetriever.html) |
| MedlinePlus | Consumer/clinical summaries (Selenium in paper) | [MedlinePlus](https://medlineplus.gov/) |

---

## 6. Phenotype analyser (disease prioritization APIs)

| Item | Role | Link / notes |
|------|------|----------------|
| PhenoBrain | HPO-based top disease suggestions (API) | [PhenoBrain Web API (GitHub)](https://github.com/xiaohaomao/timgroup_disease_diagnosis/tree/main/PhenoBrain_Web_API) |
| PubCaseFinder | HPO-based case matching from PubMed reports (API) | [PubCaseFinder API](https://pubcasefinder.dbcls.jp/api); [PubCaseFinder site](https://pubcasefinder.dbcls.jp/) |
| Zero-shot LLM | Additional top-k candidates from structured HPO | Same as central LLM provider |

---

## 7. Case searcher (similar patients)

| Item | Role | Link / notes |
|------|------|----------------|
| OpenAI text-embedding-3-small | Dense embedding for initial case retrieval (paper) | [OpenAI embeddings](https://platform.openai.com/docs/guides/embeddings) |
| MedCPT Cross-Encoder | Re-ranking query vs case HPO profiles | [MedCPT paper (Bioinformatics)](https://doi.org/10.1093/bioinformatics/btad651) |
| BM25 | Alternative lexical retrieval (mentioned for comparison) | Classic IR baseline (implementation-specific) |

---

## 8. Genotype analyser (Exomiser stack)

Exomiser is the main **genotype** tool in the paper; internal data sources and predictors include:

| Item | Role | Link / notes |
|------|------|----------------|
| Exomiser | VCF annotation, prioritization, phenotype-driven ranking | [Exomiser documentation](https://exomiser.readthedocs.io/) |
| gnomAD | Population allele frequencies | [gnomAD](https://gnomad.broadinstitute.org/) |
| 1000 Genomes | Population variation | [1000 Genomes](https://www.internationalgenome.org/) |
| TOPMed | Population frequencies | [TOPMed](https://www.nhlbiwgs.org/) |
| UK10K | UK population frequencies | Project pages via [EBI / literature](https://www.ebi.ac.uk/) |
| NHLBI Exome Sequencing Project (ESP) | Population frequencies | Legacy resource; see Exomiser data bundles |
| PolyPhen-2 | Missense pathogenicity | [PolyPhen-2](http://genetics.bwh.harvard.edu/pph2/) |
| SIFT | Missense tolerance | [SIFT](https://sift.bii.a-star.edu.sg/) |
| MutationTaster | Pathogenicity prediction | [MutationTaster](https://www.mutationtaster.org/) |
| OMIM | Gene-disease associations (within prioritization) | [OMIM](https://www.omim.org/) |
| HiPhive | Cross-species phenotype-driven prioritization (Exomiser module) | Described in [Exomiser / Monarch](https://monarchinitiative.org/) ecosystem |

Outputs may also surface **ClinVar** annotations and **ACMG**-style classifications as packaged by Exomiser:

| Item | Role | Link / notes |
|------|------|----------------|
| ClinVar | Clinical significance of variants | [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/) |
| ACMG / AMP guidelines | Variant interpretation framework (referenced in outputs) | [ACMG/AMP statement (example DOI)](https://doi.org/10.1038/gim.2015.30) |

---

## 9. Third-tier data and case banks (paper)

These support retrieval, benchmarks, or case libraries; URLs are project or download entry points.

| Item | Link / notes |
|------|----------------|
| RareBench / related benchmark discourse | See paper references; [RareArena GitHub](https://github.com/zhao-zy15/RareArena) cited in *Nature* HTML for PMC-Patients processing |
| MyGene2 | [MyGene2](https://mygene2.org/) |
| MIMIC-IV / Note | [PhysioNet MIMIC](https://physionet.org/content/mimiciv/) |
| PMC-Patients | Derived from PubMed case reports; see RareArena / paper SI |
| ICD-10 | [WHO ICD-10 browser](https://icd.who.int/browse10/2019/en) |

---

## 10. Open-source DeepRare repo: `tools/` modules (GitHub)

These mirror part of the stack; not equal to the full “40+” product deployment.

| Module (path) | Typical role | Source |
|---------------|--------------|--------|
| `tools/web_search.py` | Bing / Google / DuckDuckGo helpers | [web_search.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/web_search.py) |
| `tools/page_fetch.py` | Page fetch and summarize | [page_fetch.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/page_fetch.py) |
| `tools/hpo_search.py` | HPO-related search | [hpo_search.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/hpo_search.py) |
| `tools/search_pubmed.py` | PubMed | [search_pubmed.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/search_pubmed.py) |
| `tools/search_arxiv.py` | arXiv | [search_arxiv.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/search_arxiv.py) |
| `tools/search_wiki.py` | Wikipedia | [search_wiki.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/search_wiki.py) |
| `tools/omim_search.py` | OMIM | [omim_search.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/omim_search.py) |
| `tools/phenobrain_api.py` | PhenoBrain API | [phenobrain_api.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/phenobrain_api.py) |
| `tools/pubcase_finder.py` | PubCaseFinder | [pubcase_finder.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/pubcase_finder.py) |
| `tools/exomizer_inference.py` | Exomiser integration | [exomizer_inference.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/exomizer_inference.py) |
| `tools/exomizer_split.py` | Exomiser splitting utilities | [exomizer_split.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/exomizer_split.py) |
| `tools/llm_agent.py` | LLM check agents | [llm_agent.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/llm_agent.py) |
| `tools/uptodate_search.py` | UpToDate (commented out in `diagnosis.py` in public snapshot) | [uptodate_search.py](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/tools/uptodate_search.py) |

---

## 11. Deployment and data (README)

| Item | Link / notes |
|------|----------------|
| Public web demo (README) | [deeprare.cn](http://deeprare.cn) (also mentioned: raredx.cn/doctor in README variants) |
| Hugging Face dataset / database bundle | `Angelakeke/DeepRare` per [README](https://github.com/MAGIC-AI4Med/DeepRare/blob/main/README.md) |
| ChromeDriver (browser automation) | [Chrome for Testing](https://googlechromelabs.github.io/chrome-for-testing/) |

---

## 12. MCP (conceptual)

The paper cites MCP-inspired architecture for tool integration:

| Item | Link |
|------|------|
| Model Context Protocol | [MCP specification / Anthropic](https://modelcontextprotocol.io/) |

---

*Disclaimer: vendors may change endpoints, terms, and APIs. For clinical use, follow local policy and licensing (e.g. OMIM, UpToDate, commercial search APIs).*
