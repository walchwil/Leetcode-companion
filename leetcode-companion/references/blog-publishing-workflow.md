# LeetCode 讨论 → GitHub Pages 博客发布协议

本文件是 `leetcode-companion` 的发布执行手册。用户在一题讨论后说“更新到博客 / 发布到博客 / 同步到博客”时使用。

## 1. 当前默认目标（写入前仍须实时验证）

- GitHub repo：`walchwil/walchwil.github.io`
- 默认分支：`main`
- 内容源：`content/problems.ts`
- 页面路由：`/problems/<slug>/`
- 站点根地址：`https://walchwil.github.io/`
- 站点采用 Next.js 静态导出；GitHub Pages 部署产物由构建产生，不直接编辑 `out/`。

当前仓库 README 说明：题解内容集中在 `content/problems.ts`，首页和详情页由该数据生成；push 到 `main` 会触发 GitHub Actions 自动重新构建和部署。

**这是一份契约快照，不是永恒真理。每次发布必须先读取 live repo。**

## 2. 发布触发与授权

当当前讨论已经明确是哪道题，用户主动说：

- “更新到博客”
- “发布到博客”
- “同步到博客”
- “把这题放博客”

即视为对“将当前题学习成果写入博客仓库”的明确意图。不要再要求一次重复确认。

以下情况例外，需要最小澄清：

- 当前上下文没有可识别的题目；
- 用户同时谈了多题且没说发布哪一题；
- 将涉及删除/重构站点而非单题内容更新；
- 用户明确说远端 main 不是最新、本地还有未 push 的冲突改动。

## 3. 发布前从对话自动抽取

不要让用户手填。自动整理：

1. LeetCode id、英文/中文标题、难度、topics、slug；
2. 当前题真正的“白话本质”；
3. 数据结构/算法的正规定义；
4. 祛魅后的当前题映射；
5. 用户最初能理解的朴素方案；
6. 朴素方案瓶颈；
7. 优化方法与为什么有效；
8. 最终稳定 Python 代码；
9. 时间/空间复杂度；
10. 1–2 个本题 Python 积跬步；
11. 3–5 条高价值 take-away；
12. 关键边界或易错点。

如果某项对网页 schema 不是必需且对本次学习没有价值，不要为了凑字段编造内容。

## 4. 公开文章的编辑原则

博客是“未来自己能快速复习”的公开学习笔记，不是聊天 transcript。

保留：

- 从朴素思路到优化的认知跳跃；
- 术语正规定义与祛魅；
- 真正暴露出的薄弱点所对应的知识补丁；
- 有迁移价值的 bug/边界；
- 简洁、可靠的最终 Python 实现。

默认不公开：

- “用户算法小白”等长期个人画像标签；
- 与题目无关的聊天；
- 私密信息；
- 大段试错对话原文；
- 尚未验证、对话中后来被推翻的错误结论。

## 5. Live repo 读取（强制）

每次发布都先读取：

1. repo metadata：确认仓库、默认分支、写权限；
2. `README.md`：确认当前维护/部署约定；
3. `content/problems.ts`：确认最新 schema、现有条目和 blob SHA；
4. 必要时 `.github/workflows/*` / `package.json`：确认部署仍由 push 驱动。

不要用聊天记忆里的旧版本覆盖 live repo。

## 6. 找到正确的题目槽位

优先用 `id` 和 `slug` 定位：

### 已有 completed 文章
更新同一个对象的 `article`、`date`、`note` 等必要字段，不新增重复题。

### 已有 planned 占位
把该条从 `planned` 升为 `completed`，补齐 `article`，保留原 id/slug；不要再创建第二条。

### 尚不存在
按 live `Problem` 类型新增一条，保持当前文件的排序/风格，不大规模重排其他题目。

## 7. 当前 ProblemArticle 语义映射

如果 live repo 仍使用现有结构：

- `subtitle`：本题认知转折；
- `readTime`：合理估算，不追求精准；
- `focus`：如 `FOUNDATION FIRST · 哈希表入门`；
- `focusTag`：如 `dict` / `dp` / `two pointers`；
- `focusDescription`：一句工具价值；
- `essence`：题目表面问题背后的真正操作需求；
- `equation`：本题最核心关系；
- `foundation.name`：如“哈希表是什么？”；
- `foundation.definition`：正规定义；
- `foundation.mapping`：当前题映射 + 祛魅；
- `initialApproach`：朴素方案；
- `optimizedApproach`：优化方案；
- `code`：最终 Python；
- `codeTitle`：简短标题；
- `syntaxNote`：1–2 个 Python 积跬步；
- `takeaways`：3–5 条可迁移经验。

若 live 类型改变，严格跟随 live 类型，不擅自回滚 schema。

## 8. 写入安全协议

1. 取得 `content/problems.ts` 的**最新 blob SHA**；
2. 只对当前题对应对象做语义增量；
3. 保留所有不认识/无关的新内容；
4. 写前做内部一致性检查：TS 字符串/模板字符串闭合、字段齐全、slug 唯一、代码块不破坏反引号；
5. 用当前 SHA 调用 GitHub `update_file`；
6. commit message：`docs(leetcode): publish <id> <slug>` 或 `docs(leetcode): update <id> <slug>`；
7. 禁止 force push；
8. 如果 SHA 冲突：refetch 最新文件，重新应用本题变化，再提交；
9. 如果发现用户本地尚未 push 的同文件修改：远端无法合并未知本地状态，先要求 push 或改用分支/PR。

## 9. 何时用直接 main，何时用分支/PR

### 默认：单题内容更新 → 直接 main
前提：

- 只更新 `content/problems.ts`；
- schema 不变；
- 没有已知并行本地修改；
- 变化局部、可逆。

这样最符合用户“更新到博客”一句话完成的目标。

### 改用分支/PR

- 需要改组件、样式、路由或构建配置；
- 需要改多个核心文件；
- 网站 schema 要迁移；
- 检测到并行开发/潜在冲突；
- 用户明确要求先审阅 diff。

## 10. 部署验证

当前 deploy workflow 在 `main` push 后执行：安装依赖 → `npm run build:pages` → 上传 `out` → deploy Pages。

提交后必须继续：

1. 确认 commit 已创建；
2. 查看对应 GitHub Actions run；
3. 构建失败：读取失败 job/log，修复根因后再提交；
4. 部署成功：访问站点首页；
5. 访问 `/problems/<slug>/`；
6. 最少检查：标题、标签、foundation、代码、takeaways 是否正常渲染。

不要在只完成 git commit 时就说“网页已发布成功”。

## 11. 工具能力降级

### 有 GitHub 读写工具
真正执行 fetch → update → verify。

### 只有 GitHub 读取
生成准确的目标字段和 patch，但明确“尚未写入 GitHub”。

### 没有 GitHub 工具
说明 Skill 无法凭自己获得外部仓库权限；让用户连接 GitHub app/plugin，或切换到能访问该仓库的 Chat/Work/Codex 环境。

Skill 是流程说明，不是凭证。

## 12. 最终给用户的回执

发布成功时只需短回执：

- 已更新题目：`<id> <title>`；
- 修改源：`content/problems.ts`；
- commit：简短 SHA / 信息；
- Pages：部署成功/仍在构建/失败；
- 页面：可访问则给页面位置。

用户不需要再手动整理。
