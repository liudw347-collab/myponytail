---
name: ponytail-help
description: >
  所有 ponytail 模式、技能和命令的快速参考卡。一次性展示，不是持久模式。触发条件：/ponytail-help、"ponytail help"、"ponytail 有哪些命令"、"怎么用 ponytail"。
---

# Ponytail Help

被调用时展示这张参考卡。一次性，不要改模式、不要写 flag 文件、不要持久化任何东西。

## 档位

| 档位 | 触发 | 行为变化 |
|------|------|----------|
| **Lite** | `ponytail lite` | 做被要求的事，用一行指出更懒惰的备选方案。 |
| **Full** | `ponytail` | 阶梯强制执行：YAGNI → 标准库 → 原生 → 一行 → 最小。默认。 |
| **Ultra** | `ponytail ultra` | YAGNI 极端派。删除优先于新增。构建前先质疑需求。 |

档位持续到被改变或会话结束。

## 技能

| 技能 | 触发 | 作用 |
|------|------|------|
| **ponytail** | `ponytail` | 懒惰模式本身。最简可行方案。 |
| **ponytail-review** | `ponytail-review` | 过度设计审查：`L42: yagni: 工厂只有一个产品。内联。` |
| **ponytail-audit** | `ponytail-audit` | 整仓过度设计审计：可删项排名清单。 |
| **ponytail-debt** | `ponytail-debt` | 把 `ponytail:` 捷径注释收集成被跟踪的账本。 |
| **ponytail-gain** | `ponytail-gain` | 实测收益记分牌：更少代码、更少成本、更快速度。 |
| **ponytail-help** | `ponytail-help` | 本卡。 |

在华为云码道中，技能由智能体根据上下文自动识别并加载，也可在对话中显式说"使用 ponytail-xxx 技能"来触发。

## 关闭

说 "stop ponytail" 或 "正常模式"。随时用 `ponytail` 重新启用。

## 配置默认档位

默认档位 = `full`，每个会话自动激活。改变它：

**环境变量**（最高优先级）：
```bash
export PONYTAIL_DEFAULT_MODE=ultra
```

**配置文件**（`~/.config/ponytail/config.json`，Windows: `%APPDATA%\ponytail\config.json`）：
```json
{ "defaultMode": "lite" }
```

设为 `"off"` 在会话开始时不自动激活，需要时手动说 `ponytail` 启用。

优先级：环境变量 > 配置文件 > `full`。

## 更多

完整文档和示例：https://github.com/DietrichGebert/ponytail （原始上游项目）
