# GitHub 搜索方法

用于把需求变成查询、核对候选和解释空结果。命令只是可替换的例子，先看实际可用工具及其参数；不要求安装 `gh`、MCP 或额外脚本。

## 选择渠道

| 渠道 | 合适用途 | 边界 |
|---|---|---|
| 项目内 `rg` | 已有实现、依赖和测试 | 只代表已读取的本地文件。 |
| anysearch | 用自然语言发现项目；`code.snippet` 找实现 | 按该 Skill 先发现代码子域；失败自动换渠道，摘要不作最终证据。 |
| 当前 GitHub 连接器/MCP | 仓库、代码、Issue/PR 搜索和读取 | 先读工具 schema。有的代码搜索包装只接受普通关键词，通过单独参数限定仓库；不要把原生查询限定符塞入禁止它们的包装器。 |
| GitHub CLI / REST | 可复核的仓库数据、文件和有限条数搜索 | `gh` 需要可用的认证；只让现有工具使用已有认证，不读取或输出凭证。认证失败转可用只读渠道。 |
| GitHub 网页 Code Search | 布尔表达式、正则、符号和路径搜索 | 与 CLI/REST 代码搜索不是同一套能力，结果也可能不同。 |

## 查询策略

- **找产品**：问题词 + 产品形态，例如 `bookmark manager self-hosted`。
- **找组件**：核心能力 + 接口/平台，例如 `markdown broken links cli`。用户指定技术栈时再加语言。
- **找技能**：发现阶段可用 `site:github.com "SKILL.md" "核心能力"`；检索失败时去候选仓库定点找文件。
- **找实现**：从 README/文档取真实函数名、配置键或错误信息，再限定仓库/目录。不要只查“实现某功能”这类空泛短语。
- **查短板**：用目标平台、缺失功能或已知错误在候选的 Issue/PR 中找证据；需要标签过滤时先核对真实标签，不臆造 `bug`、`critical` 等名称。
- 一条查询只回答一个问题；同义词可分别搜索后按 `owner/repo` 去重。查询太窄时先去掉 star、时间、语言或部署偏好中的一个，不放宽用户硬约束。

## 仓库搜索：使用仓库限定符

```bash
gh search repos 'bookmark manager in:name,description,readme archived:false' --limit 8 --json fullName,url,description,stargazersCount,pushedAt,isArchived,license
gh search repos 'markdown link checker in:readme archived:false' --sort updated --limit 8 --json fullName,url,description,pushedAt
```

`in:readme` 可发现名称未包含关键词的方案；仓库搜索不能搜任意源码。`topic:`、`language:`、`license:`、`pushed:>=YYYY-MM-DD` 按实际需要加入，日期占位必须替换。star 用于比较，许可筛选不能替代读取 LICENSE。[官方仓库搜索说明](https://docs.github.com/en/search-github/searching-on-github/searching-for-repositories)

## 代码搜索：分清 CLI/REST 与网页

CLI 的可运行示例：

```bash
gh search code 'search' --repo aaddrick/gh-cli-search --filename SKILL.md --limit 5 --json repository,path,url
gh search code 'NewRequestWithContext' --repo github/github-mcp-server --language go --limit 5 --json repository,path,url
```

下面是给 GitHub 网页 Code Search 的查询，不是 shell 命令，也不要直接塞进 CLI/REST：

```text
path:/(^|\/)SKILL\.md$/ ("GitHub" OR "open source") "search"
repo:github/github-mcp-server language:go symbol:SearchCode
```

CLI/REST 使用旧代码搜索引擎，不支持网页新增的正则功能。网页支持 `repo:owner/repo`、`path:`、`symbol:`、`content:` 以及布尔组合；符号名必须换成目标中的真实名称。[CLI 说明](https://cli.github.com/manual/gh_search_code) · [网页语法](https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax)

查询在 shell 中用单引号或安全的参数数组传递。查询含以 `-` 起头的排除项时，将所有 CLI 选项放在 `--` 前、查询放在后；不要通过拼接用户原文生成可执行 shell。

```bash
gh search repos --limit 5 --json fullName,url -- 'bookmark manager -topic:awesome'
```

## 维护核验：分开查 Issue 与 PR

```bash
gh search issues --repo aaddrick/gh-cli-search --limit 5 --sort updated --json title,url,state,updatedAt
gh search prs --repo aaddrick/gh-cli-search --merged --limit 5 --sort updated --json title,url,state,updatedAt
gh api 'repos/aaddrick/gh-cli-search/commits?per_page=3' --jq '.[] | {sha, url: .html_url, date: .commit.committer.date, subject: (.commit.message | split("\n")[0])}'
gh api 'repos/aaddrick/gh-cli-search/releases?per_page=1' --jq '.[] | {tag_name, published_at, html_url}'
```

`repo:` 使用完整的 `owner/repo`。状态和更新时间只能筛选线索；需要读相关讨论、修复提交与发布情况才能判断问题是否解决。[Issue/PR 查询说明](https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests)

在 CLI 中优先用 `--repo`、`--state`、`--merged` 等参数表达过滤条件，减少整段查询被二次解析的歧义。确需完整原生查询时可显式调用只读 REST：

```bash
gh api --method GET search/issues -f q='repo:aaddrick/gh-cli-search is:issue' -f per_page=5 --jq '{total_count, incomplete_results, items: [.items[] | {title, html_url, state}]}'
```

REST 的 Issues 接口可能返回 PR；需要纯 Issue 时排除带 `pull_request` 字段的项。仓库的 `open_issues_count` 不能直接当未解决缺陷数量。[GitHub Issues API 说明](https://docs.github.com/en/rest/issues/issues)

## 证据读取与失效处理

1. 读取仓库元数据、默认分支和真实 LICENSE，记录 `stargazers_count`（Star 数）、`size`（仓库体积，KB）及查询日期。体积从仓库详情 API 读取，不假设搜索命令支持该字段；它不是下载包、依赖或安装占用。分别统计实际读取的去重候选数与已核验数，未知值写“未取得”。再按需读近期提交、Release 与相关 Issue/PR，不批量抓无关历史。
2. 读取目录和关键文件，记录当时的 commit SHA。支持时把借鉴链接固定为 `https://github.com/owner/repo/blob/<commit>/<path>`；行号也须核对。
3. `404` 可能是路径改名、版本变化或访问权限；先核对规范仓库地址和 ref。重定向到新仓库名时按其规范地址去重。
4. 无结果先检查查询类型、限定符、默认分支和索引覆盖。网页、连接器与 CLI 结果不同不能直接认定某一方出错。
5. 分页只在会影响结论时继续。REST 每条搜索最多可取得 1,000 个结果；`incomplete_results=true` 表示可能不完整。429/限流时按返回提示停止或换可用渠道，不连续重试同一请求。[REST 搜索边界](https://docs.github.com/en/rest/search/search)

输出只声称实际覆盖的范围，例如“检索到的候选中未找到满足离线约束的实现”。工具不可用时写明渠道未覆盖，不用猜测的 star、提交日期或仓库链接填表。
