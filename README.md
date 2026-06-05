# build-xmind-test-cases

用于将 PRD、原型或已有测试用例整理成 April 偏好的 XMind 测试用例脑图结构。

## 目录

```text
skills/build-xmind-test-cases/
  SKILL.md
  agents/openai.yaml
  references/prompt-patterns.md
  references/warehouse-xmind-pattern.md
```

## 核心规则

- 用例流程编码作为第一层节点，如 `RCV10`、`SKU20`、`PUT30`。
- 流程名称节点不重复编码。
- 删除 `测试步骤（操作）` 汇总节点。
- 每个操作步骤下必须有测试数据节点；没有输入数据也保留空节点。
- 每个测试数据节点下接该步骤的预期结果。
- XMind 样式写入 `content.json` topic style：灰色分支线、黑色加粗边框。

## 使用方式

将 `skills/build-xmind-test-cases` 复制到 Codex skills 目录后，可通过：

```text
用 $build-xmind-test-cases 根据这个 PRD 生成 XMind 测试用例脑图。
```

触发生成流程。
