# Warehouse XMind Test Case Pattern

Use this structure for April's warehouse administrator digital employee project. The `.xmind` file should focus on executable test case flows, not explanatory coverage sections.

## Preferred Node Hierarchy

```text
<用例流程编码>
  <主流程名称>
    <优先级>
      <前置条件>
        <是否为冒烟用例>
          <编号化操作步骤 1>
            <测试数据；没有输入数据也必须保留空节点>
              <该步骤对应的中间输出或预期>
          <编号化操作步骤 2>
            <测试数据；没有输入数据也必须保留空节点>
              <该步骤对应的中间输出或预期>
```

Do not create a wrapper node named `测试步骤（操作）`.

## Code Rules

- Inbound receiving: `RCV10`, `RCV20`, `RCV30`.
- SKU validation: `SKU10`, `SKU20`, `SKU30`.
- Feishu distribution: `FS10`, `FS20`, `FS30`.
- Putaway: `PUT10`, `PUT20`, `PUT30`.
- Task tracking: `TASK10`, `TASK20`, `TASK30`.
- Executor list: `USER10`, `USER20`.
- Use increments of 10 so April can insert new flows later.

## Step Number Rules

- Use `1.1`, `1.2`, `2.1`, `2.2` to show order and parallel branches.
- Put user operations and system operations in the same numbered flow when they belong to one execution path.
- Put the intermediate result as a child of the relevant operation step when useful.
- Every operation step must have a test-data child node. Keep the child node empty if the step has no input data.
- Put that operation step's expected result under the test-data node.
- Do not put global `测试数据` and `预期结果` nodes after all steps.

## Priority Rules

- `高`: core path, launch-blocking exception, inventory/order risk.
- `中`: common exception or important UI/integration path.
- `低`: placeholder, low-risk UI, exploratory coverage.

## Smoke Rules

- `是`: core happy path and key blocking exception.
- `否`: placeholder, low-risk branch, or detailed regression-only case.

## Style Rules

- Branch line color: `#999999`.
- Topic border color: black.
- Topic border width: thick.
- For modern XMind files, write these into each topic's `content.json` `style.properties`: `line-color: #999999FF`, `line-width: 3pt`, `border-line-color: #000000FF`, `border-line-width: 5pt`.

## Example

```markdown
# Sheet: 智能收货流程测试用例

## 智能收货流程测试用例

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
              - 系统创建收货任务编号，系统完成 OCR 识别并展示 OCR 结果卡
          - 2.1 A 在 OCR 结果卡点击「确认正确」按钮
            - 
              - 结构化数据传递给 SKU 校验步骤
```

## Product Rationale

This structure matches April's hand-built XMind habit: the case code is the visible anchor, the case name follows immediately without repeating the code, and numbered step nodes make execution order easier to scan. Each step carries its own data and expected result, so the map reads horizontally like an executable flow.
