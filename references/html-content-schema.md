# HTML 内容 Schema 与模板填充规则

本文件定义 Agent 如何把已完成的摄影分析填入 `html-template.html`。它约束内容结构和占位符，不取代 `photo-analysis.md` 的分析方法，也不允许为满足字段而编造信息。

## 1. 基本原则

- 先分析，后渲染；模板不决定分析结论。
- 文字占位符必须进行 HTML 转义：至少处理 `&`、`<`、`>`、`"`、`'`。
- 只有明确标为 HTML 片段的占位符可以注入标签；片段只能使用本文件规定的结构和类名，不得包含脚本、事件处理属性、iframe、object、embed、外链或任意 style。
- 原图必须来自本次实际可访问的图像字节，内嵌为 `data:image/...;base64,...`；不得把文件名、网页 URL 或示意图当作原图。
- 所有占位符都必须替换；输出前搜索 `{{` 与 `}}`，若仍存在就不能交付。
- 空的可选片段使用空字符串，并移除对应整段 section；不保留空容器。

## 2. 占位符清单

| 占位符 | 类型 | 要求 |
|---|---|---|
| `{{TITLE}}` | 文字 | 简短、有内容指向，不用泛化标题 |
| `{{DECK}}` | 文字 | 一句话说明这张卡的学习价值 |
| `{{IMAGE_DATA_URI}}` | 图像数据 | 原图字节的 Data URI；不可外链 |
| `{{IMAGE_ALT}}` | 文字 | 准确描述照片主体和关键关系，不加入推测 |
| `{{IMAGE_WIDTH}}` / `{{IMAGE_HEIGHT}}` | 数字 | 原图真实像素宽高，正整数 |
| `{{IMAGE_DIMENSIONS_LABEL}}` | 文字 | 如“原始尺寸 4032 × 3024” |
| `{{ANNOTATION_LAYER}}` | HTML 片段 | 有准确标注时填 SVG；否则为空 |
| `{{ANNOTATION_CONTROL}}` | HTML 片段 | 有标注时填按钮和图例；否则为空 |
| `{{OBSERVATION_TEXT}}` | 文字 | 至少三项直接可见元素及画面位置 |
| `{{INTENT_NOTE}}` | HTML 片段 | 仅用户自己的照片需要；否则为空 |
| `{{CORE_DECISION}}` | 文字 | 一个最值得学习的拍摄决定 |
| `{{CORE_EXPLANATION}}` | 文字 | 证据、机制与观看效果的简洁连接 |
| `{{ANALYSIS_CARDS}}` | HTML 片段 | 2–4 个分析卡；只选相关维度 |
| `{{REPLICATION_CARDS}}` | HTML 片段 | 光线、构图、色彩三个复刻卡 |
| `{{LENS_SUMMARY}}` | 文字 | 视角观感、透视、距离、景深等镜头语言 |
| `{{PARAMETER_SUMMARY}}` | 文字 | EXIF/作者来源事实、推测或“无法确定” |
| `{{ADVANCED_SECTION}}` | HTML 片段 | 可选的完整折叠区 section；无深度内容时为空 |
| `{{SOURCE_LINE}}` | 文字 | 原图/作者/来源；只写真实已知来源 |
| `{{VERIFICATION_LINE}}` | 文字 | 本次真实完成的检查及未完成项 |

## 3. HTML 片段结构

### 分析卡：`{{ANALYSIS_CARDS}}`

每张卡使用以下结构，按实际需要重复 2–4 次：

```html
<article class="insight-card">
  <span class="insight-index">01</span>
  <div class="insight-copy">
    <h3>光影 · 亮区让目光有了落点</h3>
    <p><strong>证据</strong>：画面中的具体位置与关系。
       <strong>机制</strong>：这些关系如何组织观看。
       <strong>效果</strong>：为什么适合这张照片。</p>
  </div>
</article>
```

### 三要素复刻卡：`{{REPLICATION_CARDS}}`

必须恰好包含光线、构图、色彩三张卡，顺序可按题材调整，但三项都要有现场动作和检查信号：

```html
<article class="replica-card">
  <div class="replica-heading">
    <span class="replica-index">01</span>
    <h3>光线 · 先找什么</h3>
  </div>
  <p class="replica-action">寻找条件与具体安排动作。</p>
  <p class="replica-check"><strong>检查</strong>：成功时画面会出现的可见信号。</p>
</article>
```

### 意图说明：`{{INTENT_NOTE}}`

仅用于用户自己的照片，使用一段 `<p class="intent-note">...</p>`。尊重用户明确表达的意图；没有表达时用“这张似乎想突出……”标明是个人解读，并指出一处已做好的地方。

### 标注层：`{{ANNOTATION_LAYER}}`

只允许内嵌 SVG，使用原图像素坐标：

```html
<svg id="annotation-layer" class="annotation-layer" data-annotation-layer
     viewBox="0 0 4032 3024" preserveAspectRatio="none"
     role="img" aria-label="照片观察标注" hidden>
  <g fill="none" stroke="#D7B978" stroke-width="8">
    <ellipse cx="..." cy="..." rx="..." ry="..." />
  </g>
  <g class="annotation-number" transform="translate(... ...)">
    <circle r="28" stroke="#26362D" stroke-width="2" />
    <text text-anchor="middle" dy="9" font-size="30">1</text>
  </g>
</svg>
```

上面尺寸和坐标仅为结构示例，不可复制到实际照片。按真实原图尺寸填写 `viewBox`，所有坐标按原图像素定位；线宽约为长边的 0.2%，编号圆半径约为长边的 0.7%，文字字号约为长边的 0.75%，随原图尺寸同比例计算。SVG 必须带 `id="annotation-layer"`，与标注按钮的 `aria-controls` 对应。SVG 不允许 script、foreignObject、外部资源或事件处理器。编号从 1 开始且不重复；图例与编号一一对应。没有足够定位证据时，两个标注占位符都留空.

### 标注控制：`{{ANNOTATION_CONTROL}}`

有标注时填入按钮和图例；没有标注时整个片段为空：

```html
<div class="annotation-tools">
  <button class="annotation-toggle" type="button"
          data-annotation-toggle aria-pressed="false"
          aria-controls="annotation-layer annotation-legend">
    显示观察标注
  </button>
  <div class="annotation-legend" id="annotation-legend"
       data-annotation-legend hidden>
    <p><b>1</b> 图例说明，描述可见位置与分析用途。</p>
  </div>
</div>
```

图例说明应解释标注为什么支持某项判断，不重复整段分析。

### 深度区：`{{ADVANCED_SECTION}}`

有内容时填入一个完整 `<section class="deep-section">`，内部使用一个或多个原生 `<details class="deep-item">`。可放证据/替代解释、参数来源、迁移、反事实与局限。默认关闭；不要把可见主体整段复制进去。无额外价值时整个片段为空。

## 4. 内容密度

- 标题尽量短，副标题一句话。
- 核心判断只选一个拍摄决定。
- 分析卡通常 2–4 张；只写最能解释效果的维度。
- 可见速学主体约 300–500 个中文字为目标，避免重复；复杂照片可略增，但深度细节优先折叠。
- 三要素每张卡只保留一个优先动作和一到两个检查信号。
- 镜头语言与参数区保持简洁。没有证据的参数明确写“无法确定”，不要为了填满字段猜测。
- 不强制 A/B；没有必要时不出现对照卡。
- 不要加入评分、星级、百分比、KPI、无意义徽章或营销文案。

## 5. 安全与一致性

- `TITLE`、`DECK`、`IMAGE_ALT`、观察、核心判断、解释、镜头参数、来源和验证说明均作为纯文本转义。
- SVG 片段只含安全的静态几何元素和文本；不含脚本、外链、事件处理器或嵌入 HTML。
- 不把用户输入拼入 CSS/JS；模板内 JS 保持固定，不按照片动态生成脚本。
- 图片路径和来源路径不等于公开授权。私人照片不上传仓库。
- HTML 与 Markdown 的主技巧、事实、推测、未知、复刻步骤一致。
- `VERIFICATION_LINE` 只写实际完成的检查。静态校验、浏览器检查、实拍验证是不同证据，不能混称。

## 6. 输出前机器/人工自检

1. 搜索并确认无 `{{` 或 `}}` 残留。
2. 确认图像 src 是 Data URI，不是 http(s) URL；width/height 与原图一致。
3. 确认 SVG viewBox 与原图宽高一致，编号与图例集合一致。
4. 确认没有外部 script、stylesheet、font、image、iframe 或 fetch 请求。
5. 确认所有 HTML 片段只使用已定义结构；文字内容已经转义。
6. 确认没有空 section、无效按钮或重复来源。
7. 实际打开后检查桌面/窄屏、标注切换、折叠和打印；若做不到，明确标记未完成。
