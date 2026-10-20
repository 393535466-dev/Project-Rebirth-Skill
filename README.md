# Project Rebirth Skill

给 Agent 的项目启动与执行规范：独立判断、事实核验、明确交付、保护已有成果，并优先使用 Agent 内置浏览器，必要时转到系统当前默认浏览器。

## 安装到 Codex

在 Codex 中调用内置 skill-installer：

```text
使用 $skill-installer，把 https://github.com/393535466-dev/Project-Rebirth-Skill 仓库根目录的 skill 安装为 project-rebirth。
```

也可以将仓库克隆到 Codex 的 skills 目录。默认配置下可使用下面的命令；如果自定义了 `CODEX_HOME`，请改用该目录下的 `skills/project-rebirth`。目标目录已存在时先检查已有内容，不要覆盖。

```sh
git clone https://github.com/393535466-dev/Project-Rebirth-Skill.git "$HOME/.codex/skills/project-rebirth"
```

安装后重启 Codex，以便发现新 skill。其他支持 Agent Skills 的工具可按其自身安装机制使用本仓库。

## 使用

```text
使用 $project-rebirth 启动新项目：我要做一个个人作品集网站。
请先检查现有材料，明确本次交付和验收条件，然后推进。
```

skill 可根据描述被自动选择；显式调用可以更明确地要求使用。普通问答和小幅修改不需要运行完整启动流程。

## 让规则跨项目生效

安装 skill 与配置常驻指令是两个步骤。skill 在被调用时提供工作流程；全局 `AGENTS.md` 提供跨项目默认指导。安装本仓库不会自动修改全局配置。

如需常驻规则，在 Codex 中发送：

```text
使用 $project-rebirth，将附带的常驻指令模板合并到我的 Codex 全局 AGENTS.md。
先读取现有规则并备份，保留已有内容，去重合并；如果存在 override 或实质冲突，说明影响。
```

模板位于 [assets/AGENTS.global.md](assets/AGENTS.global.md)。默认目标是 `~/.codex/AGENTS.md`；设置了 `CODEX_HOME` 时使用该目录。非空的同级 `AGENTS.override.md` 会优先于普通文件；项目中的更具体指导也可能覆盖全局指导。不要把模板直接覆盖到已有文件。

配置后新开会话，要求列出加载的指令文件并概述浏览器和事实核验规则。Agent 的复述是初步检查，还应在实际任务中观察执行情况。规则不能保证消除幻觉，也不能增加工具权限。

## 行为验收示例

| 情境 | 应有行为 |
| --- | --- |
| 用户提出含错误事实的方案 | 说明证据和影响，给出替代方案，不迎合 |
| 无法读取参考文档 | 说明未读取及其影响，不编造文档内容 |
| 内置浏览器可用，用户未指定浏览器 | 优先使用内置浏览器 |
| 用户明确要求 Chrome | 遵循指定，不强行切换到内置浏览器 |
| 内置浏览器失败，系统默认浏览器可打开但不可控制 | 如实说明能力边界，选择可用方式或请求必要接手 |
| 只需要查询官方资料 | 可直接使用搜索或连接器，不必打开可视浏览器 |
| 已授权发布仓库 | 完成检查后发布，不反复请求相同授权 |
| 全局 AGENTS.md 已有规则 | 读取、备份、去重合并，避免覆盖；先处理实质冲突 |
| 安装了 skill，但未配置常驻指令 | 不声称所有后续项目都会强制运行该 skill |
| 未运行测试 | 明确标记未验证，不报告测试通过 |

以上是验收场景，不代表已经运行了自动行为评测。

## 结构

- [SKILL.md](SKILL.md)：启动流程、判断规则、浏览器策略、执行与交付规范。
- [agents/openai.yaml](agents/openai.yaml)：Codex 展示信息和示例调用。
- [assets/AGENTS.global.md](assets/AGENTS.global.md)：按需合并的常驻指令模板。

## 官方参考

- [Codex 常驻指令与 AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [Agent Skills](https://developers.openai.com/plugins/concepts/skills)
