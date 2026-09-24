# 站在巨人的肩膀上｜0924v1 发布说明

相较公开旧版 `githubskill`：使用 `standing-on-giants` 作为正式调用名，保留 `githubskill` 文字触发词；补入分层搜索、源码与许可核对、明确复用决策和可执行借鉴清单。增加必要的搜索参考文件、中文安装说明与 MIT 许可。Skill 执行正文不放逐版修正流水。

来源：本机原有 `standing-on-giants` Skill（本次发布前的功能版本为 0910v1）与旧公开仓库 `BLACKIELF/githubskill`；外部参考及当时的核验范围见 [`references/sources-0910v1.md`](references/sources-0910v1.md)。本文没有把外部项目的代码或完整 Skill 复制进来。

兼容性：按文件夹式 Skills 结构提供 `SKILL.md`、`agents/openai.yaml` 和 `references/`。本机用 Codex Skill 验证器检查结构；其他 Agent 工具的安装路径和自动发现能力应以该工具现行规则为准。

验证：发布前运行 `quick_validate.py`，核对 YAML、所有相对引用、公开仓库文件与本机源文件一致；发布后回读 GitHub 默认分支与许可证。验证不等于已运行每个候选项目，也不等于效率数字的独立测量。

回退：发布前的公开旧版位于提交 `90db30bbcc3204d93060314ef66a94fcfdd34667`；需要恢复旧版时从该提交读取，不覆盖当前本机 Skill。
