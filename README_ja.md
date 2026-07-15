[English](README.md) | [简体中文](README_zh.md) | [日本語](README_ja.md) | [한국어](README_ko.md)

# 女性ポートレート・プロンプトディレクター Skill

女性ポートレート・プロンプトディレクター Skill は、AI 画像生成向けの構造化プロンプト生成・視覚ディレクションシステムです。V1.6 では、20 種類の実装済みルートから必要なものだけを読み込み、明示されたパラメータまたは許可済み参照画像の主体を固定し、完全なプロンプトまたは主体保持型の画像編集を生成します。

このプロジェクトは単なるプロンプト集ではなく、拡張可能な女性ポートレート用 Skill フレームワークです。

## プロジェクトの目的

少数の入力パラメータから完全なプロンプトを生成します。ユーザーが明示した条件を保持しながら、顔立ち、体型、衣服、シーン、カメラとポーズ、光、フィルター、利用プラットフォーム、ネガティブ制約を視覚的に拡張します。対象人物は成人女性であることを明確にし、リアルな写真表現、節度のある演出、統一感、安定した生成を重視します。

## 対応スタイル

- ナチュラルなライフスタイル写真
- 抑制の効いた曲線美ライフスタイル写真
- 都会的なファッション写真
- 古風・仙侠テイストの人物画
- EC 向け衣服モデル画像
- レトロ香港風ポートレート
- フレンチリラックスポートレート
- 新中式・東洋美学ポートレート
- アクティブスポーツポートレート
- 旅行・バケーションポートレート
- スタジオレタッチポートレート
- 東洋的な豊潤美ポートレート
- 清冷な仙侠強化ポートレート
- 明媚で華やかな古風強化ポートレート
- 超接写リアルフェイスポートレート
- 古風貴女の水光メイクポートレート
- ブラックパール墨金 CCD 曲線ライフスタイル
- 元気で豊潤なソフト CCD ライフスタイル
- コールドホワイト透明感 CCD 曲線ライフスタイル

## 主な機能

- ユーザーが指定したパラメータを固定し、詳細化と安定化のみを行います。
- 目的に合うスタイルテンプレートを選択し、矛盾するスタイル語の混在を避けます。
- 顔立ち、体型、衣服、シーン、カメラとポーズ、光、フィルターをモジュールとして解析します。
- 短いパラメータを具体的な視覚ディレクションへ展開し、機械的な言い換えを避けます。
- 展開したモジュールを自然で詳細なコピー可能プロンプトへ統合します。
- EC 画像では衣服表示を優先し、曲線美スタイルでは明確な安全境界を維持します。
- 許可済みセルフィーの顔立ちまたは製品の主要視覚要素を保持した参照画像生成に対応します。

## クイックスタート

このリポジトリを Codex Skill として利用する場合は、`$female-portrait-director` を呼び出します。最小入力例：

```text
スタイル：ナチュラルなライフスタイル写真
シーン：カフェの窓際席
服装：白いニットカーディガン + 淡色のインナー
雰囲気：清潔感があり、やさしい
アスペクト比：9:16
```

システムは固定済みパラメータ、コピーして使える完全なプロンプト、ネガティブ制約を返します。全入力項目は [parameter_schema.md](skill/parameter_schema.md)、使用例は [usage_examples.md](skill/usage_examples.md) を参照してください。

## インストール

### npx によるワンコマンドインストール

`npx` を含む [Node.js](https://nodejs.org/) が必要です。Skill を Codex にグローバルインストールします。

```bash
npx skills@latest add https://github.com/liyue-aigc/female-portrait-director/tree/main/skills/female-portrait-director -g -a codex -y
```

インストール済み Skill を後から更新する場合：

```bash
npx skills@latest update female-portrait-director -g -y
```

### Git による手動インストール

別の方法として、リポジトリを Codex の skills ディレクトリにクローンできます。

Windows PowerShell：

```powershell
git clone https://github.com/liyue-aigc/female-portrait-director.git "$env:USERPROFILE\.codex\skills\female-portrait-director"
```

macOS または Linux：

```bash
git clone https://github.com/liyue-aigc/female-portrait-director.git "${CODEX_HOME:-$HOME/.codex}/skills/female-portrait-director"
```

Codex を再起動するか、新しい会話を開始してから次を呼び出します。

```text
$female-portrait-director
```

パラメータなしで初回呼び出しを行うと、20 種類のスタイル一覧、入力テンプレート、パラメータから詳細プロンプトへの例、直接画像生成と許可済み参照画像の使い方を含む V1.6 チュートリアルが表示されます。

## 例：パラメータからディレクション付きプロンプトへ

この Skill は入力を言い換えるだけではありません。明示された条件を保持し、不足している視覚要素を補完し、固定済みパラメータ、モジュール解析、完全なプロンプト、ネガティブ制約を出力します。

```text
スタイル：古風・仙侠テイストの人物画
シーン：霧深い山水の中にある古風な庭園回廊
服装：月白色の唐風幻想大袖衫 + 軽やかな披帛 + 銀刺繍の腰帯
雰囲気：清冷、距離感、仙気
顔立ち：古典的な東洋美人
体型：細身で繊細な体型
カメラ：軽い横向きの立ち姿、半身から太ももまで
光：冷調の柔光
フィルター：清冷で仙気のある古風フィルター
アスペクト比：9:16
用途：キャラクターポートレート
```

![古風仙侠プロンプト展開例](assets/examples/gufeng-director-output.jpg)

## 出力形式

```text
1. 固定済みパラメータ
2. モジュール解析
3. 最終プロンプト
4. ネガティブ制約
```

## リポジトリ構成

```text
.
├── README.md
├── README_zh.md
├── README_ja.md
├── README_ko.md
├── SKILL.md
├── agents/openai.yaml
├── assets/examples/
├── skill/
│   ├── skill.md
│   ├── style-registry.md
│   ├── public_instructions.md
│   ├── parameter_schema.md
│   ├── usage_examples.md
│   ├── core/
│   ├── references/
│   │   ├── director-expansion.md
│   │   └── visual-libraries.md
│   └── routes/
│       ├── commercial/
│       ├── curve/
│       ├── fantasy/
│       ├── fashion/
│       ├── lifestyle/
│       └── oriental/
├── docs/
│   ├── style_guide.md
│   ├── prompt_safety.md
│   ├── versioning.md
│   └── faq.md
└── examples/
```

## 安全境界

テキストのみの生成では、架空で明確に成人の人物を既定とします。参照画像ワークフローでは、ユーザー本人または許可済み成人人物の本人性、およびユーザーが使用権を持つ製品の視覚要素を保持できます。未成年者の性的表現、露骨なヌード、同意のない画像、なりすましを目的としたコンテンツ、嫌がらせ、名誉毀損、プライバシー侵害、その他の違法な目的には使用できません。詳細は [prompt_safety.md](docs/prompt_safety.md) と [DISCLAIMER.md](DISCLAIMER.md) を参照してください。

## ライセンス

このプロジェクトは [MIT License](LICENSE) の下で公開されています。MIT License は、使用、複製、変更、結合、公開、配布、再許諾、複製物の販売を許可します。安全境界は責任ある利用のためのガイドラインであり、標準 MIT License の条件を変更するものではありません。

## 作者とバージョン

- 作者：Li Yue（李岳）
- バージョン：`FEMALE-PORTRAIT-DIRECTOR-V1.6`
- プロジェクト：`Female Portrait Prompt Director Skill`
