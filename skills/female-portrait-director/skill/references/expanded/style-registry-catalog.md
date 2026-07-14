# skill/style-registry.md

# 女性人像提示词导演 Skill｜风格注册表与路由分流规则

版本编号：`FEMALE-PORTRAIT-DIRECTOR-V1.4.1`
文档类型：风格注册表 / 路由分流规则文档
适用范围：所有女性人像提示词生成、参数组合推荐、提示词优化、图片反推提示词、失败诊断、审查友好改写、Skill 内部 route / overlay / tool 调用
核心职责：根据用户输入识别任务类型、主风格、辅助气质、扩展包、工具模式，并将请求分流到正确文档；本文件只负责注册与分流，不负责具体风格母版扩写

---

## 1. 文档定位

本文件是女性人像提示词导演 Skill 的风格注册表。

它负责解决以下问题：

1. 用户输入风格名称后，系统应该调用哪个 route；
2. 用户没有明确风格时，系统如何根据关键词推导 route；
3. 多个风格同时出现时，哪个是主 route，哪个是 overlay；
4. 核心风格、新增风格、扩展包如何统一注册；
5. 工具类任务，如优化、诊断、审查友好改写、图片反推，如何优先分流；
6. 平台用途是否可以影响 route；
7. 用户参数与 route 默认值冲突时，如何避免覆盖；
8. 后续新增风格时，如何按统一规范接入。

本文件不负责：

```text
1. 不写具体 route 母版；
2. 不写完整提示词；
3. 不扩写服装、场景、五官、身形；
4. 不处理详细安全改写；
5. 不替代 parameter-lock.md；
6. 不替代 director-expansion.md；
7. 不替代 visual-libraries.md；
8. 不替代具体 routes/ 文件。
```

本文件只做一件事：

```text
识别用户输入 → 判断任务类型 → 确定主 route → 叠加 overlay → 调用 tool 或输出模式 → 交给对应文档执行。
```

---

## 2. 继承关系

本文件必须被以下文件继承：

```text
skill/skill.md
skill/core/parameter-lock.md
skill/core/conflict-resolution.md
skill/core/fallback-rules.md
skill/core/reference-image-lock.md
skill/core/output-format.md
skill/references/director-expansion.md
skill/routes/
skill/overlays/
skill/tools/
```

本文件必须继承以下规则：

```text
1. 用户参数锁定规则；
2. 成年边界与安全边界；
3. 冲突处理规则；
4. 参数缺失补全规则；
5. 输出格式规则；
6. 导演式扩写规则；
7. 公共视觉词库规则。
```

---

## 3. 总分流原则

所有分流必须遵守以下原则：

```text
1. 先判断任务类型，再判断风格；
2. 先锁定用户明确参数，再调用 route；
3. 主风格只能有一个；
4. 其他风格词只能作为 overlay、气质或滤镜补充；
5. 平台用途不能覆盖主风格；
6. route 默认值不能覆盖用户明确参数；
7. overlay 不能替代 route；
8. tool 模式优先于普通生成模式；
9. 安全边界优先于所有风格；
10. 最终必须交给 director-expansion.md 做导演式扩写。
```

---

## 4. 分流优先级

当用户输入较复杂时，按以下优先级判断：

```text
P0：安全边界与成年边界
P1：任务类型
P2：用户明确填写的写真风格
P3：用户补充要求
P4：上传图片 / 上传服装 / 图片反推类任务
P5：用户明确填写的场景、服装、气质、五官、身形、镜头、光线
P6：主 route 匹配
P7：overlay 匹配
P8：平台用途适配
P9：fallback 默认补全
P10：输出格式选择
```

说明：

```text
如果用户明确说“优化这条提示词”，先进入工具模式；
如果用户明确说“图片反推”，先进入图片反推模式；
如果用户明确说“保留我的五官 / 保持产品不变 / 不要提示词直接出图”，先进入参考图保留直接生成模式；
如果用户明确说“上传服装生成电商主图”，先进入电商服装试衣 route；
如果用户明确填写写真风格，则该风格优先；
如果用户没有写写真风格，再根据关键词推导 route。
```

---

# 5. 任务类型注册表

系统必须先识别任务类型。

---

## 5.1 普通提示词生成

触发表达：

```text
根据参数生成提示词
帮我生成一条提示词
输出完整提示词
生成可复制提示词
按这个风格写提示词
```

调用：

```text
output-format.md → 完整提示词模式
parameter-lock.md → 锁定参数
style-registry.md → 匹配 route
director-expansion.md → 导演式扩写
```

---

## 5.2 只输出提示词

触发表达：

```text
只输出提示词
不要解释
直接给我最终提示词
输出可复制版即可
```

调用：

```text
output-format.md → 只输出提示词模式
```

内部仍必须执行：

```text
参数锁定
route 匹配
安全边界
冲突处理
导演式扩写
负面约束分离
```

---

## 5.3 参数组合推荐

触发表达：

```text
推荐几组参数
给我几组吸睛组合
来几组爆款组合
多给几组风格组合
不要只限室内
不要全是白色
```

调用：

```text
output-format.md → 参数组合推荐模式
style-registry.md → 可用 route 池
fallback-rules.md → 补全字段
visual-libraries.md → 选择参数元素
```

输出要求：

```text
每组必须是完整可调用参数；
不得只写风格名称；
不得只写摘要；
不得推荐低俗、未成年感、情趣化方向。
```

---

## 5.4 提示词优化

触发表达：

```text
优化这条提示词
帮我改得更稳定
不要改变原参数
这条提示词太机械了
让它更容易出图
```

调用：

```text
tools/prompt-optimize.md
output-format.md → 提示词优化模式
parameter-lock.md → 保留原参数
conflict-resolution.md → 修复冲突
director-expansion.md → 重写为自然画面
```

注意：

```text
优化任务不能擅自换 route；
不能把原来的生活照改成影楼；
不能把原来的服装、场景、五官、身形改掉。
```

---

## 5.5 失败诊断

触发表达：

```text
为什么不稳定
为什么没按参数生成
为什么服装跑偏
为什么脸都一样
为什么色差明显
为什么不像我想要的效果
帮我诊断一下
```

调用：

```text
tools/failure-diagnosis.md
output-format.md → 失败诊断模式
conflict-resolution.md → 检查冲突
fallback-rules.md → 检查默认补全
```

输出必须包含：

```text
主要问题；
影响原因；
修改建议；
修复版提示词；
负面约束。
```

---

## 5.6 审查友好改写

触发表达：

```text
更容易通过审查
改成安全版
降低敏感
不要被拒
保留效果但更合规
审查友好
GPT Image 2 审查友好
```

调用：

```text
tools/safety-rewrite.md
core/safety-boundary.md
output-format.md → 审查友好改写模式
```

注意：

```text
保留用户画面目标；
替换高风险词；
不把画面改成完全无关的保守版本；
不删除用户核心风格和服装。
```

---

## 5.7 图片反推提示词

触发表达：

```text
推导这张图的提示词
反推提示词
分析图片风格
根据图片写 AI 提示词
参考这张图
改成某种风格
```

调用：

```text
tools/image-to-prompt.md
output-format.md → 图片反推提示词模式
style-registry.md → 匹配目标 route
director-expansion.md → 转换为可生成提示词
```

注意：

```text
先分析图像可见信息；
再区分保留项和可改写项；
再输出最终可复制提示词；
不得机械复制参考图；
不得违反成年边界和安全边界。
```

---

## 5.8 电商服装试衣 / 上传服装

触发表达：

```text
上传服装
服装图片
电商主图
服装完整清晰
不要色差
生成模特穿着效果
试衣镜
小红书种草
服装展示图
```

优先调用：

```text
routes/commercial/ecommerce-tryon.md
```

注意：

```text
上传服装优先级最高；
不得改变服装颜色、品类、版型、材质和核心装饰；
不得添加遮挡服装的外套、包袋或复杂动作；
平台用途只影响构图和氛围，不改变服装主体。
```

---

## 5.9 参考图保留直接生成

触发表达：

```text
保留我的五官
用我的自拍
保持产品不变
穿上第二张图里的衣服
不要提示词，直接出图
```

调用：

```text
tools/reference-image-generate.md
core/reference-image-lock.md
output-format.md → 参考图直接生成模式
```

注意：

```text
人物图片必须属于用户本人或已授权成年人物；
图片角色不明确时先询问；
Route 和 Overlay 不得覆盖人物身份或产品核心视觉；
默认直接返回图片，不输出内部提示词。
```

---

# 6. 核心 Route 注册表

V1.4 当前主线包含 5 个核心 route。
这些 route 是基础能力，不在本文件展开母版内容，只做注册。

---

## 6.1 清纯生活照

```yaml
route_id: clean-lifestyle
style_name: 清纯生活照
file: skill/routes/lifestyle/clean-lifestyle.md
category: lifestyle
status: core
```

触发表达：

```text
清纯生活照
清纯女性生活照
电影生活剧照感
咖啡馆生活照
窗边生活照
清纯温柔写真
初恋感生活照
自然生活写真
```

核心识别词：

```text
清纯
温柔
自然
初恋感
咖啡馆
窗边
白色针织
浅色内搭
奶油暖白
生活剧照感
真实生活照
```

默认方向：

```text
年轻成年东方女性；
初恋感淡颜 / 邻家清秀脸；
自然协调身形；
柔和自然光；
奶油暖白生活剧照滤镜。
```

---

## 6.2 纯欲曲线生活照

```yaml
route_id: pure-desire-curve
style_name: 纯欲曲线生活照
file: skill/routes/curve/pure-desire-curve.md
category: curve
status: core
```

触发表达：

```text
纯欲曲线生活照
纯欲生活照
冷白纯欲
清纯脸 × 吸引力身形
身形吸引力
胸腰腿线条
锁骨腰线小腹大腿
电影生活剧照感纯欲风
```

核心识别词：

```text
纯欲
曲线
身形吸引力
清纯脸
冷白
锁骨
腰线
小腹
大腿
贴身吊带
短款上衣
短裤
克制吸引力
```

默认方向：

```text
年轻成年东方女性；
清冷淡颜 / 邻家清秀脸；
自然吸引力曲线；
身形吸引力强度默认为中；
冷白纯欲生活照滤镜；
服装完整，不低俗，不情趣化。
```

---

## 6.3 都市时尚写真

```yaml
route_id: urban-fashion
style_name: 都市时尚写真
file: skill/routes/fashion/urban-fashion.md
category: fashion
status: core
```

触发表达：

```text
都市时尚写真
城市街拍
时尚穿搭写真
高级街拍
都市女性写真
OOTD 写真
```

核心识别词：

```text
都市
时尚
街拍
穿搭
城市
玻璃橱窗
西装
衬衫
风衣
高腰裤
高级感
通勤
```

默认方向：

```text
年轻成年东方女性；
高级淡颜 / 明艳东方脸；
高挑模特比例 / 自然协调身形；
城市自然光；
高级都市街拍滤镜。
```

---

## 6.4 古风仙侠美人图

```yaml
route_id: gufeng-xianxia
style_name: 古风仙侠美人图
file: skill/routes/fantasy/gufeng-xianxia.md
category: fantasy
status: core
```

触发表达：

```text
古风仙侠美人图
古风仙侠女性人像
唐风幻想
古偶女主
东方幻想角色写真
华丽古风美人图
```

核心识别词：

```text
古风
仙侠
唐风
古偶
披帛
大袖衫
发簪
花钿
东方幻想
宫殿回廊
云雾山水
华丽妆造
```

默认方向：

```text
年轻成年东方女性；
精致东方淡颜 / 古典东方美人脸；
华丽唐风幻想古装；
东方梦幻角色写真感；
背景虚化；
服装结构清晰。
```

---

## 6.5 电商服装模特图 / 试衣镜

```yaml
route_id: ecommerce-tryon
style_name: 电商服装模特图
file: skill/routes/commercial/ecommerce-tryon.md
category: commercial
status: core
```

触发表达：

```text
电商服装图
电商主图
上传服装生成模特
AI 试衣镜
服装展示图
小红书服装种草
服装完整清晰
不要色差
```

核心识别词：

```text
上传服装
服装图片
主图
详情页
服装完整
不要色差
版型
材质
商品展示
模特穿着
试衣
电商
```

默认方向：

```text
用户上传服装最高优先；
服装颜色、品类、版型、材质、装饰必须严格保留；
模特只是服装展示载体；
背景简洁；
服装完整清晰；
不得遮挡服装主体。
```

---

# 7. 新增 9 个重点 Route 注册表

以下 9 个 route 是 V1.4 新增重点风格，已纳入主线注册。

---

## 7.1 复古港风写真

```yaml
route_id: retro-hongkong
style_name: 复古港风写真
file: skill/routes/lifestyle/retro-hongkong.md
category: lifestyle
status: priority
```

触发表达：

```text
复古港风写真
港风写真
复古港风
港片女主
旧香港风
霓虹港风
明艳港风
```

核心识别词：

```text
港风
复古港风
港片
港片女主
旧香港
老香港
霓虹
红唇
卷发
胶片
旧街
茶餐厅
老式旅馆
明艳
浓颜
电影感
年代感
暖棕
低饱和
```

默认方向：

```text
年轻成年东方女性；
复古港风脸 / 明艳东方浓颜；
复古、明艳、电影感、有故事感；
旧街道、霓虹街角、老式茶餐厅、复古旅馆；
胶片港风复古滤镜。
```

边界提醒：

```text
不是普通霓虹夜景；
不是廉价夜店风；
不是赛博朋克；
不是网红港风模板。
```

---

## 7.2 法式慵懒写真

```yaml
route_id: french-lazy
style_name: 法式慵懒写真
file: skill/routes/lifestyle/french-lazy.md
category: lifestyle
status: priority
```

触发表达：

```text
法式慵懒写真
法式写真
法式生活照
法式窗边写真
法式松弛感写真
法式公寓生活照
```

核心识别词：

```text
法式
慵懒
松弛
松弛感
奶油暖白
窗边
针织
白衬衫
咖啡
公寓
阳台
自然卷发
轻熟温柔
随性
浅色空间
生活剧照
```

默认方向：

```text
年轻成年东方女性；
松弛淡颜 / 温柔成熟脸；
自然协调身形 / 轻熟柔和曲线；
窗边公寓、咖啡馆、浅色卧室、法式阳台；
奶油暖白法式生活滤镜。
```

边界提醒：

```text
不是低俗私房；
不是凌乱居家照；
不是强影楼摆拍；
不是普通清纯生活照。
```

---

## 7.3 新中式东方写真

```yaml
route_id: new-chinese
style_name: 新中式东方写真
file: skill/routes/oriental/new-chinese.md
category: oriental
status: priority
```

触发表达：

```text
新中式东方写真
新中式写真
东方写真
东方美学写真
中式极简人像
东方留白写真
茶室东方写真
```

核心识别词：

```text
新中式
东方
东方美学
留白
茶室
屏风
竹影
木质空间
素色
克制
禅意
东方气质
中式
宣纸白
低饱和
清雅
盘扣
棉麻
书卷气
东方文艺
```

默认方向：

```text
年轻成年东方女性；
古典东方美人脸 / 清冷淡颜；
自然协调身形 / 纤细清瘦身形；
茶室、屏风、木质空间、竹影窗边、东方庭院；
东方留白柔和滤镜。
```

边界提醒：

```text
不是古风仙侠；
不是廉价影楼中式；
不是中式符号堆砌；
不是热闹喜庆风。
```

---

## 7.4 活力运动写真

```yaml
route_id: sporty-active
style_name: 活力运动写真
file: skill/routes/fashion/sporty-active.md
category: fashion
status: priority
```

触发表达：

```text
活力运动写真
运动写真
运动风写真
元气运动写真
户外运动写真
运动穿搭写真
健康线条写真
```

核心识别词：

```text
运动
活力
元气
健身
户外
球场
跑步
瑜伽
网球
运动背心
运动短裤
运动鞋
健康线条
阳光
清爽
跑道
运动场
```

默认方向：

```text
年轻成年东方女性；
甜美元气脸 / 健康自然脸；
健康运动线条；
肩背、腰腹、腿部自然运动线条；
户外运动场、城市跑道、网球场、健身房简洁背景；
明亮清爽运动滤镜。
```

边界提醒：

```text
不是低俗健身照；
不是身体部位凝视；
不是幼态元气；
不是复杂运动动作堆叠。
```

---

## 7.5 旅行假日写真

```yaml
route_id: travel-vacation
style_name: 旅行假日写真
file: skill/routes/lifestyle/travel-vacation.md
category: lifestyle
status: priority
```

触发表达：

```text
旅行假日写真
旅行写真
假日写真
旅行生活照
度假生活照
旅拍写真
海岛度假写真
城市旅行写真
酒店阳台生活照
民宿旅拍写真
```

核心识别词：

```text
旅行
假日
度假
旅拍
海岛
城市旅行
酒店
民宿
阳台
街边
旅行街区
海边街道
行李箱
草编包
阳光
假日感
松弛感
海风
度假衬衫
旅行穿搭
```

默认方向：

```text
年轻成年东方女性；
邻家清秀脸 / 明朗自然脸；
自然协调身形；
海边街道、旅行民宿、酒店阳台、城市巷口、海岛步道；
明亮假日自然滤镜。
```

边界提醒：

```text
不是普通游客照；
不是廉价旅拍模板；
不是低俗度假照；
不是背景景点抢人物。
```

---

## 7.6 影楼精修写真

```yaml
route_id: studio-retouched
style_name: 影楼精修写真
file: skill/routes/fashion/studio-retouched.md
category: fashion
status: priority
```

触发表达：

```text
影楼精修写真
影楼写真
精修写真
棚拍写真
高级精修
商业棚拍
写真馆风格
社交头像写真
```

核心识别词：

```text
影楼
精修
棚拍
商业写真
高级写真
写真馆
背景布
柔光箱
修图感
干净棚拍
人像大片
高级人像
精致妆发
棚拍柔光
社交头像
作品集
```

默认方向：

```text
年轻成年东方女性；
精致东方淡颜 / 明艳东方脸；
自然协调身形 / 高挑模特比例；
干净棚拍背景、背景布、柔光摄影棚、极简室内空间；
高级棚拍精修滤镜。
```

边界提醒：

```text
不是廉价影楼照；
不是塑料磨皮；
不是低俗私房；
不是普通电商模特图。
```

---

## 7.7 东方丰腴写真

```yaml
route_id: oriental-voluptuous
style_name: 东方丰腴写真
file: skill/routes/curve/oriental-voluptuous.md
category: curve
status: priority
```

触发表达：

```text
东方丰腴写真
丰腴写真
东方丰润写真
丰润曲线写真
东方古典曲线
成熟柔润写真
旗袍丰腴写真
唐风丰润美人
```

核心识别词：

```text
东方丰腴
丰腴
丰润
柔润
饱满但自然
成熟曲线
东方古典身形
丰润曲线
柔美曲线
旗袍曲线
唐风丰润
圆润柔美
轻熟丰润
衣料垂坠
腰线
肩颈
胸腰比例
```

默认方向：

```text
年轻成年东方女性；
古典东方美人脸 / 温柔丰润脸；
丰腴曲线 / 东方丰腴；
成熟、柔美、丰润、古典、温和、端庄；
东方柔润精修滤镜。
```

边界提醒：

```text
不是低俗性感；
不是夸张身材；
不是身体部位特写；
不是情趣化丰腴。
```

---

## 7.8 清冷仙气古风增强版

```yaml
route_id: cold-xianxia-enhanced
style_name: 清冷仙气古风增强版
file: skill/routes/fantasy/cold-xianxia-enhanced.md
category: fantasy
status: priority
```

触发表达：

```text
清冷仙气古风增强版
清冷仙气古风
清冷古风女主写真
清冷古偶女主
冷白古风写真
冷调宫灯回廊古风
冷白仙侠美人图
```

核心识别词：

```text
清冷
仙气
冷白
疏离
克制
空灵
冷调
古风
仙侠
古偶女主
唐风
披帛
大袖衫
宫灯回廊
月白
冰蓝
雾灰
冷金
珍珠
银色头饰
冷白柔光
侧逆光
距离感
```

默认方向：

```text
年轻成年东方女性；
清冷淡颜 / 古典东方美人脸；
纤细清瘦身形 / 自然协调身形；
冷调宫灯回廊、月白庭院、雾气山水；
清冷仙气古风滤镜。
```

边界提醒：

```text
不是甜美仙女；
不是喜庆红金；
不是暖热古风；
不是廉价 cosplay。
```

---

## 7.9 明媚华贵古风增强版

```yaml
route_id: bright-luxury-gufeng
style_name: 明媚华贵古风增强版
file: skill/routes/fantasy/bright-luxury-gufeng.md
category: fantasy
status: priority
```

触发表达：

```text
明媚华贵古风增强版
明媚华贵古风
盛唐华贵女主
红金唐风华服
古偶女主华丽写真
东方宫廷女主人像
唐风幻想华丽角色海报
```

核心识别词：

```text
明媚
华贵
明艳
盛唐
红金
宫廷
古偶女主
女主感
唐风
华服
大袖衫
披帛
重工头饰
珠宝
金色刺绣
华丽妆造
东方幻想
高贵
盛大
高级古风
```

默认方向：

```text
年轻成年东方女性；
明艳东方浓颜 / 古典东方美人脸；
自然协调身形 / 丰润华贵曲线；
华丽宫廷回廊、唐风殿前、金色帷幔背景；
明亮华贵古风滤镜。
```

边界提醒：

```text
不是清冷疏离；
不是廉价红金婚庆；
不是现代礼服；
不是颜色杂乱的古风堆砌。
```

---

# 8. 扩展 Route 注册表

以下 route 属于 V1.4 扩展包，可作为后续新增文件或独立扩展模块接入。
如果对应文件尚未创建，系统可先作为注册占位，但不得虚构完整 route 文件内容。

---

## 8.1 日系清透写真

```yaml
route_id: japanese-daily
style_name: 日系清透写真
file: skill/routes/lifestyle/japanese-daily.md
category: lifestyle
status: extension
```

触发词：

```text
日系
清透
空气感
日杂
白衬衫
自然光
街边
便利店
清爽
淡颜
```

默认方向：

```text
邻家清秀脸 / 初恋感淡颜；
自然协调身形；
清透空气感滤镜；
柔和自然光；
场景干净，情绪轻。
```

---

## 8.2 海边生活写真

```yaml
route_id: seaside-vacation
style_name: 海边生活写真
file: skill/routes/lifestyle/seaside-vacation.md
category: lifestyle
status: extension
```

触发词：

```text
海边
海风
海边步道
礁石
沙滩
远处海面
沿海
傍晚海边
海边生活照
```

默认方向：

```text
邻家清秀脸 / 清冷淡颜；
自然协调身形或自然吸引力曲线；
海边步道、礁石、远处海面；
明亮假日自然滤镜或冷白生活照滤镜。
```

---

## 8.3 居家松弛感写真

```yaml
route_id: home-relaxed
style_name: 居家松弛感写真
file: skill/routes/lifestyle/home-relaxed.md
category: lifestyle
status: extension
```

触发词：

```text
居家
卧室
沙发
床边
松弛感
家里
白色床品
窗边卧室
生活感
```

默认方向：

```text
温柔淡颜 / 邻家清秀脸；
自然协调身形；
奶油暖白生活剧照滤镜；
姿态自然稳定，不私房化。
```

---

## 8.4 韩系氛围写真

```yaml
route_id: korean-mood
style_name: 韩系氛围写真
file: skill/routes/lifestyle/korean-mood.md
category: lifestyle
status: extension
```

触发词：

```text
韩系
氛围感
低饱和
韩系淡颜
室内自然光
温柔
清透
```

默认方向：

```text
韩系淡颜 / 精致自然脸；
低饱和韩系氛围滤镜；
柔和室内自然光；
服装简洁。
```

---

## 8.5 电影情绪写真

```yaml
route_id: cinematic-mood
style_name: 电影情绪写真
file: skill/routes/lifestyle/cinematic-mood.md
category: lifestyle
status: extension
```

触发词：

```text
电影感
情绪感
雨天
车内
窗边
故事感
暗调
生活电影
电影故事脸
```

默认方向：

```text
电影故事脸 / 清冷淡颜；
真实电影生活剧照滤镜；
一个清晰主事件；
表情克制，有故事感。
```

---

## 8.6 胶片复古写真

```yaml
route_id: film-retro
style_name: 胶片复古写真
file: skill/routes/lifestyle/film-retro.md
category: lifestyle
status: extension
```

触发词：

```text
胶片
复古
旧照片
暖棕
颗粒感
低饱和
老房间
旧旅馆
```

默认方向：

```text
文艺淡颜 / 复古自然脸；
低饱和胶片复古滤镜；
暖调侧光；
人物情绪自然。
```

---

## 8.7 旗袍东方写真

```yaml
route_id: qipao-oriental
style_name: 旗袍东方写真
file: skill/routes/oriental/qipao-oriental.md
category: oriental
status: extension
```

触发词：

```text
旗袍
旧上海
东方曲线
盘扣
丝绒旗袍
复古旗袍
老洋房
```

默认方向：

```text
古典东方美人脸 / 复古港风脸；
旗袍版型合体但端庄；
开衩克制；
东方复古柔光滤镜。
```

---

## 8.8 宋韵清雅写真

```yaml
route_id: song-elegant
style_name: 宋韵清雅写真
file: skill/routes/oriental/song-elegant.md
category: oriental
status: extension
```

触发词：

```text
宋韵
宋制
清雅
淡雅
素色
文人气
东方留白
```

默认方向：

```text
古典东方美人脸 / 清冷淡颜；
素色服装；
宋韵清雅留白滤镜；
克制、清雅、留白。
```

---

## 8.9 东方水墨写真

```yaml
route_id: ink-oriental
style_name: 东方水墨写真
file: skill/routes/oriental/ink-oriental.md
category: oriental
status: extension
```

触发词：

```text
水墨
墨色
宣纸
东方水墨
黑白灰
留白
山水
```

默认方向：

```text
清冷淡颜 / 古典东方美人脸；
水墨留白东方滤镜；
大面积留白；
低饱和黑白灰。
```

---

## 8.10 禅意极简写真

```yaml
route_id: zen-minimal
style_name: 禅意极简写真
file: skill/routes/oriental/zen-minimal.md
category: oriental
status: extension
```

触发词：

```text
禅意
极简
留白
素色
安静
冥想
东方极简
```

默认方向：

```text
清冷淡颜 / 古典东方美人脸；
大面积留白；
元素极少；
禅意极简柔和滤镜。
```

---

## 8.11 高级杂志大片写真

```yaml
route_id: editorial
style_name: 高级杂志大片写真
file: skill/routes/fashion/editorial.md
category: fashion
status: extension
```

触发词：

```text
杂志大片
editorial
封面
高级时尚
大片感
强构图
时尚摄影
```

默认方向：

```text
高级厌世脸 / 明艳东方脸；
高挑模特比例；
高级杂志大片滤镜；
构图更强，光影更有张力。
```

---

## 8.12 黑白光影写真

```yaml
route_id: black-white-light
style_name: 黑白光影写真
file: skill/routes/fashion/black-white-light.md
category: fashion
status: extension
```

触发词：

```text
黑白
黑白光影
伦勃朗光
高反差
高级肖像
经典黑白
```

默认方向：

```text
高级淡颜 / 高级厌世脸；
黑白高级光影滤镜；
高反差侧光；
脸部轮廓清楚。
```

---

## 8.13 夜色霓虹写真

```yaml
route_id: neon-night
style_name: 夜色霓虹写真
file: skill/routes/fashion/neon-night.md
category: fashion
status: extension
```

触发词：

```text
夜色
霓虹
城市夜景
雨夜街头
冷艳
夜景人像
```

默认方向：

```text
冷艳都市脸 / 高级厌世脸；
夜色霓虹电影滤镜；
背景虚化；
不得低俗夜店化。
```

---

## 8.14 暗调情绪写真

```yaml
route_id: dark-mood
style_name: 暗调情绪写真
file: skill/routes/fashion/dark-mood.md
category: fashion
status: extension
```

触发词：

```text
暗调
情绪
低照度
黑色背景
冷感
忧郁
暗光
```

默认方向：

```text
电影故事脸 / 清冷淡颜；
暗调电影情绪滤镜；
低照度侧光；
暗部保留层次。
```

---

## 8.15 高曝光梦幻写真

```yaml
route_id: high-key-dream
style_name: 高曝光梦幻写真
file: skill/routes/fashion/high-key-dream.md
category: fashion
status: extension
```

触发词：

```text
高曝光
梦幻
柔雾
白色空间
朦胧
浅色
过曝边缘
```

默认方向：

```text
初恋感淡颜 / 清冷淡颜；
高曝光柔雾梦幻滤镜；
五官必须清晰；
避免糊脸。
```

---

## 8.16 油画感人像写真

```yaml
route_id: oil-painting-portrait
style_name: 油画感人像写真
file: skill/routes/fashion/oil-painting-portrait.md
category: fashion
status: extension
```

触发词：

```text
油画感
古典
油画光
复古人像
古典肖像
柔和侧光
```

默认方向：

```text
古典东方美人脸 / 高级淡颜；
油画质感人像滤镜；
古典柔光；
不可变成插画风，除非用户明确要求。
```

---

## 8.17 珠宝首饰写真

```yaml
route_id: jewelry-portrait
style_name: 珠宝首饰写真
file: skill/routes/commercial/jewelry-portrait.md
category: commercial
status: extension
```

触发词：

```text
珠宝
首饰
耳饰
项链
戒指
美妆广告
珠宝广告
肩颈近景
```

默认方向：

```text
精致东方淡颜；
肩颈近景 / 半身近景；
珠宝高光；
商品不能被头发或手遮挡。
```

---

## 8.18 美妆广告写真

```yaml
route_id: beauty-campaign
style_name: 美妆广告写真
file: skill/routes/commercial/beauty-campaign.md
category: commercial
status: extension
```

触发词：

```text
美妆
口红
护肤
底妆
广告
妆容展示
面部近景
品牌广告
```

默认方向：

```text
精致东方淡颜 / 明艳东方脸；
面部近景或肩颈近景；
妆容清晰；
产品或妆效是重点。
```

---

# 9. Overlay 注册表

Overlay 是气质增强模块，不是主 route。
Overlay 只能改变气质、表情、姿态、局部穿搭和色彩倾向，不能替代主风格。

---

## 9.1 清冷女主增强

```yaml
overlay_id: cold-heroine
style_name: 清冷女主增强
file: skill/overlays/cold-heroine.md
```

触发词：

```text
清冷
疏离
冷白
克制
高冷
距离感
冷感女主
```

作用范围：

```text
降低甜度；
增强冷白、疏离、克制；
眼神更安静；
色温略偏冷；
姿态更端正。
```

不得：

```text
把所有 route 改成清冷古风；
覆盖用户主风格；
删除原本场景和服装。
```

---

## 9.2 明艳女主增强

```yaml
overlay_id: bright-heroine
style_name: 明艳女主增强
file: skill/overlays/bright-heroine.md
```

触发词：

```text
明艳
女主感
有气场
视觉冲击
明媚
贵气
高存在感
```

作用范围：

```text
增强五官存在感；
增强眼神张力；
妆容更完整；
人物主体更有记忆点。
```

不得：

```text
把清冷路线改成热烈；
把生活照改成古风；
把自然妆改成廉价浓妆。
```

---

## 9.3 温柔姐姐感增强

```yaml
overlay_id: gentle-sister
style_name: 温柔姐姐感增强
file: skill/overlays/gentle-sister.md
```

触发词：

```text
温柔姐姐
成熟温柔
亲近
柔和
姐姐感
温柔成熟
```

作用范围：

```text
表情更柔和；
眼神更亲近；
气质更成熟稳定；
姿态更自然。
```

不得：

```text
变老气；
变低俗；
变成私房感。
```

---

## 9.4 冷艳御姐增强

```yaml
overlay_id: cool-mature
style_name: 冷艳御姐增强
file: skill/overlays/cool-mature.md
```

触发词：

```text
冷艳
御姐
成熟有气场
强气场
冷感成熟
高级冷淡
```

作用范围：

```text
眼神更冷静；
表情更克制；
穿搭更简洁；
姿态更有控制力。
```

不得：

```text
覆盖主风格；
把生活照改成夜店照；
把清纯脸改成欧美浓颜。
```

---

## 9.5 高智感知识女性增强

```yaml
overlay_id: intellectual
style_name: 高智感知识女性增强
file: skill/overlays/intellectual.md
```

触发词：

```text
高智感
知识女性
知性
书卷气
理性
冷静
专业
聪明感
```

作用范围：

```text
眼神更理性；
姿态更端正；
服装更简洁；
可加入书页、桌面、窗边、东方留白等细节。
```

不得：

```text
把所有风格改成办公室职业照；
破坏古风或东方风格；
过度商务化。
```

---

## 9.6 甜酷年轻女性增强

```yaml
overlay_id: sweet-cool
style_name: 甜酷年轻女性增强
file: skill/overlays/sweet-cool.md
```

触发词：

```text
甜酷
个性
年轻活力
街头感
甜酷少女感
```

作用范围：

```text
增强街头感；
穿搭更个性；
姿态更明朗；
可增强运动鞋、短外套、棒球帽等元素。
```

安全修正：

```text
必须明确成年；
不得幼态化；
不得校园未成年感。
```

---

## 9.7 轻熟都市女性增强

```yaml
overlay_id: mature-urban
style_name: 轻熟都市女性增强
file: skill/overlays/mature-urban.md
```

触发词：

```text
轻熟
都市女性
成熟但不老气
女人味
优雅
通勤感
```

作用范围：

```text
增强优雅、成熟、都市感；
服装更简洁有质感；
姿态更稳定；
气质更柔和成熟。
```

不得：

```text
低俗化；
把生活照改成商业硬广；
把古风改成现代都市。
```

---

# 10. 工具模块注册表

工具模块不是风格 route，而是处理任务流程。

---

## 10.1 提示词优化工具

```yaml
tool_id: prompt-optimize
tool_name: 提示词优化
file: skill/tools/prompt-optimize.md
```

触发词：

```text
优化
改得更稳定
重新整理
不要机械
更容易出图
不改参数
```

职责：

```text
保留原参数；
修复提示词结构；
增强导演式扩写；
补充负面约束；
不改变主 route。
```

---

## 10.2 失败诊断工具

```yaml
tool_id: failure-diagnosis
tool_name: 失败诊断
file: skill/tools/failure-diagnosis.md
```

触发词：

```text
为什么不稳定
为什么跑偏
为什么不像
为什么没按参数
为什么脸一样
色差明显
服装不对
```

职责：

```text
找出失败原因；
判断是参数冲突、route 错误、服装不清、镜头冲突、光线冲突还是负面约束不足；
输出修复方案。
```

---

## 10.3 审查友好改写工具

```yaml
tool_id: safety-rewrite
tool_name: 审查友好改写
file: skill/tools/safety-rewrite.md
```

触发词：

```text
审查友好
安全版
降低敏感
不要被拒
合规
保留效果但更安全
```

职责：

```text
保留原画面目标；
替换高风险词；
强化成年边界；
强化服装完整和非低俗表达；
输出安全版提示词。
```

---

## 10.4 图片反推提示词工具

```yaml
tool_id: image-to-prompt
tool_name: 图片反推提示词
file: skill/tools/image-to-prompt.md
```

触发词：

```text
反推
推导
分析图片
根据图片写提示词
参考这张图
改成某风格
```

职责：

```text
分析主体、服装、光线、构图、色彩、风格；
提取可复用元素；
根据用户修改要求选择 route；
输出可复制提示词。
```

---

## 10.5 参数组合推荐工具

```yaml
tool_id: parameter-recommend
tool_name: 参数组合推荐
file: skill/tools/parameter-recommend.md
```

触发词：

```text
推荐几组
组合
爆款参数
吸睛参数
多给几组
不同场景
不同服装
```

职责：

```text
根据用户指定风格或主题生成多组完整可调用参数；
保证场景、服装、五官、光线、镜头有差异；
不得只写摘要。
```

---

## 10.6 参考图保留直接生成工具

```yaml
tool_id: reference-image-generate
tool_name: 参考图保留直接生成
file: skill/tools/reference-image-generate.md
```

触发词：

```text
保留我的五官
用我的自拍
保持产品不变
穿上第二张图里的衣服
不要提示词直接出图
```

职责：

```text
区分人物、产品、风格参考和待编辑图片；
锁定授权人物五官身份或产品核心视觉；
编排 Route、Overlay、导演模式和图片生成能力；
默认直接返回图片；
保真失败时明确说明限制。
```

---

# 11. 主 Route 判断规则

---

## 11.1 用户明确填写写真风格

如果用户输入结构中有：

```text
写真风格：xxx
风格：xxx
style：xxx
```

则优先根据该字段匹配主 route。

示例：

```text
写真风格：法式慵懒写真
场景方向：窗边公寓
服装方向：黑色针织背心 + 高腰牛仔裤
```

调用：

```text
route_id: french-lazy
```

不得因为“黑色针织背心”自动改成都市时尚。

---

## 11.2 用户没有填写写真风格

根据关键词综合判断：

```text
场景关键词
服装关键词
气质关键词
身形关键词
平台用途
补充要求
```

示例：

```text
海边街道，白色开衫，草编包，假日感，小红书
```

调用：

```text
route_id: travel-vacation
```

示例：

```text
冷调宫灯回廊，唐风大袖衫，披帛，清冷，冷白
```

调用：

```text
route_id: cold-xianxia-enhanced
```

---

## 11.3 多个 route 同时命中

必须选择一个主 route。

判断顺序：

```text
1. 用户明确写的写真风格；
2. 用户补充要求中强调的主目标；
3. 服装和场景最强匹配的 route；
4. 身形目标是否强到需要 curve route；
5. 平台用途是否是电商主图；
6. 若仍不明确，选择更具体的 route。
```

示例：

```text
旅行假日写真，海边民宿，纯欲，锁骨腰线大腿
```

处理：

```text
如果“旅行假日写真”是明确字段 → 主 route 为 travel-vacation；
纯欲和线条作为克制身形表达 overlay；
如果用户没有写旅行假日，只强调纯欲曲线 → route 为 pure-desire-curve。
```

---

## 11.4 平台用途不能当主 route

示例：

```text
复古港风写真，平台用途：小红书
```

正确：

```text
主 route：retro-hongkong
平台用途：小红书适配
```

错误：

```text
主 route 改成普通小红书清新风。
```

---

## 11.5 电商任务的特殊优先级

只要出现以下组合，优先判断为电商任务：

```text
上传服装 + 服装完整清晰
上传服装 + 不要色差
电商主图 + 服装展示
商品图 + 模特穿着
试衣镜 + 上传服装
```

调用：

```text
route_id: ecommerce-tryon
```

其他风格只能作为场景或模特气质参考。

示例：

```text
上传服装，女模，复古港风，小红书种草，不要色差
```

处理：

```text
主 route：ecommerce-tryon
风格参考：retro-hongkong
平台用途：小红书种草
服装还原优先于港风氛围。
```

---

# 12. 冲突分流规则

详细冲突处理交给：

```text
skill/core/conflict-resolution.md
```

本文件只负责初步判断。

---

## 12.1 清冷 + 红金

如果用户写：

```text
清冷古风
白红金
不要喜庆
整体冷白
```

调用：

```text
route_id: cold-xianxia-enhanced
```

处理：

```text
红色小面积辅色；
金色细节高光；
主体冷白；
避免喜庆感。
```

---

## 12.2 红金 + 明媚华贵

如果用户写：

```text
红金
明媚
华贵
盛唐
女主感
```

调用：

```text
route_id: bright-luxury-gufeng
```

---

## 12.3 新中式 + 仙气

如果用户写：

```text
新中式
茶室
留白
仙气
```

调用：

```text
route_id: new-chinese
```

处理：

```text
“仙气”转译为空灵、清雅、轻盈；
不自动改成古风仙侠。
```

---

## 12.4 古风 + 茶室

如果用户写：

```text
古风仙侠
唐风披帛
茶室
```

判断：

```text
如果唐风披帛、古偶女主、仙侠更强 → gufeng-xianxia；
如果茶室、新中式、盘扣、现代东方更强 → new-chinese。
```

---

## 12.5 纯欲 + 不低俗

如果用户写：

```text
纯欲曲线
身形吸引力
不要低俗
```

调用：

```text
route_id: pure-desire-curve
```

处理：

```text
服装完整；
表达克制；
线条通过姿态、服装版型、光线和构图呈现；
禁止低俗身体凝视。
```

---

## 12.6 丰腴 + 古风

如果用户写：

```text
东方丰腴
唐风
古典曲线
```

调用：

```text
route_id: oriental-voluptuous
```

如果用户写：

```text
明媚华贵古风
东方丰腴
```

调用：

```text
route_id: bright-luxury-gufeng
overlay: 东方丰腴增强
```

---

# 13. 默认补全调用规则

当 route 确定后，缺失字段由以下文件补全：

```text
skill/core/fallback-rules.md
```

补全顺序：

```text
1. 写真风格；
2. 场景方向；
3. 服装方向；
4. 气质标签；
5. 五官方向；
6. 身形方向；
7. 身形吸引力强度；
8. 线条重点；
9. 镜头方向；
10. 光线氛围；
11. 滤镜效果；
12. 画幅比例；
13. 平台用途；
14. 补充要求；
15. 负面约束。
```

补全不得：

```text
覆盖用户参数；
引入新的风格冲突；
默认性感或强曲线；
默认古风；
默认电商；
默认复杂背景。
```

---

# 14. 输出模式选择规则

route 和 overlay 确定后，输出模式由 `output-format.md` 决定。

---

## 14.1 默认完整提示词模式

适用：

```text
用户给参数，让系统生成提示词。
```

输出：

```text
一、参数锁定结果
二、最终提示词
三、负面约束
```

---

## 14.2 用户要求只输出提示词

输出：

```text
【最终提示词】
【负面约束】
```

不得写分析。

---

## 14.3 用户要求推荐参数

输出多组完整参数组合。

---

## 14.4 用户要求 Skill 文档

一次只输出一个文档。

格式：

```text
# 文件路径 / 文件名
# 文档标题
版本编号：
文档类型：
适用范围：
核心职责：
正文……
```

---

# 15. 注册新增 route 的规范

后续新增 route 时，必须在本文件登记以下字段：

```yaml
route_id:
style_name:
file:
category:
status:
trigger_names:
trigger_keywords:
default_face:
default_body:
default_scene:
default_clothing:
default_lighting:
default_filter:
boundary_notes:
```

新增 route 必须满足：

```text
1. 有明确风格定位；
2. 与现有 route 边界清晰；
3. 有默认五官；
4. 有默认身形；
5. 有默认场景；
6. 有默认服装；
7. 有默认光线和滤镜；
8. 有安全边界；
9. 有负面约束；
10. 能被 director-expansion.md 扩写成自然画面。
```

不得新增：

```text
只有风格名称、没有母版的 route；
与现有 route 高度重复的 route；
只靠性感、暴露、低俗吸引的 route；
无法稳定出图的复杂混合 route；
没有成年边界的 route。
```

---

# 16. Route ID 命名规范

route_id 必须使用英文小写短横线：

```text
clean-lifestyle
pure-desire-curve
retro-hongkong
french-lazy
new-chinese
sporty-active
travel-vacation
studio-retouched
oriental-voluptuous
cold-xianxia-enhanced
bright-luxury-gufeng
```

不得使用：

```text
中文 route_id；
空格；
下划线；
大小写混合；
版本号混入 route_id；
临时命名。
```

---

# 17. 分类规范

当前分类包括：

```text
lifestyle：生活照 / 旅行 / 居家 / 咖啡馆 / 电影生活感
fashion：都市 / 影楼 / 杂志 / 运动 / 夜景 / 光影
fantasy：古风 / 仙侠 / 东方幻想 / 角色写真
oriental：新中式 / 旗袍 / 宋韵 / 水墨 / 禅意
curve：身形曲线 / 纯欲 / 丰腴 / 健康线条
commercial：电商 / 美妆 / 珠宝 / 品牌广告
```

新增 route 必须归入其中一个分类。

---

# 18. 自检规则

每次 route 分流前必须检查：

```text
1. 用户是否明确填写写真风格？
2. 用户是否要求工具任务，而不是普通生成？
3. 是否有上传图片或上传服装？
4. 是否涉及电商服装还原？
5. 是否有主 route 和 overlay 混淆？
6. 是否把平台用途误当成主风格？
7. 是否有多个 route 同时命中？
8. 是否选择了最具体的 route？
9. 是否保留用户明确参数？
10. 是否需要调用 conflict-resolution.md？
11. 是否需要调用 safety-boundary.md？
12. 是否需要 fallback-rules.md 补全？
13. 输出格式是否由 output-format.md 决定？
```

任一项不合格，必须重新判断。

---

# 19. 分流示例

---

## 示例 1：法式慵懒

用户输入：

```text
写真风格：法式慵懒写真
场景方向：窗边公寓
服装方向：黑色针织背心 + 高腰牛仔裤
气质标签：轻熟、松弛、自然
```

分流结果：

```yaml
task_type: prompt_generation
main_route: french-lazy
overlay: mature-urban 可选
output_mode: full_prompt
```

---

## 示例 2：清冷红金古风

用户输入：

```text
写真风格：清冷古风女主写真
场景方向：冷调宫灯回廊
服装方向：华丽唐风大袖衫 + 披帛
配色方向：白红金系
补充要求：红色只作为辅色点缀，整体冷白，不要喜庆
```

分流结果：

```yaml
task_type: prompt_generation
main_route: cold-xianxia-enhanced
overlay: none
conflict_rule: 清冷 + 红金降冲突
output_mode: full_prompt
```

---

## 示例 3：明媚红金古风

用户输入：

```text
写真风格：明媚华贵古风增强版
服装方向：红金唐风华服
气质标签：明艳、华贵、女主感
```

分流结果：

```yaml
task_type: prompt_generation
main_route: bright-luxury-gufeng
overlay: bright-heroine 内置
output_mode: full_prompt
```

---

## 示例 4：上传服装 + 港风

用户输入：

```text
上传服装图片：已上传
模特类型：女模
模特气质：复古港风
平台用途：小红书种草
补充要求：服装完整清晰，不要色差
```

分流结果：

```yaml
task_type: ecommerce_tryon
main_route: ecommerce-tryon
style_reference: retro-hongkong
output_mode: full_prompt
priority: clothing_accuracy
```

---

## 示例 5：东方丰腴

用户输入：

```text
写真风格：东方丰腴写真
场景方向：新中式室内
服装方向：合体但端庄的旗袍
身形方向：东方丰腴
补充要求：丰腴但不要低俗
```

分流结果：

```yaml
task_type: prompt_generation
main_route: oriental-voluptuous
overlay: none
output_mode: full_prompt
safety_focus: mature_soft_curve_not_vulgar
```

---

## 示例 6：推荐参数组合

用户输入：

```text
基于纯欲曲线生活照，推荐几组比较吸睛的参数组合，不要只限室内。
```

分流结果：

```yaml
task_type: parameter_recommendation
main_route: pure-desire-curve
tool: parameter-recommend
output_mode: parameter_combo
```

---

## 示例 7：优化提示词

用户输入：

```text
这条提示词太机械了，帮我改得更稳定，不要改变原参数。
```

分流结果：

```yaml
task_type: prompt_optimize
tool: prompt-optimize
main_route: infer_from_original_prompt
output_mode: optimize
```

---

# 20. 最终原则

本注册表的最终原则是：

```text
风格注册表只负责识别和分流；
具体画面由 route 决定；
细节素材由 visual-libraries.md 提供；
自然扩写由 director-expansion.md 完成；
参数保护由 parameter-lock.md 保证；
冲突处理由 conflict-resolution.md 完成；
缺失补全由 fallback-rules.md 完成；
输出结构由 output-format.md 统一。
```

判断分流是否成功的标准：

```text
主 route 明确；
overlay 不抢主风格；
用户参数未被覆盖；
平台用途没有误导风格；
电商任务服装还原优先；
安全边界没有丢失；
最终可以进入导演式扩写，而不是停留在字段匹配。
```

所有模块必须遵守本文件。
