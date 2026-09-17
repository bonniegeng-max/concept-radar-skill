# v0.1.0 发布说明

`concept-radar` v0.1.0 提供渠道无关、周期无关的概念发现流程。发布包强调：

- **抗 FOMO**：不因发布时间、热度、作者影响力或新奇表达降低概念质量门槛。
- **证据分层**：一手来源支持机制，独立来源支持对比，分析推断显式标注不确定性。
- **范式对比**：同时说明共同目标、关键差异、适用时机、成本和非替代边界。

发布时同时使用 `.gitignore` 管理工作树、以 `.skillignore` 作为打包排除清单。

## 必须排除

- `__pycache__/`、`*.pyc`、测试覆盖率与本地工具缓存。
- `runtime/`、`state.json`、`candidates.json`、`concept-radar.md/json` 等运行产物。
- `.env`、token、cookie、open_id、真实作者池配置及其他凭证。
- 抓取到的正文、临时 selection、调试日志和失败响应。

## 必须保留

- `LICENSE`、`CHANGELOG.md`、`SKILL.md`、`README.md`、`RELEASE.md`、
  `.gitignore` 与 `.skillignore`。
- `scripts/`、`references/`、`config/*.example.yaml`、`examples/` 和 `tests/`。
- `examples/llm-wiki-brief.md`、`examples/llm-wiki-card.json` 与
  `examples/rejected-hello.md`，分别作为黄金简报、黄金结构化卡片和拒绝反例。

## 发布前检查

```bash
python3 -m unittest discover -s .trae/skills/concept-radar/tests -v
git check-ignore -v .trae/skills/concept-radar/runtime/state.json
```

此外，应扫描包内疑似密钥、token、cookie、个人标识、运行状态和非示例作者配置；
扫描结果必须为零命中，或逐项确认仅属于规则文本/测试占位符。

注意：仓库级 `.trae/concept-radar/` 是运行数据目录，不属于 Skill 发布包；旧的
`.trae/skills/weekly-concept-radar/` 是独立 Skill，也不应被合并或删除。