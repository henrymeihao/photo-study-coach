# 摄影拆解与练习 · Photo Study Coach

把喜欢的摄影作品，或你自己拍的照片，变成真正能迁移到下一次拍摄的摄影判断：**它为什么好看，以及我怎样拍出类似的效果。**

Photo Study Coach 不只是描述构图和光线，而是解释光影、构图、色彩、人物表现如何共同产生视觉效果，再把方法拆成三个现场要素：**光线、构图、色彩**。

默认交付视觉优先的离线 HTML 学习卡和 Markdown 档案。HTML 使用仓库固定模板与设计系统，Agent 只负责填充经过证据约束的分析内容；原图以 Data URI 内嵌，SVG 仅作叠加标注，不使用生成式模型重绘照片。深度分析默认折叠，镜头与参数模块优先解释镜头语言，并严格区分 EXIF 事实、画面推断和未知参数。

## 两种用法

**在 AI Agent 里用（Codex、Claude Code 等支持 Skill 的客户端）**：Agent 按固定结构分析照片，默认制作离线 HTML 和 Markdown；用户明确只要文字或其他格式时遵循指定格式。

**在聊天 App 里用**：复制 [prompts/chat-prompt.md](prompts/chat-prompt.md) 的提示词，再上传照片。模型必须支持图片输入，客户端或 API 网关也必须真正传递图片。

## 安装

目录名必须是 `photo-study-coach`，目录内直接包含 `SKILL.md`：

```bash
# Codex
git clone https://github.com/henrymeihao/photo-study-coach ~/.codex/skills/photo-study-coach

# Claude Code
git clone https://github.com/henrymeihao/photo-study-coach ~/.claude/skills/photo-study-coach
```

重新加载技能后：

```text
用 $photo-study-coach 拆解这张照片：为什么好看，以及怎么复刻？
```

```text
用 $photo-study-coach 点评我拍的这张人像，重点看光影、构图和色彩。
```

## 文件

```text
SKILL.md                         主流程、产品原则与交付规则
references/photo-analysis.md     摄影分析方法、三要素复刻、镜头语言与参数可信度
references/card-format.md        HTML/Markdown 信息架构与交付检查
references/html-template.html    固定离线 HTML 页面模板（视觉与交互基线）
references/html-design-system.md  视觉 token、布局、组件与响应式规范
references/html-content-schema.md HTML 占位符、内容结构与安全填充规则
references/practice-review.md    可选实拍练习与回传复盘
references/acceptance-tests.md   输出验收用例
prompts/chat-prompt.md           聊天 App 用的提示词
evals/README.md                  怎样评测输出是否稳定
agents/openai.yaml               技能展示信息
```

## 关键边界

- 原始照片保持不变。标注使用 SVG/CSS 叠加；不使用 AI 生图重绘照片。
- 不强制每张照片都设计 A/B 练习。练习应服务于真实学习目标。
- 相机参数与布光无法只凭成片准确还原。EXIF/作者原文有证据时引用；否则解释镜头语言，未知参数直接标注未知。
- 不把推测的光源、机位、焦段或器材写成作者真实拍摄过程。
- 需要 HTML 时必须使用仓库固定模板与设计系统，单文件离线交付，不依赖在线 API 或外部图片地址。
- 没有原图字节时不生成假图或外链图；没有实际打开最终文件时不宣称完成浏览器验收。
- 不上传私人照片、私人学习笔记或活动记录到公开仓库。

## 灵感来源与许可

受到小红书作者 **LEMON 林檬养乐多** 的《为了高效学摄影，我专门做了个skill》启发，依据公开演示重新编写。未取得或复制其私信分发的技能源码，与原作者没有合作或官方关联。

生成学习资料时请保留原作者署名与来源；发布或再分发第三方图片需另行确认相应权利。

代码与文档以 [MIT 许可](LICENSE) 发布。
