# source-tracer · 信息溯源 Skill

> 给任何 AI 助手装上"查证员"模式：把一段二手信息**一键溯源**回官方一手出处，并逐条核对是否被歪曲。

我们每天消费大量二手资料——自媒体文章、短视频文案、群聊截图、新闻转述。这些信息经过层层转述，数据可能失真、引语可能断章取义、旧闻可能新炒。这个 Skill 让 AI 帮你回答三个问题：

1. **源头在哪？** —— 找到官方一手出处（政府文件、公司公告、原始论文、发布会实录……）
2. **原文怎么写的？** —— 精确到页码/章节/条款的原文摘录
3. **转述可信吗？** —— 按八大失真模式逐条核验，输出 ✅ / ⚠️ / ❌ / ❓ 判定

## 工作原理

```
二手信息输入
    │
    ▼
Step 1  声明拆解      提取可核验的原子声明（数据/引言/事件/政策/学术）
    │
    ▼
Step 2  信源路由      按类型匹配一手信源（巨潮/EDGAR/法律法规库/统计局/arXiv……）
    │
    ▼
Step 3  检索获取      走免密钥公开 API，拿到原文全文
    │
    ▼
Step 4  原文定位      标注精确位置（页码/条款/章节/时间戳）
    │
    ▼
Step 5  一致性核验    八大失真模式逐项检查 → 溯源报告
```

这不是一个需要运行的程序——它是一份写给 AI 的"操作手册"（SKILL.md 标准格式），安装后 AI 自动获得这套工作流。

## 信源覆盖

| 中文 | 国际 |
|------|------|
| 巨潮资讯（A股公告） | SEC EDGAR（美股） |
| 国家法律法规数据库 | 港交所披露易 |
| 中国政府网政策库 | arXiv / Crossref / PubMed |
| 国家统计局 | 世界银行 / IMF / WHO |
| 国家标准全文公开系统 | Federal Register / EUR-Lex |
| 裁判文书网 / 最高法公报 | RFC / 联合国文件 |
| 新华社通稿 / 发布会实录 | Reuters / AP 原始通稿 |

详见 [references/china-sources.md](references/china-sources.md) 与 [references/global-sources.md](references/global-sources.md)，内含各信源的 API 调用方法。

## 安装

### Claude Code / Trae（原生支持）

```bash
# Windows
git clone <repo-url> "%USERPROFILE%\.claude\skills\source-tracer"
# macOS / Linux
git clone <repo-url> ~/.claude/skills/source-tracer
```

Trae 用户：将本仓库放入 `~/.trae-cn/skills/` 目录。

### Codex

将 `SKILL.md` 正文（去掉 frontmatter）追加到你的全局 `AGENTS.md`。

### 任意网页版 AI（豆包 / ChatGPT / Kimi……）

打开对话，把 `SKILL.md` 全文粘贴为第一条消息，然后在同一对话中发送你想溯源的内容。

## 使用示例

**溯源一条政策传言**
> 我刷到一篇文章说"国家出台新规，明年起所有外卖必须明示预制菜"，帮我查一下原文。

→ AI 会检索国家法律法规数据库与部委官网，找到文件原文，标注条款号，核对"明年起""所有"等措辞是否与原文一致。

**溯源一个数据**
> 有视频说"中国单身人口已超2.4亿"，这数字哪来的？

→ 路由到国家统计局数据，核对统计口径与年份，判断是否旧数据新炒。

**溯源一篇论文结论**
> 公众号说"《自然》最新研究证实熬夜直接致癌"，是真的吗？

→ 用 Crossref/PubMed 定位原文，读 Conclusion 核对措辞强度（"相关性提示" vs "直接致癌"）。

## 目录结构

```
source-tracer/
├── SKILL.md                            # 核心：角色、原则、工作流、输出格式
├── README.md
└── references/
    ├── china-sources.md                # 中文一手信源检索手册（含巨潮/统计局 API）
    ├── global-sources.md               # 国际一手信源检索手册（含 EDGAR/arXiv API）
    └── verification-checklist.md       # 八大失真模式核验清单
```

## 设计红线

- **不编造**：找不到源头必须明说，严禁编造出处、编号、链接
- **可复查**：所有结论附检索式与原文位置，第三方可独立验证
- **只核验事实**：判断"转述是否与原文一致"，不仲裁观点对错
- **免责定位**：本工具是信息溯源辅助，不构成投资建议、法律意见或事实仲裁

## License

MIT
