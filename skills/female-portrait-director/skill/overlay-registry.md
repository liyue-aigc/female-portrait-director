# 女性人像提示词导演 Skill｜轻量 Overlay 注册表

版本编号：`FEMALE-PORTRAIT-DIRECTOR-V1.4.1`

Overlay 只增强人物气质，不替代主 Route。仅在用户明确表达气质倾向，或主 Route 需要稳定化补充时，读取一个最匹配的 Overlay 文件。没有明确需要时不加载。

## 已实现 Overlay

| Overlay ID | 气质方向 | 主要触发词 | 文件 |
| --- | --- | --- | --- |
| `bright-heroine` | 明艳女主增强 | 明艳、女主感、抓眼、存在感、妆造完整 | [overlays/bright-heroine.md](overlays/bright-heroine.md) |
| `cold-heroine` | 清冷女主增强 | 清冷、疏离、克制、冷白、高级冷感 | [overlays/cold-heroine.md](overlays/cold-heroine.md) |
| `cool-mature` | 冷艳御姐增强 | 冷艳、御姐、强气场、成熟、控制力 | [overlays/cool-mature.md](overlays/cool-mature.md) |
| `gentle-sister` | 温柔姐姐感增强 | 温柔姐姐、轻熟温柔、亲和、柔和、自然亲近 | [overlays/gentle-sister.md](overlays/gentle-sister.md) |
| `intellectual` | 高智感知识女性增强 | 知性、高智感、书卷气、理性、专业感 | [overlays/intellectual.md](overlays/intellectual.md) |
| `mature-urban` | 轻熟都市女性增强 | 轻熟、都市女性、优雅、通勤、现代生活方式 | [overlays/mature-urban.md](overlays/mature-urban.md) |
| `sweet-cool` | 甜酷年轻女性增强 | 甜酷、年轻活力、街头个性、轻时尚 | [overlays/sweet-cool.md](overlays/sweet-cool.md) |

## 使用规则

- 先确定主 Route，再判断是否叠加 Overlay。
- Overlay 只能调整气质、眼神、表情、姿态、妆容强度、穿搭细节和少量光线色调。
- Overlay 不得覆盖用户锁定参数，不得把生活照改成棚拍、把都市街拍改成古风或改变商业用途。
- 一个请求默认只读取一个 Overlay。确需组合时，只保留兼容且不冲突的增强项。
