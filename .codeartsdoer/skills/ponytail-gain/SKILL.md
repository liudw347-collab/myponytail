---
name: ponytail-gain
description: >
  展示 ponytail 的实测收益作为一张紧凑的记分牌：更少代码、更少成本、更快速度，数据来自 benchmark 中位数。一次性展示，不是持久模式，也不是当前仓库的数字。触发条件：/ponytail-gain、"ponytail gain"、"ponytail 节省了什么"、"显示 ponytail 收益"、"ponytail 记分牌"。
---

# Ponytail Gain

被调用时展示这张记分牌。一次性：不要改模式、不要写 flag 文件、不要持久化任何东西。

这些数字是已发布的 benchmark 中位数（5 个日常任务：邮箱验证器、debounce、CSV 求和、倒计时器、限流器；三个模型：Haiku、Sonnet、Opus）。它们是测量出来的，不是从当前仓库算出来的。来源：原 ponytail 仓库的 `benchmarks/` 和 README。

## 记分牌

用纯 ASCII 条形图。条长代表测量范围；标签带精确数字：

```
  ponytail gain                     benchmark 中位数 · 5 任务 · 3 模型

  代码行数    无技能  ████████████████████  100%
              ponytail  ██▌·················    6–20%   ▼ 80–94%
  成本        无技能  ████████████████████  100%
              ponytail  █████▌··············   23–53%  ▼ 47–77%
  速度        ponytail  ▸ 3–6× 更快

  本仓库：  /ponytail-debt  （你延迟的捷径）
            /ponytail-audit （还能砍什么）
```

## 诚实边界

这些是 benchmark 中位数，不是本仓库的数据。永远不要打印一个本仓库的节省数字（"你这里省了 X 行/tokens"）：未构建的版本从未被写过，所以活仓库里没有真实基线可减。唯一真实的本仓库数字来自 `/ponytail-debt`（一个数过的账本），这张卡指向它而不是凭空发明一个。

## 边界

一次性展示。不改动任何东西、不改模式。"stop ponytail" 或 "正常模式"：还原。
