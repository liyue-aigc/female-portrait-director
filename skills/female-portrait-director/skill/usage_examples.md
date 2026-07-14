# 调用示例

下面是部分内置风格的简短调用入口。V1.4.1 会通过风格注册表按需加载单一路由，锁定参数或授权参考图主体，完整扩写年龄、五官、身形、服装、姿态、场景、镜头、光线和滤镜，再将这些模块融合为摄影导演式提示词或直接生成图片。当前已实现 14 条 Route，完整列表见 [style-registry.md](style-registry.md)，详细示例位于 `examples/`。

## 清纯生活照

```text
风格：清纯生活照
场景：咖啡馆靠窗座位
服装：白色针织开衫 + 浅色内搭
气质：清纯温柔
画幅：9:16
```

完整示例：[clean_lifestyle_examples.md](../examples/clean_lifestyle_examples.md)

## 纯欲曲线生活照

```text
风格：纯欲曲线生活照
场景：海边步道
服装：雾蓝色短款吊带 + 白色轻薄开衫 + 浅色短裤
气质：安静、克制、有吸引力
画幅：9:16
```

完整示例：[pure_desire_curve_examples.md](../examples/pure_desire_curve_examples.md)

## 都市时尚写真

```text
风格：都市时尚写真
场景：城市写字楼落地窗旁
服装：黑色西装外套 + 白色丝质内搭 + 高腰长裤
气质：清冷、利落、成熟
画幅：4:5
```

完整示例：[urban_fashion_examples.md](../examples/urban_fashion_examples.md)

## 古风仙侠美人图

```text
风格：古风仙侠美人图
场景：云雾山水间的古风庭院
服装：月白色飘逸古风长裙 + 银色细节
气质：清冷、疏离、仙气
画幅：9:16
```

完整示例：[gufeng_fantasy_examples.md](../examples/gufeng_fantasy_examples.md)

## 电商服装模特图

```text
风格：电商服装模特图
场景：干净摄影棚
服装：米白色针织连衣裙，完整展示版型、材质和腰线
气质：自然商业模特感
平台用途：电商主图
画幅：4:5
```

完整示例：[ecommerce_tryon_examples.md](../examples/ecommerce_tryon_examples.md)

## 参考图保留直接生成

```text
图片 1：我的成年自拍，作为 identity_reference
图片 2：需要试穿的服装，作为 product_reference
风格：都市时尚写真
要求：保留我的五官，穿上第二张图里的衣服，不要提示词，直接出图
```

人物图片仅支持本人或已授权成年人物。产品默认锁定核心视觉；Logo 与小字尽量保留，但不承诺逐字准确。规则见 [reference-image-lock.md](core/reference-image-lock.md)。
