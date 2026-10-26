# 站在巨人的肩膀上

一个免费、开源的 Agent Skill：让 AI 在执行新任务前，先查 GitHub 上有没有可直接使用、有限改造或值得借鉴的方案，再决定从哪里动手。

做 AI 视频、PPT、App、设计或自动化，都可以先让它调查已有工作流、工具、组件和示例工程。它不会只交一串热门仓库，而会回答：**用哪个、借哪部分、还缺什么、怎样验证。**

## 它会做什么

1. 拆清目标、平台、预算和硬约束。
2. 从完整方案、小型组件、具体文件或 Skill 三层查找。
3. 核对关键文件、运行条件、维护情况、改造成本和许可证。
4. 给出一个选择：直接使用、基于现有方案改造，或自己做最小版本。

例如做视频时先核对模型和设备；做 PPT 时先核对编辑与导出格式；做 App 时先核对目标平台；做设计时先核对组件能否适配风格及许可。找到相似项目，不代表它一定能直接用。

## 安装

适用于支持文件夹式 Agent Skills 的工具。把**整个仓库文件夹**放入该工具的 Skills 目录，保留 `SKILL.md`、`agents/` 和 `references/`；刷新工具后再调用。Codex 用户在目标目录尚不存在时，可以运行：

```bash
git clone https://github.com/BLACKIELF/standing-on-giants.git ~/.codex/skills/standing-on-giants
```

也可以把这句话交给你的 AI，让它按当前工具的规则处理：

```text
请先检查 https://github.com/BLACKIELF/standing-on-giants 中的 SKILL.md、references 和依赖，再按当前工具的 Skill 规则安装「站在巨人的肩膀上」；若同名 Skill 已存在，先对比，不要直接覆盖；完成后用我的一个实际任务验证。
```

安装后这样用：

```text
用「站在巨人的肩膀上」Skill，帮我找能实现【具体目标】的 GitHub 方案。约束是【平台／预算／设备】。告诉我直接用、改造还是自己做，并指出可借鉴的文件和最小验证方法。
```

也可以用更短的触发词：

```text
github一下，帮我找能实现【具体目标】的开源方案。
```

## 文件与边界

- [`SKILL.md`](SKILL.md)：主流程与决策标准。
- [`agents/openai.yaml`](agents/openai.yaml)：Codex 显示与调用配置。
- [`references/search-recipes.md`](references/search-recipes.md)：按任务需要读取的搜索方法。
- [`references/sources-0910v1.md`](references/sources-0910v1.md)：设计参考、来源和历史验证记录；其中的热度数字不是当前快照。

Skill 本身免费，采用 [MIT License](LICENSE)。它找到的第三方模型、API 或服务可能收费；公开可见的项目也不等于允许直接复制，实际复用要核对对应许可证。当前仓库验证的是 Skill 结构与搜索方法，不代表每个候选项目都已安装或运行。
