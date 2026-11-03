# 国际一手信源检索手册

> 标注 🔓 的为免密钥公开渠道，优先使用。

## 1. 美股公司公告 —— SEC EDGAR 🔓

**强制要求**：所有请求必须携带含联系信息的 User-Agent，否则封 IP：
```
User-Agent: YourName your.email@example.com
```

**三步法定位公司文件**
```
1. ticker → CIK：https://www.sec.gov/files/company_tickers.json
2. CIK → 文件列表：https://data.sec.gov/submissions/CIK{10位补零}.json
3. 文件 URL：https://www.sec.gov/Archives/edgar/data/{cik}/{accession号无横线}/{主文档名}
```

**全文检索**
```
GET https://efts.sec.gov/LATEST/search-index?q="{关键词}"&forms=10-K&startdt=2020-01-01&enddt=2026-12-31
```

**文档类型速查**
| 类型 | 含义 | 引用格式 |
|------|------|----------|
| 10-K | 年报 | Item 章节号（如 Item 1A 风险因素） |
| 10-Q | 季报 | 章节号 |
| 8-K | 重大事件临时公告 | Item 号 |
| DEF 14A | 股东大会代理声明（含高管薪酬） | 章节号 |
| 20-F | 中概股/外国公司年报 | 章节号 |
| 6-K | 中概股临时报告 | 章节号 |
| S-1/F-1 | 招股书 | 章节号 |

美股文档引用**章节号**而非页码。注意 XBRL 字段名与展示值的口径陷阱（单位可能是千美元/百万美元）。

## 2. 港股公告 —— 港交所披露易 🔓

- 入口：`https://www1.hkexnews.hk/search/titlesearch.xhtml`
- 按股份代码 + 公告类别（业绩公告、年报、通函）检索
- 港股公告多为中英文双语，引用时注明语言版本

## 3. 学术论文

**arXiv** 🔓 —— 预印本
```
GET http://export.arxiv.org/api/query?search_query=all:"{关键词}"&start=0&max_results=10
```
引用格式：arXiv:XXXX.XXXXX + 章节号。注意预印本未经同行评审，报告中需注明。

**Crossref** 🔓 —— 正式发表论文元数据 + DOI
```
GET https://api.crossref.org/works?query.bibliographic={标题}&rows=5
（建议在 User-Agent 或参数中附 mailto，进入 polite pool）
```
用于确认论文是否真实存在、期刊、发表日期。

**PubMed** 🔓 —— 生物医学
```
检索：https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term={关键词}&retmode=json
取摘要：https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=pubmed&id={pmid}&retmode=xml
```

**bioRxiv / medRxiv** 🔓 —— 生物/医学预印本，API 公开

**论文核验要点**：自媒体常夸大论文结论（"研究证明"实为"相关性提示"），必须读原文 Conclusion 部分核对措辞强度。

## 4. 国际统计数据 🔓

**世界银行**
```
GET https://api.worldbank.org/v2/country/{国家代码}/indicator/{指标代码}?format=json
例：中国GDP → /country/CHN/indicator/NY.GDP.MKTP.CD?format=json
```

其他：IMF Data、WHO 统计、UN Data、OECD Data —— 均为公开入口，按指标检索。

## 5. 美国政府与法规 🔓

**Federal Register**（联邦公报，行政命令/规章原文）
```
GET https://www.federalregister.gov/api/v1/documents.json?conditions[term]={关键词}
```

**congress.gov**（法案原文与进度）：`https://api.congress.gov`（需免费申请 key）或网页检索

**白宫官网**：行政命令、讲话实录原文

## 6. 欧盟法规 🔓

**EUR-Lex** —— `https://eur-lex.europa.eu`
- 法规用编号引用（如 GDPR = Regulation (EU) 2016/679 第X条）

## 7. 技术标准

- **RFC**（互联网标准）🔓：`https://www.rfc-editor.org/rfc/rfc{编号}` —— 全文免费
- **ISO/IEC**：目录与摘要公开，全文付费；引用标准号 + 条款号即可

## 8. 国际组织与通讯社

- 联合国文件：`https://documents.un.org`（决议编号引用，如 A/RES/xx/xx）
- 通讯社原始通稿：Reuters、AP、AFP —— 国际事件的多数二手报道源头
- 名人言论：优先找原始演讲全文（政府/大学官网实录）、原始播客/视频时间戳，而非二手报道

## 通用注意

- 引用国际组织数据时注明**统计年份与修订版本**（历史数据会被回溯修订）
- 英文原文被翻译成中文时是失真高发环节，核验以英文原文为准
- "据外媒报道"不等于有出处——必须落到具体的媒体名 + 发布日期 + 原文链接
