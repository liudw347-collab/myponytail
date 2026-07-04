# 在华为云码道（CodeArts）代码智能体中使用 Ponytail

华为云码道原生支持两种自定义规则机制：

1. **`AGENTS.md`** —— 放在项目根目录，码道自动识别并应用为全局规则。
2. **`.codeartsdoer/skills/`** —— 项目级技能目录，码道自动加载其中的每个子目录作为一个技能。

本仓库同时使用了这两种机制：`AGENTS.md` 提供"懒惰模式"的全局默认行为，6 个技能提供更具体的子命令（审查、审计、账本等）。

## 安装方式一：项目级（推荐，随代码库分发）

适合为某个具体项目定制规则，规则会随代码库一起被团队成员获取。

### 步骤

1. **把规则文件放到项目根目录**

   把本仓库的以下两个东西复制到你的目标项目根目录：

   ```
   你的项目/
   ├── AGENTS.md                              # 来自本仓库根目录
   └── .codeartsdoer/
       └── skills/                            # 来自本仓库的 .codeartsdoer/skills/
           ├── ponytail/SKILL.md
           ├── ponytail-review/SKILL.md
           ├── ponytail-audit/SKILL.md
           ├── ponytail-debt/SKILL.md
           ├── ponytail-gain/SKILL.md
           └── ponytail-help/SKILL.md
   ```

   可以用 git submodule 或直接复制：

   ```bash
   # 方式 A：作为 submodule（推荐，便于跟踪上游更新）
   cd 你的项目
   git submodule add https://github.com/liudw347-collab/myponytail.git .ponytail
   # 然后软链接或复制所需文件
   ln -s .ponytail/AGENTS.md AGENTS.md
   ln -s .ponytail/.codeartsdoer .codeartsdoer

   # 方式 B：直接复制
   cd 你的项目
   git clone https://github.com/liudw347-collab/myponytail.git /tmp/myponytail-src
   cp /tmp/myponytail-src/AGENTS.md .
   cp -r /tmp/myponytail-src/.codeartsdoer .
   ```

2. **打开华为云码道 IDE**，进入你的项目。

3. **确认技能已加载**

   - 单击 IDE 右上角的设置图标
   - 左侧导航选择"技能与规则"
   - 切换到"项目级"页签
   - 应该能看到 6 个技能：`ponytail`、`ponytail-review`、`ponytail-audit`、`ponytail-debt`、`ponytail-gain`、`ponytail-help`
   - 状态默认为已开启，如未开启请手动打开

4. **开始使用**

   在聊天窗口正常对话，码道会按上下文自动加载技能。也可以显式触发：

   - "用 ponytail 模式帮我实现一个限流器"
   - "使用 ponytail-review 技能审查当前 diff 的过度设计"
   - "用 ponytail-audit 审计整个仓库找臃肿"
   - "把所有 ponytail: 注释列成账本"（等同 ponytail-debt）
   - "显示 ponytail 的收益记分牌"（等同 ponytail-gain）

## 安装方式二：个人级（所有项目生效）

适合你个人在所有项目中都使用 ponytail 规则。

### 步骤

1. **复制技能到个人级目录**

   - Linux / macOS: `~/.codeartsdoer/skills/`
   - Windows: `%USERPROFILE%\.codeartsdoer\skills\`

   ```bash
   # Linux / macOS
   mkdir -p ~/.codeartsdoer/skills
   git clone https://github.com/liudw347-collab/myponytail.git /tmp/myponytail-src
   cp -r /tmp/myponytail-src/.codeartsdoer/skills/* ~/.codeartsdoer/skills/
   ```

2. **个人级 AGENTS.md**

   个人级规则文件位置：`~/.codeartsdoer/AGENTS.md`（Linux/macOS）或 `%USERPROFILE%\.codeartsdoer\AGENTS.md`（Windows）。

   如果想全局启用 ponytail，把本仓库的 `AGENTS.md` 复制到这个位置。

3. **在 IDE 中确认**

   设置 → 技能与规则 → "个人级"页签，确认 6 个技能可见且已开启。

## 安装方式三：通过 npx 一键安装（如果码道支持）

华为云码道支持通过 `npx skills add` 命令从 GitHub 安装技能：

```bash
# 安装到个人级
npx skills add https://github.com/liudw347-collab/myponytail.git --skill ponytail -a codearts-agent

# 安装到项目级
npx skills add https://github.com/liudw347-collab/myponytail.git --skill ponytail -a codearts-agent
# 选择 Project 即项目级
```

可以一次安装所有 6 个技能：

```bash
for skill in ponytail ponytail-review ponytail-audit ponytail-debt ponytail-gain ponytail-help; do
  npx skills add https://github.com/liudw347-collab/myponytail.git --skill $skill -a codearts-agent
done
```

> 注：`npx skills add` 命令要求目标仓库的技能目录结构遵循码道规范。本仓库的 `.codeartsdoer/skills/<skill-name>/SKILL.md` 结构已遵循该规范。

## 档位切换

| 档位 | 触发方式 | 行为 |
|------|----------|------|
| **lite** | 对话中说 "ponytail lite" | 做被要求的事，用一行指出更懒惰的备选方案。 |
| **full** | 对话中说 "ponytail" 或 "ponytail full" | 阶梯强制执行。标准库和原生优先。默认。 |
| **ultra** | 对话中说 "ponytail ultra" | YAGNI 极端派。删除优先于新增。 |

也可以通过环境变量配置默认档位：

```bash
export PONYTAIL_DEFAULT_MODE=ultra   # lite / full / ultra / off
```

或配置文件 `~/.config/ponytail/config.json`：

```json
{ "defaultMode": "ultra" }
```

## 卸载

- **项目级**：删除项目根目录下的 `AGENTS.md` 和 `.codeartsdoer/skills/ponytail*` 目录。
- **个人级**：删除 `~/.codeartsdoer/skills/ponytail*` 目录和 `~/.codeartsdoer/AGENTS.md`（如果之前复制过）。
- **submodule 方式**：`git submodule deinit -f .ponytail && git rm -f .ponytail`，然后清理软链接。

## 验证安装

安装完成后，在码道对话窗口里输入：

```
用 ponytail 模式给我写一个 Python 邮箱验证函数
```

如果安装正确，码道会：
1. 优先建议标准库方案（如 `email.utils.parseaddr` 或简单的 `"@" in email` 检查）
2. 跳过自定义 validator 类
3. 在代码后用 1-3 行说明跳过了什么、何时加回来

如果码道照旧写了一个 30 行的 `EmailValidator` 类，说明技能未生效，请回到"技能与规则"页面检查状态。

## 常见问题

**Q: 我看不到技能？**
A: 检查 `.codeartsdoer/skills/` 目录是否在项目根目录下（不是子目录），每个技能文件夹内必须有 `SKILL.md`（文件名不可改）。

**Q: AGENTS.md 不生效？**
A: 确认它在项目根目录（不是 `.codeartsdoer/` 内），且码道 IDE 已重启或重新加载项目。

**Q: 技能优先级冲突？**
A: 码道的技能优先级：云端企业级 > 云端团队级 > 本地项目级 > 本地个人级 > 云端个人级 > 系统内置。如果你的项目里同名技能被覆盖，检查更高优先级是否有冲突。

**Q: 想关闭但不卸载？**
A: 在"技能与规则"页面把对应技能的开关关掉，或在对话里说 "stop ponytail" / "正常模式"。

## 上游与许可

本仓库内容来自上游 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)（MIT License）。本仓库的中文翻译和码道适配同样以 MIT 许可发布。
