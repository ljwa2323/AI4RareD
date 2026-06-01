# 罕见病 AI 诊断：公共数据资源全景（含示例数据与下载链接）

> 本文面向罕见病初筛 / 差异诊断 / Agentic RAG 系统设计，按**诊断推理所需数据类型**整理公共资源。  
> 每个资源均给出：**用途**、**示例数据**、**下载/访问链接**、**访问限制**。  
> 信息来源以官方站点为准（检索时间：2026-06）；落地前请核对最新版本与许可条款。

---

## 0. 资源分类总览


| 类别                 | 代表资源                                         | 典型用途                |
| ------------------ | -------------------------------------------- | ------------------- |
| A. 病例与评测 benchmark | RareBench, RareArena, PMC-Patients, MIMIC-IV | 模型训练、评测、相似病例检索      |
| B. 表型与疾病本体         | HPO, Orphanet, OMIM, MONDO, UMLS             | 表型标准化、疾病归一化、知识图谱    |
| C. 基因变异与遗传证据       | ClinVar, gnomAD, G2P, Exomiser, ClinGen      | VCF 注释、致病变异过滤、基因优先级 |
| D. 医学知识与文献         | PubMed, GeneReviews, NORD, MedlinePlus       | RAG 语料、证据链、鉴别诊断     |
| E. 跨库连接层           | UMLS CUI, MONDO xref, Orphadata Product1     | 多源 ID 对齐、跨库检索       |


**主线论文数据源**：DeepRare（[arXiv:2506.20430](https://arxiv.org/pdf/2506.20430)）、Deep-DxSearch（[arXiv:2508.15746](https://arxiv.org/pdf/2508.15746)）、RareBench（[arXiv:2402.06341](https://arxiv.org/pdf/2402.06341)）。

---

## A. 病例与评测 Benchmark

### A1. RareBench

**用途**：罕见病 LLM 四维评测 benchmark（表型抽取、筛查、鉴别诊断等）；含 RAMEDIS / MME / HMS / LIRICAL 四个公共子集。

**示例数据**（Hugging Face 官方 schema）：

```json
{
  "Phenotype": ["HP:0001250", "HP:0002093", "HP:0001270"],
  "RareDisease": ["OMIM:219700", "ORPHA:586"],
  "Department": "Pediatrics"
}
```


| 子集        | 病例数      | 疾病数 | 国家/地区 |
| --------- | -------- | --- | ----- |
| RAMEDIS   | 624      | 74  | 欧洲    |
| MME       | 40       | 17  | 加拿大   |
| HMS       | 88       | 39  | 德国    |
| LIRICAL   | 370      | 252 | 多国    |
| PUMCH_ADM | 75（公开部分） | 16  | 中国    |


**下载/访问**：


| 类型               | 链接                                                                                                   |
| ---------------- | ---------------------------------------------------------------------------------------------------- |
| Hugging Face 数据集 | [https://huggingface.co/datasets/chenxz/RareBench](https://huggingface.co/datasets/chenxz/RareBench) |
| GitHub 代码与映射文件   | [https://github.com/chenxz1111/RareBench](https://github.com/chenxz1111/RareBench)                   |
| 加载示例             | `load_dataset('chenxz/RareBench', 'RAMEDIS', split='test')`                                          |
| 映射文件             | `phenotype_mapping.json`, `disease_mapping.json`（GitHub `mapping/` 目录）                               |


**访问限制**：CC 开放；PUMCH 子集仅公开 75 例。

---

### A2. RareArena

**用途**：近 5 万例、4000+ 罕见病的诊断数据集；基于 PMC-Patients 构建，用于 LLM 罕见病诊断评测（DeepRare / Deep-DxSearch 均使用）。

**示例数据**（`RDC.json` 概念化，Rare Disease Classification）：

```json
{
  "patient_id": "PMC1234567_p0",
  "patient_summary": "A 3-year-old boy presented with recurrent respiratory infections, failure to thrive, and elevated sweat chloride...",
  "disease_label": "Cystic fibrosis",
  "orpha_code": "586",
  "source_pmcid": "PMC1234567"
}
```

**下载/访问**：


| 类型                                    | 链接                                                                                                                                                       |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hugging Face（主文件 RDC.json / RDS.json） | [https://huggingface.co/datasets/THUMedInfo/RareArena](https://huggingface.co/datasets/THUMedInfo/RareArena)                                             |
| 直接下载 RDC.json                         | [https://huggingface.co/datasets/THUMedInfo/RareArena/resolve/main/RDC.json](https://huggingface.co/datasets/THUMedInfo/RareArena/resolve/main/RDC.json) |
| 直接下载 RDS.json                         | [https://huggingface.co/datasets/THUMedInfo/RareArena/resolve/main/RDS.json](https://huggingface.co/datasets/THUMedInfo/RareArena/resolve/main/RDS.json) |
| GitHub 复现脚本                           | [https://github.com/zhao-zy15/RareArena](https://github.com/zhao-zy15/RareArena)                                                                         |


**访问限制**：CC BY-NC-SA 4.0。

---

### A3. PMC-Patients

**用途**：167k 患者摘要（来自 PMC 病例报告）、3.1M 患者-文章相关标注、293k 患者相似性标注；病例检索 / RAG / 相似患者匹配的基础库。

**示例数据**：

```json
{
  "patient_uid": "12345678-0",
  "patient": "A 45-year-old woman with progressive muscle weakness and elevated CK levels was diagnosed with...",
  "pmcid": "PMC7890123",
  "relevant_articles": ["PMC7890123", "PMC3456789"]
}
```

**下载/访问**：


| 类型           | 链接                                                                                                             |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| Figshare 合集  | [https://figshare.com/collections/PMC-Patients/6723465](https://figshare.com/collections/PMC-Patients/6723465) |
| Hugging Face | [https://huggingface.co/zhengyun21](https://huggingface.co/zhengyun21)                                         |
| GitHub       | [https://github.com/zhao-zy15/PMC-Patients](https://github.com/zhao-zy15/PMC-Patients)                         |
| Leaderboard  | [https://pmc-patients.github.io/](https://pmc-patients.github.io/)                                             |


**访问限制**：无需 DUA，直接下载；解压后将 `datasets/` 与 `meta_data/` 置于项目根目录。

---

### A4. MIMIC-IV / MIMIC-IV-Note / MIMIC-RD

**用途**：真实世界 EHR；DeepRare 构建 MIMIC-IV-Rare，Deep-DxSearch 构建 MIMIC-R / MIMIC-C 用于罕见病/常见病对照。

**示例数据**（`diagnoses_icd` 表，概念化）：


| subject_id | hadm_id  | icd_code | icd_version | seq_num |
| ---------- | -------- | -------- | ----------- | ------- |
| 10000032   | 22595853 | E84.9    | 10          | 1       |
| 10000032   | 22595853 | J44.9    | 10          | 2       |


**示例数据**（MIMIC-IV-Note 出院小结片段）：

```
DISCHARGE DIAGNOSIS:
Primary: Cystic fibrosis (E84.9)
Secondary: Chronic obstructive pulmonary disease (J44.9)
...
```

**下载/访问**：


| 类型                       | 链接                                                                                                                                                 |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| MIMIC-IV v3.0（PhysioNet） | [https://www.physionet.org/content/mimiciv/3.0/](https://www.physionet.org/content/mimiciv/3.0/)                                                   |
| MIMIC-IV-Note            | [https://physionet.org/content/mimic-iv-note/](https://physionet.org/content/mimic-iv-note/)                                                       |
| MIMIC 官网文档               | [https://mimic.mit.edu/](https://mimic.mit.edu/)                                                                                                   |
| GCP 云存储桶                 | [https://console.cloud.google.com/storage/browser/mimic-iv.physionet.org](https://console.cloud.google.com/storage/browser/mimic-iv.physionet.org) |
| 认证申请                     | [https://physionet.org/settings/credentialing/](https://physionet.org/settings/credentialing/)                                                     |
| CITI 培训                  | [https://physionet.org/about/citi-course/](https://physionet.org/about/citi-course/)                                                               |


**访问限制**：需 PhysioNet 认证用户 + CITI 培训 + 签署 DUA；**禁止团队内共享账号**。

---

### A5. MyGene2

**用途**：患者/家庭共享的罕见遗传病表型与基因信息；表型+基因型联合诊断评测（DeepRare、SHEPHERD 等使用）。

**示例数据**（网页展示结构，概念化）：

```json
{
  "family_id": "F1234",
  "proband_sex": "female",
  "age_at_onset": "infancy",
  "phenotypes": ["Global developmental delay", "Seizures"],
  "candidate_genes": ["SCN2A"],
  "inheritance": "de novo",
  "diagnosis": "Developmental and epileptic encephalopathy"
}
```

**下载/访问**：


| 类型    | 链接                                                                       |
| ----- | ------------------------------------------------------------------------ |
| 门户首页  | [https://mygene2.org/MyGene2/](https://mygene2.org/MyGene2/)             |
| 搜索与浏览 | [https://mygene2.org/MyGene2/search](https://mygene2.org/MyGene2/search) |


**访问限制**：**无公开批量下载 API**；需在网页逐例浏览，或联系数据贡献者/项目组获取研究级访问。

---

### A6. DDD（Deciphering Developmental Disorders）

**用途**：英国/爱尔兰儿童发育障碍研究；表型（HPO）+ 基因型（外显子/WES）+ 家系三联体；DeepRare 从 Gene2Phenotype 获取 2,283 例。

**示例数据**（DECIPHER 公开 track，概念化）：

```json
{
  "decipher_id": "DDD123456",
  "hpo_terms": ["HP:0001249", "HP:0001250"],
  "candidate_variant": "CFTR c.1521_1523delCTT",
  "inheritance": "compound heterozygous",
  "gene": "CFTR"
}
```

**下载/访问**：


| 类型                | 链接                                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------- |
| DDD 项目页           | [https://www.ddduk.org/accessing-ddd-data/](https://www.ddduk.org/accessing-ddd-data/)                     |
| EGA 研究（受控访问）      | [https://www.ega-archive.org/studies/EGAS00001000775](https://www.ega-archive.org/studies/EGAS00001000775) |
| EGA DAC 申请        | [https://www.ega-archive.org/dacs/EGAC00001000282](https://www.ega-archive.org/dacs/EGAC00001000282)       |
| DECIPHER DDD 公开数据 | [https://www.deciphergenomics.org/ddd/](https://www.deciphergenomics.org/ddd/)                             |
| 关联论文              | Nature PMID:25533962                                                                                       |


**访问限制**：完整 BAM/VCF/临床数据需 **EGA 正式申请 + DAC 审批**；DECIPHER 公开 track 仅含摘要级候选变异。

---

### A7. 其他 RareBench 来源子集


| 资源                            | 用途               | 示例                    | 下载链接                                                                                                                                                           |
| ----------------------------- | ---------------- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **RAMEDIS**                   | 欧洲罕见病病例          | 74 种病、624 例 HPO 表型    | [https://agbi.techfak.uni-bielefeld.de/ramedis/](https://agbi.techfak.uni-bielefeld.de/ramedis/) ; 整理版见 RareBench                                              |
| **Matchmaker Exchange (MME)** | 基因-表型跨中心匹配       | 基因 `SCN2A` + HPO 表型提交 | [https://github.com/ga4gh/mme-apis](https://github.com/ga4gh/mme-apis) ; API: [https://search.matchmakerexchange.org/](https://search.matchmakerexchange.org/) |
| **HMS**                       | 汉诺威医学院临床罕见病      | 39 种病、88 例            | 整理版见 RareBench                                                                                                                                                 |
| **LIRICAL**                   | 表型驱动诊断 benchmark | HPO 列表 -> 疾病排序        | [https://github.com/TheJacksonLaboratory/LIRICAL](https://github.com/TheJacksonLaboratory/LIRICAL)                                                             |


---

## B. 表型与疾病本体 / 知识库

### B1. HPO（Human Phenotype Ontology）

**用途**：表型标准化核心；18,000+ 表型术语；与 OMIM / Orphanet 疾病注释联动。

**示例数据**（OBO 格式）：

```
[Term]
id: HP:0002093
name: Respiratory insufficiency
synonym: "Respiratory impairment" EXACT []
is_a: HP:0002793 ! Abnormal respiratory system physiology
```

**示例数据**（`phenotype.hpoa` 疾病-表型关联）：

```
ORPHA:586  Cystic fibrosis  HP:0002093  Respiratory insufficiency
ORPHA:586  Cystic fibrosis  HP:0012236  Elevated sweat chloride
ORPHA:586  Cystic fibrosis  HP:0002027  Abdominal pain
```

**下载/访问**：


| 文件                     | 链接                                                                                                                                                   |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| hp.obo（推荐）             | [http://purl.obolibrary.org/obo/hp.obo](http://purl.obolibrary.org/obo/hp.obo)                                                                       |
| hp.json                | [http://purl.obolibrary.org/obo/hp.json](http://purl.obolibrary.org/obo/hp.json)                                                                     |
| phenotype.hpoa（疾病-表型）  | [https://github.com/obophenotype/human-phenotype-ontology/releases/latest](https://github.com/obophenotype/human-phenotype-ontology/releases/latest) |
| genes_to_phenotype.txt | 同上 Releases 页面                                                                                                                                       |
| phenotype_to_genes.txt | 同上 Releases 页面                                                                                                                                       |
| GitHub Releases        | [https://github.com/obophenotype/human-phenotype-ontology/releases](https://github.com/obophenotype/human-phenotype-ontology/releases)               |
| HPO 官网                 | [https://hpo.jax.org/](https://hpo.jax.org/)                                                                                                         |


**访问限制**：开放下载；引用 HPO 官方论文。

---

### B2. Orphanet / ORPHA（Orphadata）

**用途**：罕见病标准命名、ORPHA 编码、表型关联、基因关联、流行病学；CC BY 4.0。

**示例数据**（Product4 表型关联 XML 概念化）：

```xml
<Disorder id="586">
  <OrphaCode>586</OrphaCode>
  <Name lang="en">Cystic fibrosis</Name>
  <HPODisorderAssociation>
    <HPO id="HP:0002093">
      <HPOTerm lang="en">Respiratory insufficiency</HPOTerm>
      <Frequency>Very frequent (99-80%)</Frequency>
    </HPO>
  </HPODisorderAssociation>
</Disorder>
```

**示例数据**（Product1 跨库映射）：


| ORPHA | 名称              | 外部 ID                                     |
| ----- | --------------- | ----------------------------------------- |
| 586   | Cystic fibrosis | OMIM:219700, ICD-10:E84, SNOMED:190905008 |


**下载/访问**：


| 产品           | 内容                            | 下载页                                                                                                |
| ------------ | ----------------------------- | -------------------------------------------------------------------------------------------------- |
| Product 1    | 罕见病与外部术语对齐                    | [https://www.orphadata.com/_alignments/](https://www.orphadata.com/_alignments/)                   |
| Product 4    | 罕见病-HPO 表型关联（en_product4.xml） | [https://www.orphadata.com/_phenotypes/](https://www.orphadata.com/_phenotypes/)                   |
| Product 6    | 罕见病-基因关联（en_product6.xml）     | [https://sciences.orphadata.com/genes/](https://sciences.orphadata.com/genes/)                     |
| Product 9    | 自然史/发病年龄                      | [https://www.orphadata.com/_natural-history/](https://www.orphadata.com/_natural-history/)         |
| 分类 Product 3 | 按专科分类                         | [https://sciences.orphadata.com/classifications/](https://sciences.orphadata.com/classifications/) |
| API（需申请）     | REST 查询                       | [https://api.orphadata.com/](https://api.orphadata.com/)                                           |
| 历史版本         | GitHub                        | [https://github.com/Orphanet/API_Orphadata](https://github.com/Orphanet/API_Orphadata)             |


**访问限制**：XML 免费开放（CC BY 4.0）；API 需联系 Orphadata 申请。

---

### B3. OMIM

**用途**：孟德尔遗传病基因-表型知识；MIM 号是遗传病研究常用锚点。

**示例数据**（`mim2gene.txt`，**唯一免注册文件**）：

```
# Mim_number	MIM_entry_type	Entrez_Gene_ID	Approved_Gene_Symbol	Ensembl_Gene_ID
219700	phenotype	1080	CFTR	ENSG00000001626
301500	phenotype	2717	GLA	ENSG00000130234
```

**示例数据**（API JSON 片段，entry/219700）：

```json
{
  "omim": {
    "mimNumber": 219700,
    "entry": {
      "titles": { "preferredTitle": "CYSTIC FIBROSIS; CF" },
      "geneMap": { "geneSymbols": "CFTR" }
    }
  }
}
```

**下载/访问**：


| 类型                | 链接                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------ |
| mim2gene.txt（免注册） | [https://omim.org/static/omim/data/mim2gene.txt](https://omim.org/static/omim/data/mim2gene.txt) |
| 完整数据下载申请          | [https://www.omim.org/downloads/](https://www.omim.org/downloads/)                               |
| API 申请            | [https://www.omim.org/api](https://www.omim.org/api)                                             |
| API 文档            | [https://www.omim.org/help/api](https://www.omim.org/help/api)                                   |
| API 示例            | `https://api.omim.org/api/entry?mimNumber=219700&format=json&apiKey=YOUR_KEY`                    |


**可下载文件**（注册后）：`mimTitles.txt`, `genemap2.txt`, `morbidmap.txt` 等， nightly 更新。

**访问限制**：需**机构邮箱**注册；商用/再分发需 JHU 许可。

---

### B4. MONDO

**用途**：跨源疾病本体整合层；精确标注 OMIM / Orphanet / ICD / DOID 等等价关系。

**示例数据**（OBO 格式）：

```
[Term]
id: MONDO:0009061
name: cystic fibrosis
xref: OMIM:219700 {source="MONDO:equivalentTo"}
xref: Orphanet:586 {source="MONDO:equivalentTo"}
xref: DOID:1485 {source="MONDO:equivalentTo"}
xref: UMLS:C0010674 {source="MONDO:equivalentTo"}
```

**下载/访问**：


| 文件         | 链接                                                                                                                         |
| ---------- | -------------------------------------------------------------------------------------------------------------------------- |
| mondo.obo  | [http://purl.obolibrary.org/obo/mondo.obo](http://purl.obolibrary.org/obo/mondo.obo)                                       |
| mondo.json | [http://purl.obolibrary.org/obo/mondo.json](http://purl.obolibrary.org/obo/mondo.json)                                     |
| mondo.owl  | [http://purl.obolibrary.org/obo/mondo.owl](http://purl.obolibrary.org/obo/mondo.owl)                                       |
| 最新 Release | [https://github.com/monarch-initiative/mondo/releases/latest](https://github.com/monarch-initiative/mondo/releases/latest) |
| 官网         | [https://mondo.monarchinitiative.org/](https://mondo.monarchinitiative.org/)                                               |


**访问限制**：开放（CC BY 4.0）。

---

### B5. UMLS（Metathesaurus）

**用途**：元术语系统；通过 CUI 连接 ICD / SNOMED / MeSH / OMIM / RxNorm 等。

**示例数据**（MRCONSO 概念化，CUI `C0010674` = Cystic fibrosis）：


| SAB         | CODE      | STR                          |
| ----------- | --------- | ---------------------------- |
| SNOMEDCT_US | 190905008 | Cystic fibrosis (disorder)   |
| ICD10CM     | E84.9     | Cystic fibrosis, unspecified |
| MSH         | D003550   | Cystic Fibrosis              |
| OMIM        | 219700    | CYSTIC FIBROSIS              |
| ORPHANET    | 586       | Cystic fibrosis              |


**下载/访问**：


| 类型                    | 链接                                                                                                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| UMLS 许可注册             | [https://www.nlm.nih.gov/research/umls/licensedcontent/umlsknowledgesources.html](https://www.nlm.nih.gov/research/umls/licensedcontent/umlsknowledgesources.html) |
| 2026AA Full Release   | `umls-2026AA-full.zip`（约 5.3 GB 压缩）                                                                                                                                |
| UTS API               | [https://documentation.uts.nlm.nih.gov/](https://documentation.uts.nlm.nih.gov/)                                                                                   |
| Metathesaurus Browser | [https://uts.nlm.nih.gov/uts/metathesaurus/home](https://uts.nlm.nih.gov/uts/metathesaurus/home)                                                                   |


**访问限制**：需免费 UMLS 许可账号；部分源词表有额外限制。

---

### B6. Gene2Phenotype（G2P / DDG2P）

**用途**：基因-疾病关联 curated 证据；含 Developmental Disorders（DDG2P）等面板；DeepRare / DDD 分析使用。

**示例数据**（G2P CSV 概念化）：


| gene symbol | disease name                               | allelic requirement   | mutation consequence | confidence |
| ----------- | ------------------------------------------ | --------------------- | -------------------- | ---------- |
| CFTR        | Cystic fibrosis                            | biallelic_autosomal   | loss of function     | confirmed  |
| SCN2A       | Developmental and epileptic encephalopathy | monoallelic_autosomal | missense / LoF       | confirmed  |


**下载/访问**：


| 类型        | 链接                                                                                                                                             |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 下载页       | [https://www.ebi.ac.uk/gene2phenotype/download](https://www.ebi.ac.uk/gene2phenotype/download)                                                 |
| API 按面板下载 | `https://www.ebi.ac.uk/gene2phenotype/api/panel/DD/download`                                                                                   |
| 面板名       | Cancer, Cardiac, DD, Ear, Eye, Skeletal, Skin, All                                                                                             |
| FTP 根目录   | [http://ftp.ebi.ac.uk/pub/databases/gene2phenotype/](http://ftp.ebi.ac.uk/pub/databases/gene2phenotype/)                                       |
| G2P 数据目录  | [http://ftp.ebi.ac.uk/pub/databases/gene2phenotype/G2P_data_downloads/](http://ftp.ebi.ac.uk/pub/databases/gene2phenotype/G2P_data_downloads/) |


**访问限制**：开放下载。

---

### B7. DECIPHER（deciphergenomics.org）

**用途**：患者级基因型-表型共享与匹配；DDD 候选变异公开 track；**注意**：与 R 包 DECIPHER（decipher.codes）是不同项目。

**示例数据**（公开 patient 概念化）：

```json
{
  "decipher_id": "328045",
  "chromosome": "7",
  "start": 117480025,
  "end": 117668665,
  "variant_type": "DEL",
  "hpo_terms": ["HP:0001250", "HP:0001263"],
  "consent": "granted for broad sharing"
}
```

**下载/访问**：


| 类型     | 链接                                                                                                             |
| ------ | -------------------------------------------------------------------------------------------------------------- |
| 门户首页   | [https://www.deciphergenomics.org/](https://www.deciphergenomics.org/)                                         |
| 数据下载页  | [https://www.deciphergenomics.org/about/downloads/data](https://www.deciphergenomics.org/about/downloads/data) |
| DDD 专区 | [https://www.deciphergenomics.org/ddd/](https://www.deciphergenomics.org/ddd/)                                 |
| 基因组浏览器 | [https://www.deciphergenomics.org/browser](https://www.deciphergenomics.org/browser)                           |
| 表型浏览器  | [https://www.deciphergenomics.org/phenogram](https://www.deciphergenomics.org/phenogram)                       |


**访问限制**：公开 browse 无需登录；完整 patient-level 数据需注册 DECIPHER 项目账号。

---

### B8. ClinGen

**用途**：基因-疾病有效性 curated 分级（Definitive / Strong / Moderate 等）。

**示例数据**：


| gene | disease (MONDO)                 | classification | MOI |
| ---- | ------------------------------- | -------------- | --- |
| CFTR | cystic fibrosis (MONDO:0009061) | Definitive     | AR  |
| GLA  | Fabry disease (MONDO:0010526)   | Definitive     | XL  |


**下载/访问**：


| 类型                    | 链接                                                                                               |
| --------------------- | ------------------------------------------------------------------------------------------------ |
| 下载/API 页              | [https://search.clinicalgenome.org/kb/downloads](https://search.clinicalgenome.org/kb/downloads) |
| Gene-Disease Validity | 实时 TSV，按基因(MONDO)分组                                                                              |
| Dosage Sensitivity    | GRCh37/GRCh38 TSV                                                                                |
| 搜索门户                  | [https://search.clinicalgenome.org/](https://search.clinicalgenome.org/)                         |


**访问限制**：开放下载。

---

### B9. MeSH

**用途**：PubMed 文献主题词；疾病文献检索锚点。

**示例数据**：


| MeSH ID | 主题词             | 树状号                 |
| ------- | --------------- | ------------------- |
| D003550 | Cystic Fibrosis | C06.452.829.100     |
| D005776 | Gaucher Disease | C16.320.565.925.249 |


**下载/访问**：


| 类型              | 链接                                                                                                           |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| MeSH 首页         | [https://www.nlm.nih.gov/mesh/meshhome.html](https://www.nlm.nih.gov/mesh/meshhome.html)                     |
| MeSH Browser    | [https://meshb.nlm.nih.gov/](https://meshb.nlm.nih.gov/)                                                     |
| MeSH RDF/SPARQL | [https://id.nlm.nih.gov/mesh/](https://id.nlm.nih.gov/mesh/)                                                 |
| ASCII 下载        | [https://www.nlm.nih.gov/databases/download/mesh.html](https://www.nlm.nih.gov/databases/download/mesh.html) |


---

## C. 基因变异与遗传证据

### C1. ClinVar

**用途**：临床相关变异解释与致病性断言。

**示例数据**（`variant_summary.txt` 概念化）：


| VariationID | GeneSymbol | ClinicalSignificance         | PhenotypeList   | ReviewStatus                           |
| ----------- | ---------- | ---------------------------- | --------------- | -------------------------------------- |
| 1552        | CFTR       | Pathogenic                   | Cystic fibrosis | criteria provided, multiple submitters |
| 12345       | GLA        | Pathogenic/Likely pathogenic | Fabry disease   | reviewed by expert panel               |


**下载/访问**：


| 文件                     | 链接                                                                                                                                                                 |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| variant_summary.txt.gz | [https://ftp.ncbi.nlm.nih.gov/pub/clinvar/tab_delimited/variant_summary.txt.gz](https://ftp.ncbi.nlm.nih.gov/pub/clinvar/tab_delimited/variant_summary.txt.gz)     |
| 完整 XML（按变异 VCV）        | [https://ftp.ncbi.nlm.nih.gov/pub/clinvar/xml/ClinVarVCVRelease_00-latest.xml.gz](https://ftp.ncbi.nlm.nih.gov/pub/clinvar/xml/ClinVarVCVRelease_00-latest.xml.gz) |
| FTP 根目录                | [https://ftp.ncbi.nlm.nih.gov/pub/clinvar/](https://ftp.ncbi.nlm.nih.gov/pub/clinvar/)                                                                             |
| 下载说明                   | [https://www.ncbi.nlm.nih.gov/clinvar/docs/downloads/](https://www.ncbi.nlm.nih.gov/clinvar/docs/downloads/)                                                       |


**访问限制**：开放；每月第一个周四归档完整版。

---

### C2. gnomAD

**用途**：人群变异频率过滤；Exomiser / DeepRare 基因分析核心频率库。

**示例数据**（VCF INFO 概念化）：

```
#CHROM  POS     ID      REF  ALT  AF      AC      AN
7       117559590  .    G    A    0.000004  6    1500000
```

**下载/访问**：


| 类型                           | 链接                                                                                         |
| ---------------------------- | ------------------------------------------------------------------------------------------ |
| 官方下载页（v4.1）                  | [https://gnomad.broadinstitute.org/downloads](https://gnomad.broadinstitute.org/downloads) |
| Google Cloud Public Datasets | gnomAD v4 Hail 表                                                                           |
| AWS Open Data Registry       | 同上                                                                                         |
| 预处理 SQLite（第三方）              | [https://zenodo.org/records/11077663](https://zenodo.org/records/11077663) （WGS v4.1）      |


**访问限制**：开放；完整 VCF 体量大（TB 级），建议 Hail 或按需 subset。

---

### C3. dbSNP

**用途**：变异 rs ID 与基础注释。

**示例数据**：


| rs ID       | 基因   | 功能                 |
| ----------- | ---- | ------------------ |
| rs113993960 | CFTR | F508del（囊性纤维化常见突变） |
| rs28935493  | GLA  | Fabry 病相关          |


**下载/访问**：


| 类型         | 链接                                                                                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------ |
| FTP        | [https://ftp.ncbi.nlm.nih.gov/snp/](https://ftp.ncbi.nlm.nih.gov/snp/)                                       |
| VCF（human） | [https://ftp.ncbi.nlm.nih.gov/snp/latest_release/VCF/](https://ftp.ncbi.nlm.nih.gov/snp/latest_release/VCF/) |


---

### C4. 1000 Genomes / TOPMed / UK10K / ESP

**用途**：补充人群频率参考；Exomiser 内置整合。


| 资源                            | 下载链接                                                                                   |
| ----------------------------- | -------------------------------------------------------------------------------------- |
| 1000 Genomes Phase 3          | [https://www.internationalgenome.org/data/](https://www.internationalgenome.org/data/) |
| TOPMed                        | [https://topmed.nhlbi.nih.gov/](https://topmed.nhlbi.nih.gov/) （受控访问）                  |
| UK10K                         | [https://www.uk10k.org/](https://www.uk10k.org/) （受控访问）                                |
| ESP（Exome Sequencing Project） | 已整合入 gnomAD；历史数据见 Exomiser 包                                                           |


---

### C5. Exomiser（工具 + 数据包）

**用途**：表型 + 变异联合优先级排序；DeepRare 基因型分析核心。

**示例输入**（表型 HPO + VCF）：

```yaml
analysis:
  genomeAssembly: hg38
  hpoIds: ["HP:0002093", "HP:0012236"]
  vcf: sample.vcf.gz
```

**示例输出**（基因优先级）：


| gene  | exomiser_score | disease         | hpo_match |
| ----- | -------------- | --------------- | --------- |
| CFTR  | 0.98           | Cystic fibrosis | 12/14     |
| SCN2A | 0.45           | DEE             | 5/14      |


**下载/访问**：


| 类型          | 链接                                                                                                                         |
| ----------- | -------------------------------------------------------------------------------------------------------------------------- |
| 程序 Release  | [https://github.com/exomiser/Exomiser/releases](https://github.com/exomiser/Exomiser/releases)                             |
| 数据包（latest） | [http://data.monarchinitiative.org/exomiser/latest](http://data.monarchinitiative.org/exomiser/latest)                     |
| 表型数据        | `2512_phenotype.zip`                                                                                                       |
| hg38 数据     | `2512_hg38.zip`                                                                                                            |
| hg19 数据     | `2512_hg19.zip`                                                                                                            |
| 文档          | [https://exomiser.readthedocs.io/en/latest/installation.html](https://exomiser.readthedocs.io/en/latest/installation.html) |


**访问限制**：开放；数据包体积大（数十 GB），需与 `application.properties` 中版本号一致。

---

## D. 医学知识与文献

### D1. PubMed / PubMed Central

**用途**：医学文献检索；DeepRare / Deep-DxSearch RAG 核心语料。

**示例数据**（E-utilities 返回概念化）：

```json
{
  "uid": "38123456",
  "title": "Cystic fibrosis: current understanding and future directions",
  "source": "Lancet",
  "pubdate": "2024 Jan",
  "mesh": ["Cystic Fibrosis", "CFTR protein, human"]
}
```

**下载/访问**：


| 类型              | 链接                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------ |
| PubMed          | [https://pubmed.ncbi.nlm.nih.gov/](https://pubmed.ncbi.nlm.nih.gov/)                             |
| E-utilities API | [https://eutils.ncbi.nlm.nih.gov/entrez/eutils/](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/) |
| PMC 开放全文 FTP    | [https://ftp.ncbi.nlm.nih.gov/pub/pmc/](https://ftp.ncbi.nlm.nih.gov/pub/pmc/)                   |
| Bulk OA 子集      | [https://www.ncbi.nlm.nih.gov/pmc/tools/ftp/](https://www.ncbi.nlm.nih.gov/pmc/tools/ftp/)       |


**API 示例**：`https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=cystic+fibrosis[rare+disease]`

---

### D2. GeneReviews

**用途**：遗传病诊断、鉴别诊断、管理与遗传咨询；高质量 curated 章节（800+）。

**示例数据**（`GRtitle_shortname_NBKid.txt`）：


| GR_shortname | GR_title        | NBK_id  | PMID     |
| ------------ | --------------- | ------- | -------- |
| cf           | Cystic Fibrosis | NBK1250 | 20301428 |
| fabry        | Fabry Disease   | NBK1292 | 20301353 |


**下载/访问**：


| 类型                | 链接                                                                                                 |
| ----------------- | -------------------------------------------------------------------------------------------------- |
| GeneReviews FTP   | [ftp://ftp.ncbi.nih.gov/pub/GeneReviews/](ftp://ftp.ncbi.nih.gov/pub/GeneReviews/)                 |
| 链接构造说明            | [https://www.ncbi.nlm.nih.gov/books/NBK138605/](https://www.ncbi.nlm.nih.gov/books/NBK138605/)     |
| 按 OMIM 查章节        | `NBKid_shortname_OMIM.txt`（同 FTP 目录）                                                               |
| Bookshelf OA 批量下载 | [https://ftp.ncbi.nlm.nih.gov/pub/litarch/ca/84/](https://ftp.ncbi.nlm.nih.gov/pub/litarch/ca/84/) |


---

### D3. 其他 DeepRare / Deep-DxSearch 使用的 Web 知识源


| 资源                 | 用途                    | 入口                                                                                               |
| ------------------ | --------------------- | ------------------------------------------------------------------------------------------------ |
| MedlinePlus        | 患者/临床可靠解释             | [https://medlineplus.gov/](https://medlineplus.gov/)                                             |
| NORD               | 美国罕见病组织与疾病说明          | [https://rarediseases.org/](https://rarediseases.org/)                                           |
| MSD Manuals        | 临床疾病知识                | [https://www.msdmanuals.com/](https://www.msdmanuals.com/)                                       |
| Mayo Clinic        | 疾病症状与诊疗               | [https://www.mayoclinic.org/diseases-conditions](https://www.mayoclinic.org/diseases-conditions) |
| NCBI Bookshelf     | 医学教材 / GeneReviews 宿主 | [https://www.ncbi.nlm.nih.gov/books/](https://www.ncbi.nlm.nih.gov/books/)                       |
| Wikipedia（医学页）     | 大规模背景知识（证据等级低）        | 需自行爬取或 API                                                                                       |
| ClinicalTrials.gov | 试验与新疗法线索              | [https://clinicaltrials.gov/](https://clinicaltrials.gov/)                                       |
| ICD10Data          | ICD-10-CM 编码查询        | [https://www.icd10data.com/](https://www.icd10data.com/)                                         |


---

## E. 跨资源连接与整合层

### E1. 同一疾病的多 ID 对照示例（Cystic fibrosis）


| 体系        | 编码/ID                  |
| --------- | ---------------------- |
| ORPHA     | 586                    |
| OMIM      | 219700                 |
| MONDO     | MONDO:0009061          |
| UMLS CUI  | C0010674               |
| ICD-10-CM | E84.9                  |
| SNOMED CT | 190905008              |
| MeSH      | D003550                |
| 关联基因      | CFTR (GeneID: 1080)    |
| 典型 HPO    | HP:0002093, HP:0012236 |


### E2. 推荐连接路径

```
病例表型(HPO) -> Phen2Gene/Exomiser -> 候选基因
     |                                    |
     v                                    v
Orphanet/OMIM 知识              ClinVar/gnomAD 过滤
     |                                    |
     +-------- UMLS CUI / MONDO ----------+
                      |
                      v
              文献证据(PubMed/GeneReviews)
```

---

## F. 儿童罕见病初筛：三层数据收集策略

### 第一层：阳性病例（罕见病）


| 优先级 | 资源                    | 下载入口                                                                               |
| --- | --------------------- | ---------------------------------------------------------------------------------- |
| 最高  | RareBench / RareArena | 见 A1、A2                                                                            |
| 最高  | PMC-Patients（罕见病子集）   | 见 A3                                                                               |
| 高   | MyGene2 / DDD         | 见 A5、A6                                                                            |
| 高   | MIMIC-IV-Rare         | 见 A4（需自行 ICD 筛选）                                                                   |
| 中   | Orphanet 表型注释         | [https://www.orphadata.com/_phenotypes/](https://www.orphadata.com/_phenotypes/)   |
| 中   | GeneReviews 病例描述      | [ftp://ftp.ncbi.nih.gov/pub/GeneReviews/](ftp://ftp.ncbi.nih.gov/pub/GeneReviews/) |


### 第二层：困难阴性对照（常见病 / 疑似罕见病但排除）


| 资源               | 用途        | 下载入口                                                                                             |
| ---------------- | --------- | ------------------------------------------------------------------------------------------------ |
| MIMIC-IV 常见病     | 真实 EHR 阴性 | [https://www.physionet.org/content/mimiciv/3.0/](https://www.physionet.org/content/mimiciv/3.0/) |
| MedDialog        | 常见病问诊     | 论文配套 / Hugging Face 检索                                                                           |
| PMC-Patients 常见病 | 病例报告阴性    | 见 A3                                                                                             |


### 第三层：证据链知识库


| 资源                      | 下载入口       |
| ----------------------- | ---------- |
| Orphanet + OMIM + HPO   | 见 B1-B3    |
| MONDO + UMLS            | 见 B4-B5    |
| G2P + ClinGen + ClinVar | 见 B6-B8、C1 |
| GeneReviews + PubMed    | 见 D1-D2    |


---

## G. 数据集综述总表


| 类别        | 资源            | 公开  | 数据类型     | 含儿童  | 含 HPO | 含基因 | 含 EHR | 适合任务     | 主下载入口                               |
| --------- | ------------- | --- | -------- | ---- | ----- | --- | ----- | -------- | ----------------------------------- |
| Benchmark | RareBench     | 是   | HPO/病例   | 部分   | 是     | 部分  | 否     | 罕见病诊断评测  | HF: chenxz/RareBench                |
| 病例库       | RareArena     | 是   | 病例摘要     | 部分   | 可映射   | 少量  | 否     | 训练/评估    | HF: THUMedInfo/RareArena            |
| 病例库       | PMC-Patients  | 是   | 患者摘要     | 部分   | 需抽取   | 通常无 | 否     | 相似病例/RAG | Figshare 6723465                    |
| EHR       | MIMIC-IV-Note | 需认证 | 出院小结     | 少    | 需抽取   | 否   | 是     | 真实世界验证   | PhysioNet                           |
| 家系共享      | MyGene2       | 是   | 表型+基因    | 多为儿童 | 部分    | 是   | 否     | 表型基因联合   | mygene2.org                         |
| 发育障碍      | DDD           | 部分  | 表型+基因    | 是    | 是     | 是   | 否     | 儿童罕见病    | EGA EGAS00001000775                 |
| 知识库       | Orphanet      | 是   | 疾病/表型/基因 | 是    | 是     | 是   | 否     | 罕见病知识    | orphadata.com                       |
| 知识库       | OMIM          | 部分  | 基因-疾病    | 是    | 可映射   | 是   | 否     | 遗传病证据    | omim.org/downloads                  |
| 表型本体      | HPO           | 是   | 表型术语     | 是    | 是     | 间接  | 否     | 表型标准化    | purl.obolibrary.org/obo/hp.obo      |
| 疾病本体      | MONDO         | 是   | 疾病整合     | 是    | 间接    | 间接  | 否     | 疾病标准化    | github.com/monarch-initiative/mondo |
| 变异库       | ClinVar       | 是   | 变异注释     | -    | -     | 是   | 否     | 致病性过滤    | ftp.ncbi.nlm.nih.gov/pub/clinvar/   |
| 频率库       | gnomAD        | 是   | 人群频率     | -    | -     | 是   | 否     | 变异过滤     | gnomad.broadinstitute.org/downloads |


---

## H. 优先落地清单（按工程顺序）

1. **表型底座**：HPO + Orphadata Product4 + Orphadata Product1
2. **疾病底座**：OMIM（mim2gene + API）+ MONDO + Orphanet
3. **评测集**：RareBench + RareArena
4. **病例检索**：PMC-Patients
5. **基因分析**：ClinVar + gnomAD + Exomiser 数据包 + G2P
6. **RAG 语料**：GeneReviews FTP + PubMed E-utilities
7. **真实世界验证**：MIMIC-IV（需提前 2-4 周申请 PhysioNet 认证）
8. **跨库对齐**：UMLS（可选，需许可注册）

---

## I. 许可与合规提醒


| 资源                      | 关键限制                    |
| ----------------------- | ----------------------- |
| OMIM                    | 机构邮箱；商用需许可；API key 年度续期 |
| MIMIC-IV                | 个人认证；禁止共享账号；禁止公开个体病例原文  |
| UMLS                    | 需注册许可；部分源词表禁止再分发        |
| gnomAD                  | 开放但禁止试图重新识别个体           |
| Orphadata / HPO / MONDO | CC BY 4.0，需引用           |
| RareArena               | CC BY-NC-SA 4.0，禁止商用    |
| DDD 完整数据                | EGA 受控访问，需 DAC 批准       |


---

## J. 参考文献

- DeepRare: [arXiv:2506.20430](https://arxiv.org/pdf/2506.20430)
- Deep-DxSearch: [arXiv:2508.15746](https://arxiv.org/pdf/2508.15746)
- RareBench: [arXiv:2402.06341](https://arxiv.org/pdf/2402.06341)
- RareArena: [GitHub zhao-zy15/RareArena](https://github.com/zhao-zy15/RareArena)
- PMC-Patients: [GitHub pmc-patients](https://github.com/pmc-patients/pmc-patients)
- Orphadata: [orphadata.com](https://www.orphadata.com/)
- HPO: [obophenotype/human-phenotype-ontology](https://github.com/obophenotype/human-phenotype-ontology)
- MONDO: [monarch-initiative/mondo](https://github.com/monarch-initiative/mondo)

