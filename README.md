[English](README.md) | [简体中文](README_zh.md) | [日本語](README_ja.md) | [한국어](README_ko.md)

# Female Portrait Prompt Director Skill

Turn a few portrait inputs into a coherent, camera-ready scene—or directly generate an image while preserving an authorized person or product reference.

[![skills.sh](https://skills.sh/b/liyue-aigc/female-portrait-director)](https://skills.sh/liyue-aigc/female-portrait-director)
[![GitHub stars](https://img.shields.io/github/stars/liyue-aigc/female-portrait-director?style=flat)](https://github.com/liyue-aigc/female-portrait-director/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

This is not a generic prompt collection. It locks explicit requirements, chooses one visual route, and directs the subject, action, clothing, scene, camera, lighting, and finish as one photographable moment. Subjects are fictional, clearly adult women unless an authorized adult reference is provided.

## See the difference

| Clean lifestyle | Urban fashion | Gufeng xianxia |
| --- | --- | --- |
| ![Natural cafe lifestyle portrait](assets/cases/01-clean-lifestyle.png) | ![Urban storefront fashion portrait](assets/cases/02-urban-fashion.png) | ![Moon-white gufeng xianxia portrait](assets/cases/03-gufeng-xianxia.png) |
| E-commerce model | Retro Hong Kong | New Chinese |
| ![Garment-first ecommerce model image](assets/cases/04-ecommerce-model.png) | ![Retro Hong Kong cinematic portrait](assets/cases/05-retro-hongkong.png) | ![Modern New Chinese tea-room portrait](assets/cases/06-new-chinese.png) |

Each case proves a different job: natural lifestyle direction, outfit-led street photography, complex costume organization, garment-first commercial framing, cinematic atmosphere, and restrained Eastern negative space. See the [six-case publishing pack](docs/女性人像提示词导演-6个视觉案例发布包.md) for the inputs and release copy.

## Try it in 60 seconds

Install with the open-source `skills` CLI:

```bash
npx skills add https://github.com/liyue-aigc/female-portrait-director/tree/main/skills/female-portrait-director -g
```

Start a new agent session, then paste:

```text
Use $female-portrait-director to generate an image directly:
Style: clean lifestyle portrait
Scene: quiet cafe window seat in the afternoon
Outfit: ivory knitted cardigan + light inner top
Mood: gentle, natural, clearly adult
Aspect ratio: 3:4
```

For a copy-ready prompt instead of an image, remove “generate an image directly.” The Skill returns locked parameters, a directed prompt, and negative constraints.

## Agent compatibility

The complete 55-file distribution package has been installer-verified with the current `skills` CLI for the following targets:

| Agent | Install package | Prompt workflow | Direct image workflow |
| --- | --- | --- | --- |
| Codex | Verified | Runtime-verified | Supported when image generation is available |
| Claude Code | Verified | Agent Skills compatible | Depends on the connected image tool |
| Cursor | Verified | Agent Skills compatible | Depends on the connected image tool |
| GitHub Copilot | Verified | Agent Skills compatible | Depends on the connected image tool |
| Gemini CLI | Verified | Agent Skills compatible | Depends on the connected image tool |

“Installer-verified” means the CLI copied `SKILL.md` and every referenced route, tool, safety file, and example into the target skill package. Only Codex was runtime-executed in this release environment; the other rows are packaging and specification compatibility claims, not claims that every host provides an image model.

## Supported Styles

- Clean lifestyle portraits
- Restrained curve-focused lifestyle portraits
- Urban fashion photography
- Gufeng fantasy portraits
- E-commerce clothing model images
- Retro Hong Kong portraits
- French relaxed portraits
- New Chinese oriental portraits
- Sporty active portraits
- Travel vacation portraits
- Studio-retouched portraits
- Oriental voluptuous portraits
- Cold xianxia enhanced portraits
- Bright luxury gufeng portraits

## Core Capabilities

- Lock user-specified parameters and only refine or stabilize them.
- Route each request through one on-demand style file and avoid conflicting style keywords.
- Parse face, body, outfit, scene, camera and pose, lighting, and filter modules.
- Expand short parameters into a coherent photographed moment instead of mechanically repeating them.
- Fuse the expanded modules into natural, detailed, copy-ready prompts with photography-director intent.
- Preserve clothing-display priority for e-commerce images and explicit safety boundaries for curve-focused portraits.
- Preserve authorized selfie identity or product core visuals for direct reference-image generation.

## Installation details

Requires [Node.js](https://nodejs.org/) for the one-command installer. Update an installed copy with:

```bash
npx skills@latest update female-portrait-director -g -y
```

### Manual Codex install with Git

Alternatively, clone the repository into your Codex skills directory.

Windows PowerShell:

```powershell
git clone https://github.com/liyue-aigc/female-portrait-director.git "$env:USERPROFILE\.codex\skills\female-portrait-director"
```

macOS or Linux:

```bash
git clone https://github.com/liyue-aigc/female-portrait-director.git "${CODEX_HOME:-$HOME/.codex}/skills/female-portrait-director"
```

Restart Codex or start a new conversation, then invoke:

```text
$female-portrait-director
```

## Example: Parameters to Directed Prompt

The Skill does more than restate the input. It preserves requested parameters, fills in missing visual details, and produces locked parameters, module analysis, a final prompt, and negative constraints.

```text
Portrait style: Gufeng xianxia beauty portrait
Scene: Traditional courtyard corridor surrounded by misty mountains and water
Outfit: Moon-white Tang-inspired fantasy wide-sleeve robe + flowing pibo scarf + silver embroidered waist sash
Mood: Cool, distant, ethereal
Facial direction: Classical East Asian beauty
Body direction: Slender and delicate figure
Camera direction: Slight side-facing standing pose, half-body to thigh framing
Lighting: Cool soft light
Filter: Cool ethereal gufeng filter
Aspect ratio: 9:16
Platform use: Character portrait
```

![Gufeng xianxia prompt expansion example](assets/examples/gufeng-director-output.jpg)

## Output Format

```text
1. Locked parameters
2. Module analysis
3. Final prompt
4. Negative constraints
```

## Repository Structure

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

## Safety Boundaries

Text-only generation defaults to fictional, clearly adult subjects. Reference-image workflows may preserve the identity of the user or an authorized adult subject, and may preserve product visuals that the user has the right to use. The project must not be used for sexualized minors, explicit nudity, non-consensual images, deceptive identity content, harassment, defamation, privacy violations, or other unlawful purposes. See [prompt_safety.md](docs/prompt_safety.md) and [DISCLAIMER.md](DISCLAIMER.md) for details.

## License

This project is licensed under the [MIT License](LICENSE). The MIT License permits use, copying, modification, merging, publishing, distribution, sublicensing, and selling copies. The safety boundaries are responsible-use guidelines and do not alter the standard MIT License terms.

## Author and Version

- Author: Li Yue (李岳)
- Version: `FEMALE-PORTRAIT-DIRECTOR-V1.4.1`
- Project: `Female Portrait Prompt Director Skill`
