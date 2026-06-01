# OMIM / Orphanet / DECIPHER 详细介绍

## 1. 为什么要同时了解这三个资源

在罕见病与遗传病诊断中，`OMIM`、`Orphanet`、`DECIPHER` 经常被联合使用，但它们的定位并不相同：OMIM 更偏向“基因-表型知识条目”，Orphanet 更偏向“罕见病标准命名与多维资源目录”，DECIPHER 更偏向“患者级基因型-表型共享与匹配”。把三者放在同一工作流中，通常能显著提升变异解释与疑难病例求解效率。

## 2. OMIM（Online Mendelian Inheritance in Man）

### 2.1 平台定位

OMIM 是由 Johns Hopkins University 维护的权威遗传病知识库，核心目标是系统整理“人类基因与遗传表型之间的关系”。官方说明其为一个全面且权威的汇编，覆盖已知孟德尔遗传病及大量基因条目，并保持高频更新。

### 2.2 主要数据内容

- 基因条目与疾病条目（MIM 编号体系）
- 基因-表型关联叙述（带文献引用）
- Clinical Synopsis（临床表型摘要）
- Gene Map（基因定位与关联信息）

官方页面强调其全文条目为“文献支撑的综述式内容”，适合临床遗传医生、研究者和高年级医学/生命科学学习者。

### 2.3 更新与质量特点

- 官网说明：数据库 **daily update**
- API 页面说明：数据 **nightly update**

这意味着 OMIM 具有较强的时效性，尤其适合在新近文献出现后进行二次核查。

### 2.4 访问方式与授权

- Web 前端可公开检索
- 提供 REST API（支持 JSON / XML）
- API 需申请 key，且使用条款明确了研究/教育用途边界
- 商业或再分发场景需额外授权许可

OMIM 使用协议还要求：若下载数据并用于软件/数据库，应按协议要求定期刷新并正确署名。

### 2.5 典型应用场景

- 已知候选基因的致病机制与表型谱快速回顾
- ACMG 变异解读前的证据聚合
- 撰写病例报告时的疾病命名与历史脉络核对

## 3. Orphanet

### 3.1 平台定位

Orphanet 是罕见病领域的综合知识与服务平台，目标是提升罕见病在诊断、照护、研究与政策中的可见性。其核心贡献之一是维护 `ORPHAcode` 命名体系，推动跨系统语义互操作。

### 3.2 平台核心模块

- 罕见病目录、分类与百科
- 孤儿药目录
- 专家中心、实验室、临床试验、患者组织等目录
- 报告体系（Orphanet Reports Series）
- 数据平台（Orphadata）
- 本体资源（ORDO, Orphanet Rare Disease Ontology）

### 3.3 ORPHAcode 的意义

ORPHAcode 是面向罕见病的标准化、可计算编码体系。每个临床实体对应唯一标识符，并配有术语、同义词与定义。其价值主要体现在：

- 提高罕见病在电子病历、登记系统中的可追踪性
- 促进与 ICD、SNOMED CT、OMIM 等体系的映射互通
- 支撑跨国家/跨机构的数据整合与统计

### 3.4 数据规模与网络协作（官网可见信息）

Orphanet 首页展示了当前规模指标（例如疾病数、相关基因数、专家中心数、诊断检测数等），并说明其为多国协作网络（覆盖 40 余国家）。这使其在“疾病知识 + 资源可达性 + 编码标准化”三方面同时具备实用价值。

### 3.5 授权与开放性

Orphadata 页面显示，部分数据包采用 `CC BY 4.0`，并提供 API/定制化数据服务入口。对科研、公共卫生、临床信息系统建设都较友好，但具体数据产品仍需按各自条款使用。

### 3.6 典型应用场景

- 罕见病编码落地（医院信息系统、登记系统、患者摘要）
- 医疗资源导航（专家中心、检测实验室、患者组织）
- 人群层面的流行病学与政策支持分析

## 4. DECIPHER（DatabasE of genomiC varIation and Phenotype in Humans using Ensembl Resources）

### 4.1 平台定位

DECIPHER 是面向临床基因组学社区的数据共享与解释平台，强调“患者级”基因型-表型联合信息的共享、比较和匹配，以促进罕见病诊断与新致病机制发现。

### 4.2 核心能力

- 共享去标识化患者数据（表型 + 变异）
- 按基因、坐标、HPO 术语等进行检索
- 提供浏览与匹配机制，帮助发现相似病例
- 支持项目/联盟粒度的数据共享策略

### 4.3 官网披露的关键规模信息

DECIPHER 首页显示：

- 可浏览公开数据（无需登录即可探索）
- 已有超过 52,000 例获得广泛共享同意的患者数据
- 自 2004 年以来支持了 4,000+ 发表文章

这些指标反映了其在真实世界罕见病病例共享网络中的实践成熟度。

### 4.4 典型应用场景

- 疑难病例的“相似表型 + 相似变异”匹配
- 新候选基因/新综合征线索发现
- 多中心协作中的病例证据累积与再解释

## 5. 三者对比（诊断实践视角）

| 维度 | OMIM | Orphanet | DECIPHER |
|---|---|---|---|
| 主要定位 | 基因-表型知识库 | 罕见病命名/资源/数据生态 | 患者级基因型-表型共享平台 |
| 数据颗粒度 | 条目级（基因/疾病综述） | 疾病与卫生系统资源级 | 患者与变异级 |
| 强项 | 文献整合深、知识权威 | 编码标准化与互操作强 | 病例匹配与诊断协作强 |
| 典型输入 | 基因名、疾病名、MIM 号 | ORPHAcode、疾病名、资源类型 | HPO、基因、坐标、变异 |
| 最常见输出 | 疾病机制与表型谱证据 | 标准编码与转诊/登记信息 | 相似病例与协作线索 |
| 使用注意点 | 授权条款较严格（尤其商业） | 数据产品条款按包区分 | 患者共享需符合伦理与同意框架 |

## 6. 如何在一个病例里联合使用

建议流程如下：

1. 先用 OMIM 明确候选基因与已知表型谱，建立“机制假设”；
2. 再用 DECIPHER 检索相似基因型-表型病例，判断是否有外部病例支持；
3. 最后用 Orphanet/ORPHAcode 做疾病编码与系统落地（登记、统计、跨机构共享）。

这种“知识证据（OMIM）+ 病例证据（DECIPHER）+ 标准编码（Orphanet）”的组合，对缩短罕见病诊断路径通常最有效。

## 7. 每个资源的数据长相样例（帮助快速建立直觉）

下面给的是“结构化样例”，用于说明三个平台的典型数据形态。为避免患者隐私与授权问题，DECIPHER 采用去标识化示意；OMIM 与 Orphanet 采用其公开字段风格进行结构展示。

### 7.1 OMIM 样例：疾病/基因条目（Entry）通常长什么样

最常见的是“一个条目 + 多个章节 + 文献证据”的结构，接近下面这种 JSON 组织：

```json
{
  "mimNumber": 600185,
  "title": "Parkinson Disease 1",
  "entryType": "Phenotype",
  "status": "live",
  "geneSymbols": ["SNCA"],
  "chromosomeLocation": "4q22.1",
  "textSections": [
    {"sectionTitle": "Description", "content": "..."},
    {"sectionTitle": "Clinical Features", "content": "..."},
    {"sectionTitle": "Molecular Genetics", "content": "..."}
  ],
  "clinicalSynopsis": {
    "inheritance": "Autosomal dominant",
    "neurologic": ["Bradykinesia", "Resting tremor", "Rigidity"]
  },
  "references": [
    {"pubmedId": 12345678, "authors": "Smith et al.", "year": 2021}
  ],
  "lastUpdated": "2026-04-17"
}
```

你可以把 OMIM 理解为“文献驱动的知识条目库”：重点不是单条原始样本，而是经过整理后的基因-疾病知识结构。

### 7.2 Orphanet 样例：ORPHAcode 记录与映射数据长什么样

Orphanet 的核心是“标准术语 + 编码 + 映射 + 注释”。典型记录常见如下字段：

```json
{
  "orphaCode": "ORPHA:58",
  "preferredTerm": "Alexander disease",
  "synonyms": ["Fibrinoid leukodystrophy"],
  "diseaseGroup": "Neurologic disease",
  "classificationLevel": "Disorder",
  "definition": "A rare leukodystrophy characterized by ...",
  "crossReferences": {
    "OMIM": ["203450"],
    "ICD10": ["E75.2"],
    "UMLS": ["C0000000"]
  },
  "geneAssociations": ["GFAP"],
  "hpoTerms": ["HP:0001250", "HP:0001263"],
  "epidemiology": {
    "prevalenceClass": "Very rare",
    "source": "Orphanet report"
  },
  "nomenclatureRelease": "2025-07"
}
```

如果你在做 HIS/登记系统建设，最关键就是 `ORPHAcode + 映射字段 + 版本号` 这三部分。

### 7.3 DECIPHER 样例：患者级基因型-表型记录长什么样

DECIPHER 的核心对象是“去标识化患者记录”，每条记录会绑定表型和变异信息，常见结构如下：

```json
{
  "decipherPatientId": "DECIPHER:400001",
  "sharingLevel": "open_access",
  "sex": "female",
  "ageGroup": "child",
  "phenotypesHPO": [
    {"id": "HP:0000252", "label": "Microcephaly"},
    {"id": "HP:0001263", "label": "Developmental delay"}
  ],
  "variants": [
    {
      "genomeBuild": "GRCh38",
      "chromosome": "17",
      "start": 43044295,
      "end": 43045912,
      "variantType": "deletion",
      "zygosity": "heterozygous",
      "genes": ["BRCA1"],
      "classification": "likely_pathogenic"
    }
  ],
  "inheritance": "de_novo",
  "matchmakingStatus": "enabled",
  "contactClinicianAvailable": true
}
```

你可以把 DECIPHER 理解为“病例证据层”：重点是一个患者具体有哪些 HPO 表型、什么变异、与谁相似、是否可进一步协作。

### 7.4 三种数据在实际工作中的最小对应关系

| 目标 | OMIM 给你什么 | Orphanet 给你什么 | DECIPHER 给你什么 |
|---|---|---|---|
| 病因知识 | 基因-疾病关系与文献证据 | 疾病定义与标准命名 | 相似病例中的候选基因/变异 |
| 标准编码 | MIM 编号 | ORPHAcode + ICD/SNOMED 等映射 | 患者 ID 与 HPO/变异坐标 |
| 临床落地 | 机制和综述证据 | 编码、转诊与资源目录 | 病例匹配与协作线索 |

## 8. 参考来源（联网检索）

- OMIM About: <https://data.omim.org/help/about>
- OMIM API Access: <https://www.omim.org/api>
- OMIM Use Agreement: <https://www.omim.org/help/agreement>
- Orphanet 首页: <https://www.orpha.net/>
- Orphanet About: <https://www.orpha.net/en/other-information/about_orphanet>
- ORPHAcodes: <https://www.orphacode.org/>
- Orphadata: <https://www.orphadata.com/>
- DECIPHER 首页: <https://www.deciphergenomics.org/>
