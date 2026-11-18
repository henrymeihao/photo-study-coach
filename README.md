# 摄影拆解与练习 · Photo Study Coach

把喜欢的摄影作品，或你自己拍的照片，变成一个下次拍照能用的决定：先看什么、只改哪一处、拍完怎么检查。

每张图只教一个技巧，再配一个“只改一处会怎样”的对照，让你用 A/B 两张实拍去验证。分析时区分作者原文或 EXIF、画面可见证据和推测，不编造相机参数。

## 两种用法

**在 AI Agent 里用（Codex、Claude Code 等支持 Skill 的客户端）**：Agent 按固定模板输出学习卡，需要时存成 Markdown 或制作离线 HTML。

**在聊天 App 里用（DeepSeek、豆包、Kimi、ChatGPT 等）**：复制 [prompts/chat-prompt.md](prompts/chat-prompt.md) 里的提示词，再上传照片。不需要安装任何东西。

## 模型要求

模型必须能读取图片。以 DeepSeek 为例，按 2026-11-18 的[官方模型页](https://api-docs.deepseek.com/quick_start/pricing)，`deepseek-flash` 支持图片输入，V4-Pro 不支持。其他模型请查各自文档。

模型支持图片还不够，客户端或第三方 API 网关也要把图片真正传过去。评测中出现过图片没传到模型、模型却描述了一张不存在的照片的情况。所以每张学习卡第一行都是“我看到的画面”，先核对这一行再往下读；对不上就换模型或客户端。

## 安装

目录名必须是 `photo-study-coach`，目录内直接包含 `SKILL.md`：

```bash
# Codex
git clone https://github.com/henrymeihao/photo-study-coach ~/.codex/skills/photo-study-coach

# Claude Code
git clone https://github.com/henrymeihao/photo-study-coach ~/.claude/skills/photo-study-coach
```

其他客户端放到各自的技能目录。仓库只有 Markdown 和 YAML 文本，没有可执行脚本，不需要安装依赖。

重新加载技能后：

```text
用 $photo-study-coach 拆解这组作品：[链接或图片]
```

```text
用 $photo-study-coach 看看我拍的这张，下次只改一处的话改什么？
```

```text
这是 A/B 练习照，A 是低机位，B 只升高了相机。帮我复盘。
```

## 文件

```text
SKILL.md                         流程与核心规则
references/photo-analysis.md     诊断步骤，构图、用光、色彩、镜头怎么看
references/card-format.md        学习卡模板、交付前检查表和完整示例
references/practice-review.md    A/B 练习复盘
references/acceptance-tests.md   验收用例 T1–T19
prompts/chat-prompt.md           聊天 App 用的提示词
evals/README.md                  怎样评测输出是否稳定
agents/openai.yaml               技能展示信息
```

## 边界

相机参数与布光无法只凭成片准确还原。机位图和对照都是学习示意，除非有来源，否则不代表作者真实的拍摄过程。读取网页需要客户端有浏览能力，本仓库不附带爬虫，也不保证任意平台的链接都能读取。

## 反馈

用过之后，欢迎在 Issues 里用“使用反馈”模板告诉我：你用它做了什么、有没有真的去拍 A/B、哪部分有用、哪部分没看。这些信息决定接下来改什么。

## 灵感来源与许可

受到小红书作者 **LEMON 林檬养乐多** 的[《为了高效学摄影，我专门做了个skill》](https://www.xiaohongshu.com/explore/6ae82d7b000000002501a570)启发，依据公开演示重新编写。未取得或复制其私信分发的技能源码，与原作者没有合作或官方关联。

本仓库仅包含新编写的说明和脚本，不包含原帖图片、第三方摄影原图、私人照片或个人学习记录。生成学习资料时请保留原作者署名与来源；发布或再分发第三方图片需另行确认相应权利。

代码与文档以 [MIT 许可](LICENSE) 发布。
