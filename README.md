# myponytail

适配 **华为云码道（CodeArts）代码智能体** 和 **z.ai 网页版 agent 模式** 使用的 [Ponytail](https://github.com/DietrichGebert/ponytail) 镜像仓库。

> Ponytail 让你的 AI agent 像那个扎马尾辫、戴椭圆眼镜、在公司待得比版本控制还久的资深工程师一样写代码：最懒但确实可行的方案。最好的代码是从未写出的代码。

本仓库 fork 自上游 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)，做以下两件事：

1. **适配华为云码道** —— 用码道原生支持的 `AGENTS.md` + `.codeartsdoer/skills/` 机制，无需插件系统即可生效。
2. **单独提供 z.ai 网页版 agent 模式用的 skill** —— 放在 `zai-web/ponytail/SKILL.md`，可直接上传或粘贴使用。

## 仓库结构

```
myponytail/
├── AGENTS.md                                    # 码道自动识别的全局规则（核心 ponytail 规则）
├── .codeartsdoer/
│   └── skills/                                  # 码道项目级技能目录（自动加载）
│       ├── ponytail/SKILL.md                    # 主技能：懒惰模式
│       ├── ponytail-review/SKILL.md             # 审查 diff 过度设计
│       ├── ponytail-audit/SKILL.md              # 整仓过度设计审计
│       ├── ponytail-debt/SKILL.md               # 收集 ponytail: 注释为账本
│       ├── ponytail-gain/SKILL.md               # 收益记分牌
│       └── ponytail-help/SKILL.md               # 命令参考卡
├── zai-web/
│   └── ponytail/SKILL.md                        # z.ai 网页版 agent 模式专用 skill
├── docs/
│   └── install-huawei-codearts.md               # 华为云码道安装与使用说明
├── README.md                                    # 本文件
└── LICENSE                                      # MIT
```

## 核心思想：懒人阶梯

在写任何代码之前，停在第一个成立的台阶上：

1. **这真的需要存在吗？**（YAGNI）—— 不需要就别写。
2. **代码库里已经有了吗？** —— 复用，不要重写。
3. **标准库能做吗？** —— 用它。
4. **平台原生功能能覆盖吗？** —— `<input type="date">` 优于日期选择器库。
5. **已安装的依赖能解决吗？** —— 用它，永远不要为几行能搞定的事加新依赖。
6. **能写成一行吗？** —— 一行。
7. **以上都不成立时：** 写能跑起来的最小代码。

关键约束：**懒，但不是偷工减料**。永远不要简化掉信任边界校验、防数据丢失的错误处理、安全、可访问性、用户明确请求的任何东西。

## 三档强度

| 档位 | 行为 |
|------|------|
| **lite** | 做被要求的事，用一行指出更懒惰的备选方案。用户选。 |
| **full** | 阶梯强制执行。标准库和原生优先。最短 diff、最短解释。默认。 |
| **ultra** | YAGNI 极端派。删除优先于新增。交付一行方案，同时质疑剩余需求。 |

## 快速开始

### 在华为云码道中使用

1. 把本仓库克隆到你的项目根目录，或把 `AGENTS.md` 和 `.codeartsdoer/` 复制到你的项目根目录。
2. 打开华为云码道 IDE，进入设置 → 技能与规则，确认"项目级"页签下能看到 6 个 ponytail 技能，状态为已开启。
3. 在对话窗口正常使用，码道会按上下文自动加载技能；也可显式说"使用 ponytail-review 技能审查这段 diff"。

详细说明见 [`docs/install-huawei-codearts.md`](docs/install-huawei-codearts.md)。

### 在 z.ai 网页版 agent 模式中使用

1. 把 `zai-web/ponytail/` 目录打包成 zip（保持 `ponytail/SKILL.md` 结构）。
2. 在 z.ai 网页版 agent 模式的技能管理页面上传该 zip。
3. 启用技能，在对话里说"用 ponytail 模式帮我写一个 XX"即可触发。
4. 关闭：对 agent 说 "stop ponytail" 或 "正常模式"。

如果 z.ai 网页版只支持对话级粘贴，把 `zai-web/ponytail/SKILL.md` 中的"懒人阶梯"和"规则"两节直接贴在对话开头作为系统提示也可生效。

## 上游来源

本仓库的内容（规则文本、技能定义、benchmark 数据）全部来自上游 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)（MIT License）。本仓库只做了：

- 把英文规则翻译/改写为中文（保留原意）
- 适配华为云码道的目录结构（`.codeartsdoer/skills/`）和 `AGENTS.md` 自动识别机制
- 单独抽出 z.ai 网页版可用的 SKILL.md

所有荣誉归上游作者 [Dietrich Gebert](https://github.com/DietrichGebert)。

## License

MIT，继承自上游。
