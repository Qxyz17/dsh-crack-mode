# Crack Mode（破解模式）

一个 `@deepseek-ai/dsh-agent-preset` 声明型 bundle：安装后，新会话的模式列表里会多出「破解模式」，与内置的 standard / ptc / minimal / cordis 并列。

- **id**: `crack`，**order**: 5
- **插件**：与 standard 预设等价的完整编码 agent（shell、文件、搜索、任务、子代理、workflow、web、plan mode、压缩），额外挂载本包自带的 `crack` 技能。
- **技能**：`skills/crack/SKILL.md` 提供指纹 → 判定点 → 最小绕过 → 副作用验证 → 交付的证据链。

## 文件

| 文件 | 作用 |
|---|---|
| `cordis.patch.yml` | 插入 `preset-crack` 行；persona、技能目录与工具清单 |
| `locale/{zh,en}.json` | 插件清单里的标题与描述 |
| `icon.svg` | 清单图标 |
| `skills/crack/SKILL.md` | 破解工作流技能 |

## 安装

三个等价入口，任选其一：

```text
plugin_manager { action: "install_bundle", target: "dsh-crack-mode" }              # npm 包名
plugin_manager { action: "install_bundle", target: "github:Qxyz17/dsh-crack-mode" } # GitHub 仓库
plugin_manager { action: "install_bundle", target: "<绝对路径>/dsh-crack-mode" }    # 本地目录
```

安装后新建会话，在模式选择器里选「破解模式」即可；已有会话保持创建时的预设，不会自动切换。

## 注意

- `skill-filesystem` 的 `customSkillDirs` 用 `createRequire(baseUrl).resolve('dsh-crack-mode/package.json')` 定位本包的 `skills/`，因此必须作为 profile 依赖安装，不要手抄目录路径；改包名时这串必须同步改，否则技能挂不上。
- 破解行为的边界由会话的权限预设（read-only / workspace-write / danger-full-access）与实际授权决定，模式本身不改变沙箱与审批策略。
