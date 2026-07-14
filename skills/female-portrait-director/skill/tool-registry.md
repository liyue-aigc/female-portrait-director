# 女性人像提示词导演 Skill｜轻量工具注册表

版本编号：`FEMALE-PORTRAIT-DIRECTOR-V1.4.1`

工具模块用于处理明确的功能型任务。命中工具任务时，先读取一个最匹配的工具文件，再按需读取主 Route、Overlay、核心规则和参考库。

## 已实现工具

| Tool ID | 任务类型 | 主要触发词 | 文件 |
| --- | --- | --- | --- |
| `prompt-optimize` | 优化已有提示词 | 优化提示词、增强稳定性、不要改变原参数、修复机械填参 | [tools/prompt-optimize.md](tools/prompt-optimize.md) |
| `failure-diagnosis` | 诊断出图失败 | 为什么跑偏、不稳定、服装不对、脸都一样、色差明显 | [tools/failure-diagnosis.md](tools/failure-diagnosis.md) |
| `parameter-recommend` | 推荐参数组合 | 推荐几组参数、搭配建议、批量出图组合、不同场景方案 | [tools/parameter-recommend.md](tools/parameter-recommend.md) |
| `safety-rewrite` | 安全合规改写 | 降低敏感、审查友好、合规改写、保留合法审美目标 | [tools/safety-rewrite.md](tools/safety-rewrite.md) |
| `image-to-prompt` | 图片反推提示词 | 反推提示词、分析参考图、图片转提示词、风格转译 | [tools/image-to-prompt.md](tools/image-to-prompt.md) |
| `reference-image-generate` | 参考图保留直接生成 | 保留我的五官、用我的自拍、保持产品不变、穿上第二张图衣服、不要提示词直接出图 | [tools/reference-image-generate.md](tools/reference-image-generate.md) |

## 使用规则

- 工具模块不替代参数锁定、主 Route 和安全边界。
- 用户同时要求多个工具任务时，按任务顺序读取对应文件，例如先诊断，再优化。
- 图片反推只提取可观察视觉特征；无法确认的内容标注为推断，不虚构身份信息。
- 参考图保留直接生成必须读取 [core/reference-image-lock.md](core/reference-image-lock.md)，先区分人物、产品、风格参考和待编辑图片，再调用图片生成能力。
- `image-to-prompt` 输出提示词；`reference-image-generate` 默认直接返回图片。不得混淆两者。
- 安全合规改写用于降低风险，不用于规避平台限制。
