# 女性人像提示词导演 Skill｜轻量风格注册表

版本编号：`FEMALE-PORTRAIT-DIRECTOR-V1.5`

本文件是唯一运行时主风格分流入口。每次请求只选择一个已经实现的主 Route。平台用途、气质 Overlay 和扩展占位不得覆盖主 Route。气质增强按需读取 [overlay-registry.md](overlay-registry.md)。

## 分流优先级

1. 用户明确填写且已实现的写真风格。
2. 强商业任务：电商、主图、上传服装、试衣、不要色差。
3. V1.5 复合特征指纹：先匹配“构图 / 妆容 / 色彩 / 光线 / 身形”组合，不用单一的“曲线”“古风”或“CCD”词决定路由。
4. 强曲线任务：纯欲、曲线、锁骨、腰线、小腹、大腿、身形吸引力。
5. 其他明确风格词和整体视觉意图。
6. 无法判断时回退到 `clean-lifestyle`。

## 已实现 Route

| Route ID | 风格 | 分类 | 主要触发词 | 文件 |
| --- | --- | --- | --- | --- |
| `clean-lifestyle` | 清纯生活照 | lifestyle | 清纯、温柔、自然、咖啡馆、窗边、生活剧照 | [routes/lifestyle/clean-lifestyle.md](routes/lifestyle/clean-lifestyle.md) |
| `pure-desire-curve` | 纯欲曲线生活照 | curve | 纯欲、曲线、锁骨、腰线、小腹、大腿、贴身吊带 | [routes/curve/pure-desire-curve.md](routes/curve/pure-desire-curve.md) |
| `urban-fashion` | 都市时尚写真 | fashion | 都市、街拍、OOTD、通勤、西装、风衣 | [routes/fashion/urban-fashion.md](routes/fashion/urban-fashion.md) |
| `gufeng-xianxia` | 古风仙侠美人图 | fantasy | 古风、仙侠、唐风、古偶、披帛、云雾山水 | [routes/fantasy/gufeng-xianxia.md](routes/fantasy/gufeng-xianxia.md) |
| `ecommerce-tryon` | 电商服装模特图 | commercial | 电商、主图、详情页、上传服装、试衣、不要色差 | [routes/commercial/ecommerce-tryon.md](routes/commercial/ecommerce-tryon.md) |
| `retro-hongkong` | 复古港风写真 | lifestyle | 港风、港片女主、旧香港、茶餐厅、霓虹、胶片 | [routes/lifestyle/retro-hongkong.md](routes/lifestyle/retro-hongkong.md) |
| `french-lazy` | 法式慵懒写真 | lifestyle | 法式、慵懒、松弛、公寓、阳台、奶油暖白 | [routes/lifestyle/french-lazy.md](routes/lifestyle/french-lazy.md) |
| `new-chinese` | 新中式东方写真 | oriental | 新中式、东方美学、茶室、屏风、竹影、留白 | [routes/oriental/new-chinese.md](routes/oriental/new-chinese.md) |
| `sporty-active` | 活力运动写真 | fashion | 运动、活力、网球、跑道、健身、健康线条 | [routes/fashion/sporty-active.md](routes/fashion/sporty-active.md) |
| `travel-vacation` | 旅行假日写真 | lifestyle | 旅行、假日、度假、酒店阳台、民宿、海岛 | [routes/lifestyle/travel-vacation.md](routes/lifestyle/travel-vacation.md) |
| `studio-retouched` | 影楼精修写真 | fashion | 影楼、精修、棚拍、写真馆、社交头像 | [routes/fashion/studio-retouched.md](routes/fashion/studio-retouched.md) |
| `oriental-voluptuous` | 东方丰腴写真 | curve | 东方丰腴、丰润、柔润、成熟曲线、旗袍曲线 | [routes/curve/oriental-voluptuous.md](routes/curve/oriental-voluptuous.md) |
| `cold-xianxia-enhanced` | 清冷仙气古风增强版 | fantasy | 清冷仙气、冷白、疏离、空灵、月白、冰蓝 | [routes/fantasy/cold-xianxia-enhanced.md](routes/fantasy/cold-xianxia-enhanced.md) |
| `bright-luxury-gufeng` | 明媚华贵古风增强版 | fantasy | 明媚华贵、盛唐、红金、宫廷、华服、重工头饰 | [routes/fantasy/bright-luxury-gufeng.md](routes/fantasy/bright-luxury-gufeng.md) |
| `ultra-close-real-face` | 超近景真实人脸人像 | realism | 超近景、怼脸、未修图原片、真实皮肤、毛孔、微纹理 | [routes/realism/ultra-close-real-face.md](routes/realism/ultra-close-real-face.md) |
| `ancient-lady-dewy-makeup` | 古风贵女水光妆 | beauty | 古风贵女、水光妆、玻璃唇、贵女美妆特写、富养感 | [routes/beauty/ancient-lady-dewy-makeup.md](routes/beauty/ancient-lady-dewy-makeup.md) |
| `black-pearl-dark-gold-ccd` | 黑珍珠墨金CCD曲线生活照 | curve | 黑珍珠、墨金、暗金反光、夜间、柔和直闪、CCD | [routes/curve/black-pearl-dark-gold-ccd.md](routes/curve/black-pearl-dark-gold-ccd.md) |
| `soft-ccd-energetic-voluptuous` | 元气丰腴柔光CCD生活照 | curve | 柔光CCD、元气、丰腴自然曲线、亮色夏日、柔闪 | [routes/curve/soft-ccd-energetic-voluptuous.md](routes/curve/soft-ccd-energetic-voluptuous.md) |
| `cold-white-clear-ccd-curve` | 冷白清透CCD曲线生活照 | curve | 冷白清透、日间CCD、高色温、纤细曲线、贴身针织 | [routes/curve/cold-white-clear-ccd-curve.md](routes/curve/cold-white-clear-ccd-curve.md) |

## 冲突分流

- `清冷仙气 + 古风` 优先 `cold-xianxia-enhanced`。
- `明媚华贵 + 红金 + 古风` 优先 `bright-luxury-gufeng`。
- `新中式 + 茶室 / 屏风 / 留白` 优先 `new-chinese`，不要误路由到古风仙侠。
- `丰腴 + 东方古典 / 旗袍 / 唐风丰润` 优先 `oriental-voluptuous`。
- `超近景 / 怼脸 + 未修图 + 真实皮肤微纹理` 优先 `ultra-close-real-face`；普通美女近景、商业精修头像或证件照不得误触发。
- `古风贵女身份 + 水光妆 / 玻璃唇 + 美妆近景` 优先 `ancient-lady-dewy-makeup`；仙侠特效优先 `gufeng-xianxia`，红金宫廷排场优先 `bright-luxury-gufeng`，现实留白东方空间优先 `new-chinese`。
- `黑珍珠 / 墨金暗部 + 暗金反光 + 夜间柔和直闪` 优先 `black-pearl-dark-gold-ccd`。
- `柔光 CCD + 元气明亮 + 丰腴自然曲线 + 亮色日常穿搭` 优先 `soft-ccd-energetic-voluptuous`。
- `冷白清透 CCD + 日间高色温 + 纤细曲线 + 贴身完整穿搭` 优先 `cold-white-clear-ccd-curve`。
- 只有“CCD”或“曲线”而没有上述复合指纹时，不得在三条新 CCD Route 之间猜测；根据用户明确主目标选择 `pure-desire-curve`、`oriental-voluptuous` 或回退 `clean-lifestyle`。
- 上传服装、电商主图或不要色差时优先 `ecommerce-tryon`，风格词作为画面辅助。
- 多个主风格同时出现时，保留最明确的用户目标；必要时读取 [core/conflict-resolution.md](core/conflict-resolution.md)。

## Route 与 Overlay 组合

主 Route 只能选择一个；气质词需要增强时再读取一个已实现 Overlay。Overlay 不得改变 Route 的构图、核心妆感、主色体系、光线机制或商业目标。

| 主 Route | 推荐兼容 Overlay | 不得覆盖的 Route 指纹 |
| --- | --- | --- |
| `ultra-close-real-face` | `cold-heroine`、`gentle-sister`、`intellectual` | 超近景、真实皮肤微纹理、未修图原片质感 |
| `ancient-lady-dewy-makeup` | `cold-heroine`、`gentle-sister`、`bright-heroine`、`intellectual` | 古风贵女身份、水光显妆、古装与头饰 |
| `black-pearl-dark-gold-ccd` | `cold-heroine`、`cool-mature`、`mature-urban` | 黑珍珠暗部、暗金反光、夜间柔和直闪 |
| `soft-ccd-energetic-voluptuous` | `bright-heroine`、`sweet-cool`、`gentle-sister` | 元气明亮、丰腴自然曲线、亮色柔闪 |
| `cold-white-clear-ccd-curve` | `cold-heroine`、`gentle-sister`、`bright-heroine`、`intellectual` | 冷白高色温、日间清透 CCD、纤细自然曲线 |

如果用户把两个已实现 Route 同时写进 `写真风格`，先按显式主目标和上述复合指纹选一个；另一条只能转译为兼容的气质、配色或局部摄影质感，不能读取第二个 Route 文件。

## 扩展占位

以下方向已经在完整注册表中规划，但对应 Route 文件未创建前不得假装可用：

```text
日系清透、海边生活、居家松弛、韩系氛围、电影情绪、胶片复古、
旗袍东方、宋韵清雅、东方水墨、禅意极简、高级杂志大片、
黑白光影、夜色霓虹、暗调情绪、高曝光梦幻、油画感人像、
珠宝首饰、美妆广告。
```

维护完整注册信息时读取 [references/expanded/style-registry-catalog.md](references/expanded/style-registry-catalog.md)。
