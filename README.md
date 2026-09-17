# concept-radar

渠道无关、周期无关的概念发现 Skill。它从单个 URL 或作者池抓取内容，使用来源 revision、
内容哈希、语义哈希与概念指纹去重，并生成带证据的 Markdown/JSON 概念卡片。

它的重点不是追逐最新内容，而是建立三道判断门槛：

- **抗 FOMO**：新、热、标题醒目或作者知名都不能替代机制、问题与证据。
- **证据分层**：区分一手机制证据、独立对比来源和分析者推断；成熟度不是热度，
  置信度也不是主观喜好。
- **范式对比**：明确新旧方案的共同目标、机制差异、适用场景、成本与非替代边界。

## 快速开始

在仓库根目录运行：

```bash
python3 .trae/skills/concept-radar/scripts/concept_radar.py doctor
python3 .trae/skills/concept-radar/scripts/concept_radar.py scan --url https://example.com/article
```

阅读 `.trae/concept-radar/runtime/candidates.json` 后，按
`examples/selection.example.json` 生成 selection，再运行：

```bash
python3 .trae/skills/concept-radar/scripts/concept_radar.py \
  validate-selection \
  --selection /path/to/selection.json
```

默认产物：

- `.trae/concept-radar/runtime/concept-radar.md`
- `.trae/concept-radar/runtime/concept-radar.json`

## 样例

- `examples/llm-wiki-brief.md`：可直接阅读的黄金简报，展示抗 FOMO、证据分层和
  LLM Wiki 与 RAG 的范式对比。
- `examples/llm-wiki-card.json`：符合卡片字段要求的黄金结构化 selection。
- `examples/rejected-hello.md`：不应入选的反例，说明为什么新奇随笔不自动构成概念。
- `examples/selection.example.json`：用于替换候选标识与引文的通用模板。

黄金样例只包含公开来源的短引文和分析结果，不包含抓取正文、运行状态、私有作者池
或凭证；不可直接当作本轮运行 selection 提交。

## 作者池

配置沿用 `.trae/concept-radar/authors.yaml`。通用配置示例见
`config/authors.example.yaml`。当前支持 RSS/Atom 和 GitHub Gists：

```bash
python3 .trae/skills/concept-radar/scripts/concept_radar.py scan --mode author-pool
```

## 可选发布

默认流程不需要飞书，也不会发送消息。只有用户明确要求发布时运行：

```bash
python3 .trae/skills/concept-radar/scripts/concept_radar.py doctor --channel feishu
python3 .trae/skills/concept-radar/scripts/concept_radar.py publish --channel feishu
```

发布失败不会伪造 `published` 状态；只有 CLI 成功且返回 `ok: true` 与 message_id 后才提交。

## 测试

```bash
python3 -m unittest discover -s .trae/skills/concept-radar/tests -v
```

测试覆盖单 URL、Feed、Gist 降级、去重、卡片与证据校验、默认 doctor 无飞书依赖，
以及可选飞书发布的提交条件。

## 发布

发布包应包含 `LICENSE`、`CHANGELOG.md`、Skill 指令、脚本、references、config、
examples 和 tests；不得包含运行状态、抓取正文、凭证、真实作者池、缓存或生成结果。
详见 `RELEASE.md`、`.gitignore` 与 `.skillignore`。