# 0910v1 来源与优化记录

核验日期：2026-09-10（Asia/Shanghai）。目标：借鉴 GitHub 搜索 Skills 与研究方案，改进本地 githubskill。结论：**基于现有技能改**；原有选型目标适用，补齐搜索与证据方法即可，没必要引入整套外部平台。

本版文字为重新组织的工作流，没有复制外部脚本或整份 Skill。下列链接保留所读版本与出处；未来直接复制内容时仍须核对许可并保留要求的声明。

## 候选与采用范围

| 项目 / 所读内容 | star 快照 | 默认分支最近提交日期（UTC） | 采用的做法 | 不照搬的部分 |
|---|---:|---|---|---|
| [aaddrick/gh-cli-search：搜索路由](https://github.com/aaddrick/gh-cli-search/blob/908e76ff235ee85679d2fcd503ce5bb985be66b5/skills/gh-search/SKILL.md) 与 [代码搜索](https://github.com/aaddrick/gh-cli-search/blob/908e76ff235ee85679d2fcd503ce5bb985be66b5/skills/gh-search-code/SKILL.md) | 8 | 2025-11-16 | 按仓库/代码/Issue/PR 分流；有界 JSON 输出；CLI 与网页搜索能力区分。 | 没有因 star 少排除；部分参数解释须用官方手册修正。不能泛称每个子命令都有同一组排序参数。 |
| [affaan-m/ECC：search-first](https://github.com/affaan-m/ECC/blob/cc91c24f9a35dc79811f2326b6cbf4a74739009a/skills/search-first/SKILL.md) | 255,189 | 2026-09-09 | 先找项目内已有能力；检查可用搜索渠道；比较接入、扩展与自研的实际成本。 | 不把 researcher 子代理、读配置、安装依赖或调研所有生态写成默认前提；不把示例宣称当实测。旧仓库地址已重定向，使用规范名 ECC。 |
| [bytedance/deer-flow：github-deep-research](https://github.com/bytedance/deer-flow/blob/0d4925305a6330a3442dcd336ed25750aea87cbd/skills/public/github-deep-research/SKILL.md) | 82,144 | 2026-09-09 | 官方 API 与源码先建立事实，再按疑点补查；用提交和 PR 追溯，结论就近附来源。另读其 `scripts/github_api.py` 审查指标语义。 | 不固定四轮或十几次搜索，不强制长报告/图表，不将来源类型换算成 90% 等可信度，也不复制需维护的新 API 客户端。 |
| [microsoft/vscode-pull-request-github：form-github-search-query](https://github.com/microsoft/vscode-pull-request-github/blob/036872d3ce2458356ef127d15fd3533be48cda41/src/lm/skills/form-github-search-query/SKILL.md) | 2,622 | 2026-09-09 | 把自然语言映射到真实查询约束；必要时查标签，避免堆叠不相关标签导致漏检。 | 这是 Issue/PR 查询技能，不直接当作项目选型器；不强制每次查标签。`repo:` 应为完整 owner/repo，以官方文档为准。 |
| [github/github-mcp-server：仓库搜索工具定义](https://github.com/github/github-mcp-server/blob/7d13a7ad6f2a17f351a6d77ce280c85ae1821f4d/pkg/github/__toolsnaps__/search_repositories.snap) 与 [代码搜索工具定义](https://github.com/github/github-mcp-server/blob/7d13a7ad6f2a17f351a6d77ce280c85ae1821f4d/pkg/github/__toolsnaps__/search_code.snap) | 32,835 | 2026-09-08 | 将检索与内容读取分开，按需取字段和页数；使用当前工具公开的 schema。 | 它是连接 GitHub 的工具方案，不承担选型判断；不假设当前连接器的名称、字段与原生 MCP 完全一致，也不为本次升级安装新服务。 |

star、archived 状态和默认分支通过各仓库 GitHub API 读取；五个仓库当时均未归档。日期是仓库默认分支提交日期，**不是目标 Skill 最近改动日期，也不能单独证明持续维护**。本次没有运行外部项目或逐仓库完成全面 Issue 审计。

## 所读版本与许可

| 仓库 | 固定 commit | 读取的根许可证 |
|---|---|---|
| aaddrick/gh-cli-search | `908e76ff235ee85679d2fcd503ce5bb985be66b5` | [MIT](https://github.com/aaddrick/gh-cli-search/blob/908e76ff235ee85679d2fcd503ce5bb985be66b5/LICENSE) |
| affaan-m/ECC | `cc91c24f9a35dc79811f2326b6cbf4a74739009a` | [MIT](https://github.com/affaan-m/ECC/blob/cc91c24f9a35dc79811f2326b6cbf4a74739009a/LICENSE) |
| bytedance/deer-flow | `0d4925305a6330a3442dcd336ed25750aea87cbd` | [MIT](https://github.com/bytedance/deer-flow/blob/0d4925305a6330a3442dcd336ed25750aea87cbd/LICENSE) |
| microsoft/vscode-pull-request-github | `036872d3ce2458356ef127d15fd3533be48cda41` | [MIT](https://github.com/microsoft/vscode-pull-request-github/blob/036872d3ce2458356ef127d15fd3533be48cda41/LICENSE) |
| github/github-mcp-server | `7d13a7ad6f2a17f351a6d77ce280c85ae1821f4d` | [MIT](https://github.com/github/github-mcp-server/blob/7d13a7ad6f2a17f351a6d77ce280c85ae1821f4d/LICENSE) |

许可证表仅说明读取的根文件，不代替对其他子目录、依赖、图片或品牌素材的单独许可核验。

## 本版自行补充的设计

- 把“完整产品／小组件／实现片段”分开搜索，用硬约束过滤，避免为一个小功能引入大平台。
- 给最相关候选补齐“来源 → 具体文件 → 适配 → 验证”，让借鉴能落到实施。
- 把事实、推断、源码检查和实际运行分开；审查仓库活跃、Issue 数量与 README 承诺的误读。
- 加入搜索覆盖、改写与停止条件，避免把搜索失败当无可用方案，或为了固定轮数继续搜索。
- 保持仅研究时不实施；已有“研究后优化”授权时继续，取消原版固定的等待确认句。
- 使用中文显示名与规范英文调用名，在描述中保留旧称触发；保持一个有效技能目录。
- 本机 `gh 2.95.0` 实测发现整段 Issue/PR 查询中的多个条件被错误合并，改用 `--repo`、`--merged` 等独立参数，并提供显式 GET 的原生查询方式；没有将外部 Skill 的示例直接当作已验证命令。

## 搜索覆盖与限制

本次使用 anysearch 的一般搜索与 `code.snippet`，查询涵盖 `"SKILL.md" "github" "search" "research"`、`"github-search" "skill"`、`"search-first" "SKILL.md"` 及限定 SKILL.md 的代码片段搜索。第三方目录只提供发现线索，实际采用前均读取原仓库文件与 LICENSE，并用 GitHub 官方搜索/CLI 文档复核语法。

没有把检索结果视为全网穷举。此记录是本版设计来源，不是下次任务的固定候选榜单。

## 本版验证记录

2026-09-10 本机验证：

- `quick_validate.py` 通过；YAML 可解析，显示名、正式调用名、自动触发策略和相对引用路径一致；有效目录仅有 `standing-on-giants`。
- 仓库搜索返回相关书签管理项目；代码搜索返回 aaddrick 仓库的具体 SKILL.md 文件。
- 修正后的 Issue/PR 参数写法分别返回 GitHub MCP Server 的 [Issue #3229](https://github.com/github/github-mcp-server/issues/3229) 与 [已合并 PR #3142](https://github.com/github/github-mcp-server/pull/3142)；提交和 Release 查询也成功。
- 原生 REST 查询正常返回 `total_count=0`、`incomplete_results=false`，与 aaddrick 仓库的该次 Issue 搜索范围一致；没有把正常空结果当作认证失败。

以上证明文件结构与已试命令可用，不等于完整行为评测，也不证明 Codex 已刷新技能菜单或外部项目已实际部署。
