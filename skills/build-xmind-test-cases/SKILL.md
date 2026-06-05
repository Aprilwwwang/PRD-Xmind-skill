---
name: build-xmind-test-cases
description: Build XMind mind maps for software and product test cases from PRDs, user stories, UE/Figma/HTML prototypes, workflow documents, API specs, or existing test case tables. Use when Codex is asked to create, export, organize, review, or update test cases in XMind/.xmind format, especially by combining structured QA coverage with a mind-map hierarchy.
---

# Build XMind Test Cases

## Overview

Use this skill to produce XMind-ready test case mind maps. Treat `$write-test-cases` as the coverage and QA reasoning layer, and the installed `$xmind` skill as the file-format layer for creating or updating `.xmind` files.

## Required Coordination

Before generating the final `.xmind` file:

1. Use `$write-test-cases` logic to analyze requirements, risks, normal paths, exception paths, permissions, boundaries, data consistency, and acceptance criteria.
2. Convert the result into an XMind-compatible Markdown tree.
3. Review the Markdown tree for coverage and readable hierarchy.
4. Use `$xmind` to create or update the `.xmind` file.
5. Keep a Markdown source file next to the `.xmind` output when possible, so the user can review and revise before reopening XMind.

## Recommended Output Files

When the user does not specify a path, write formal outputs to the closest project directory:

- Product/PRD test case maps: `outputs/`
- Prototype-specific test case maps: `原型/` or `outputs/`
- Temporary drafts: `outputs/`

Use this filename pattern:

```text
<主题>_测试用例脑图_<YYYYMMDD>_v1.0.xmind
<主题>_测试用例脑图_<YYYYMMDD>_v1.0.md
```

For April's warehouse administrator project, examples:

```text
仓库管理员数字员工_测试用例脑图_20260605_v1.0.xmind
仓库管理员数字员工_测试用例脑图_20260605_v1.0.md
```

## XMind Tree Structure

Use April's preferred executable test-flow hierarchy by default. Keep explanatory coverage notes in Markdown if needed, but keep the `.xmind` file focused on test execution flows.

```text
Root: <Feature or Project> 测试用例
  <用例流程编码>
    <主流程名称>
      <优先级>
        <前置条件>
          <是否为冒烟用例>
            <编号化操作步骤 1>
              <测试数据；没有输入数据也必须保留空节点>
                <该步骤对应的中间输出或预期结果>
            <编号化操作步骤 2>
              <测试数据；没有输入数据也必须保留空节点>
                <该步骤对应的中间输出或预期结果>
```

Use case flow code rules:

- Use the main flow abbreviation plus a numeric code.
- Use `10`, `20`, `30` increments instead of `1`, `2`, `3`, so April can insert new flows later.
- Examples: `IN10`, `IN20`, `OUT10`, `OUT20`, `INV10`.
- If the business already has an abbreviation convention, preserve it.
- Put the flow code as the first visible case node.
- Do not repeat the flow code at the beginning of the flow name node. Use `RCV10 -> 送货收货-标准收货链路`, not `RCV10 -> RCV10送货收货-标准收货链路`.

Priority node values:

- Use `高`, `中`, `低` unless the user requests `P0/P1/P2`.

Smoke node values:

- Use `是` or `否`.
- Core happy paths and launch-blocking exceptions are usually `是`; low-risk placeholder/UI-only cases are usually `否`.

Step node rules:

- Do not create a wrapper node named `测试步骤（操作）`.
- Put numbered operation steps directly under the smoke node.
- Use `1.1`, `1.2`, `2.1`, `2.2` numbering to show sequence and parallel branches.
- Under every operation step, add a test-data child node. If the step has no input data, keep an empty node instead of omitting it.
- Put the expected result of that operation step under the test-data node.
- Do not place global `测试数据` and `预期结果` as siblings after all operation steps.

Style rules:

- For modern XMind files, write styles into each topic's `content.json` `style.properties`, not only into `styles.xml`.
- Use `line-color: #999999FF`, `line-width: 3pt`, `border-line-color: #000000FF`, and `border-line-width: 5pt`.
- If the XMind client still does not render these visual styles after they are present in `content.json`, report that client-side theme/layout override may be preventing deterministic style control.

## XMind Markdown Rules

Use the Markdown format expected by `$xmind`:

```markdown
# Sheet: 测试用例

## <项目或功能> 测试用例

- RCV10
  - 送货收货-标准收货链路
    - 高
      - A 为唯一操作员；送货单图片或 PDF 可正常上传；SKU 主数据均存在且规格、单位一致
        - 是
          - 1.1 A 点击「开始收货」标签按钮
            - 
              - 系统提示上传送货单
          - 1.2 A 在 chatbox 拍照或选择送货单图片上传
            - 纸质送货单
              - 系统创建收货任务编号，完成 OCR 识别并展示 OCR 结果卡
          - 2.1 A 在 OCR 结果卡点击「确认正确」按钮
            - 
              - 结构化数据传递给 SKU 校验步骤
```

Keep node text concise. Put long details under child nodes instead of stuffing everything into one node.

## Workflow

1. Read source material.
   - Use local files first when paths are provided.
   - For HTML prototypes, identify tabs, cards, form fields, table columns, buttons, status states, and scripts.
   - For PRDs, identify business rules, role actions, states, input/output, and acceptance criteria.

2. Create a coverage outline.
   - List modules and process steps.
   - Mark high-risk steps as P0.
   - Capture unclear rules in `假设与待确认问题`.

3. Generate test cases.
   - Use `$write-test-cases` principles.
   - Include functional, exception, UI, integration, UAT, regression, and data/audit coverage when relevant.
   - Do not invent detailed rules for modules that are only placeholders.

4. Convert cases to a mind-map hierarchy.
   - Use the flow code as the first-level case node, such as `RCV10`, `SKU10`, or `PUT20`.
   - Use the case name without repeating the code as the next node.
   - Build the chain in this order: flow code -> case name -> priority -> precondition -> smoke flag -> numbered operation steps -> per-step test data -> per-step expected result.
   - Do not create a `测试步骤（操作）` wrapper node.
   - Keep Markdown source content unchanged when the user asks for XMind-only restructuring.

5. Create Markdown source.
   - Save a `.md` file with the same base name as the `.xmind` output.
   - Verify the source begins with `# Sheet:` and has one `##` root topic.

6. Create XMind file.
   - Use the installed `$xmind` tool to create the `.xmind`.
   - Prefer `legacy` format for now, because current XMind desktop clients may reject the minimal Zen package generated by the third-party `$xmind` skill as "not a valid XMind File".
   - If a generated file fails to open in XMind, regenerate a compatibility build that includes `content.xml`, `styles.xml`, `meta.xml`, and `META-INF/manifest.xml`.

7. Validate output.
   - Confirm both Markdown and `.xmind` files exist.
   - Parse or inspect the `.xmind` content when possible.
   - Report paths and any assumptions/open questions.

## Quality Checks

Before finalizing:

- Confirm the mind map has no orphan cases without expected results.
- Confirm P0 cases cover the core business completion path and blocking exceptions.
- Confirm every critical manual confirmation point is tested.
- Confirm placeholder modules are labeled as pending rules instead of over-specified.
- Confirm filenames follow the requested date/version pattern when generating new files.
- Confirm the `.xmind` ZIP package contains the expected internal files for the chosen format. For legacy compatibility, check `content.xml`, `styles.xml`, `meta.xml`, and `META-INF/manifest.xml`.

## References

Load these only when useful:

- `references/warehouse-xmind-pattern.md`: Recommended XMind structure for warehouse administrator digital employee test cases.
- `references/prompt-patterns.md`: Reusable prompts for turning PRDs/prototypes into XMind test case maps.
