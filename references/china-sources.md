# 中文一手信源检索手册

> 按信源类型列出检索入口、API 用法与注意事项。标注 🔓 的为免密钥公开渠道，优先使用。

## 1. 上市公司公告 —— 巨潮资讯 🔓

A股、港股（部分）官方指定信息披露平台。

**全文搜索**
```
POST http://www.cninfo.com.cn/new/fulltextSearch?notautosubmit=&keyWord={关键词}
```

**按公司查公告**
```
POST http://www.cninfo.com.cn/new/hisAnnouncement/query
参数：stock={股票代码,如 600519}, searchkey={关键词}, category={category_ndbg_szsh 年报等},
     pageNum=1, pageSize=30, column=sse, tabName=fulltext
```
- 返回 `adjunctUrl` 字段，拼接下载地址：`http://static.cninfo.com.cn/{adjunctUrl}`
- PDF 下载后逐页解析，引用时标注页码
- 常见 category：年报 `category_ndbg_szsh`、半年报 `category_bndbg_szsh`、季报 `category_yjdbg_szsh`

## 2. 政策法规

**国家法律法规数据库** 🔓 —— `https://flk.npc.gov.cn`
- 宪法、法律、行政法规、地方性法规、司法解释的官方权威文本
- 引用用条款号（第X条第X款）

**中国政府网政策文件库** 🔓 —— `https://www.gov.cn/zhengce/`
- 国务院文件、部门规章、政策解读；注意区分"原文"与"解读稿"

**部委官网**：财政部、央行、发改委等各自官网的"政策发布"栏目，部门规章以官网发布版为准

## 3. 统计数据 🔓

**国家统计局** —— `https://data.stats.gov.cn`
```
GET https://data.stats.gov.cn/easyquery.htm?m=QueryData&dbcode=hgnd&rowcode=zb&colcode=sj&dfwds=[...]
```
- dbcode：年度 `hgnd`、季度 `hgjd`、月度 `hgyd`
- 引用时注明指标代码与统计口径

**中国人民银行** —— 货币政策执行报告、金融统计数据报告（官网"统计数据"栏目）
**海关总署** —— 进出口数据（官网"统计月报"）

## 4. 国家标准 🔓

**国家标准全文公开系统** —— `https://openstd.samr.gov.cn`（原 std.samr.gov.cn）
- GB 强制性国标在线阅读；GB/T 推荐性国标部分公开
- 引用用标准号 + 条款号（GB XXXXX—XXXX 第X.X条）

## 5. 司法案件

- **中国裁判文书网** `https://wenshu.court.gov.cn`：需登录且访问受限，能找到时引用案号
- **最高人民法院公报/指导性案例**：官网公开发布，权威可靠
- 引用格式：（年份）+ 案号 + 法院

## 6. 学术论文

知网、万方无公开 API，处理方式：
1. 用论文标题在百度学术/Google Scholar 精确检索确认存在性
2. 获取期刊、年份、DOI
3. 结论性内容尽量找到开放获取版本或作者预印本核对
4. 无法获取全文时在报告中标注"仅确认文献存在，内容未核验"

## 7. 新闻事件

- **新华社通稿**：`http://www.news.cn` —— 国内重大事件的源头通稿，多数媒体转载自此
- **政府新闻发布会实录**：中国政府网"新闻发布"栏目、各部委官网"发布会实录"
- 核对要点：自媒体常把通稿中的中性表述改写为情绪化标题

## 8. 其他专项

| 类型 | 官方渠道 |
|------|----------|
| 药品/医疗器械 | 国家药监局 NMPA `https://www.nmpa.gov.cn` |
| 专利 | 国家知识产权局 CNIPA |
| 企业注册信息 | 国家企业信用信息公示系统 `https://www.gsxt.gov.cn` |
| 招投标 | 中国招标投标公共服务平台 |
| 环境数据 | 生态环境部官网 |

## 通用注意

- 政府网站改版频繁，入口失效时改用站内搜索或搜索引擎 `site:` 限定域名
- 政策类信息务必核对**发布日期与施行日期**，旧规新炒是高频失真模式
- 引用地方政策时注意适用范围（仅某省/市有效）
