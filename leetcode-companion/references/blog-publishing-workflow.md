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

---

## 13. Last-mile 核心原则：教学与博客结构彻底解耦

博客只是学习成果的**下游发布适配器**，绝不能反向塑造前面的 LeetCode 教学。

必须保持：

```text
真实刷题讨论
  ↓
3补 / 3看 / 推导 / Python / 3背
  ↓
形成已经自洽的学习成果
  ↓
【发布边界】
  ↓
把成果序列化为当前博客 schema
  ↓
写 GitHub → Actions → Pages
```

因此：

- 讲题阶段不为了凑 `ProblemArticle` 字段而改变讲解顺序；
- 不因为网站当前只有某些 section，就删掉本题真正有价值的教学内容；
- 不把 `focus`、`equation`、`takeaways` 等网页字段暴露成用户必须填写的表单；
- 先完成教学，再做 schema mapping；
- 如果博客 schema 表达不了某个高价值学习点，优先保留在内部学习归档；只有用户要求改网站时，才进入网站结构设计任务。

一句话：**content-first，schema-second。**

## 14. “更新到博客”后的工具级执行序列

当用户发出发布触发词后，Agent 按下面顺序自动执行，不把这些步骤变成用户待办：

### Phase A：锁定本次发布对象

从刚结束的讨论中确定：

- `id`
- `slug`
- 中英文标题
- 最终 Python 解法
- 本题最终 3背
- 正规定义 + 祛魅映射
- Python micro-lesson
- 复杂度、边界、take-away

若信息足够，禁止让用户重复提供。

### Phase B：发现 GitHub 能力

优先使用当前环境已有的 GitHub connector / app / MCP / repository tool。

需要的最小能力是：

1. 读取 repository metadata；
2. 读取文件及 blob SHA；
3. 更新 UTF-8 文本文件并提交；
4. 查看 commit / workflow run；
5. 必要时读取 workflow job/log。

如果能直接读写 `walchwil/walchwil.github.io`，不需要用户把仓库 clone 到本地，也不要求用户手动 `git add/commit/push`。

### Phase C：实时读取发布契约

每次都重新读取 live repo，而不是相信 Skill 内的旧路径：

```text
repo metadata
  ↓
README.md
  ↓
content/problems.ts
  ↓
.github/workflows/*（必要时）
```

确认当前仍是：

```text
walchwil/walchwil.github.io
main
content/problems.ts
push main → GitHub Actions → GitHub Pages
```

如果 live repo 已变化，以 live repo 为准，并在不改变教学内容的前提下适配新的发布结构。

### Phase D：生成“语义增量”，不要重写整个博客

先按 `id` + `slug` 找到当前题：

```text
存在 completed → 更新该题 article
存在 planned   → 原地升级为 completed
不存在          → 新增一个符合 live schema 的 Problem
```

生成修改时只操作当前题对象。

尤其禁止：

- 重新格式化整个 `problems.ts`；
- 改动无关题目的文案；
- 根据聊天记忆恢复一个旧版本文件；
- 因为新增一题而重排整个数组；
- 直接改 `out/` 生成物。

### Phase E：并发安全写入

发布采用“read-latest → edit → compare-and-write”的思路：

1. fetch 最新 `content/problems.ts`；
2. 保存它的 blob SHA；
3. 在这份最新内容上应用当前题增量；
4. 使用该 SHA 执行 update；
5. GitHub 若报告 SHA 已变化/冲突，说明有人在步骤 1 后又修改了 main；
6. 此时重新 fetch 最新文件，只重新应用当前题增量；
7. 不 force push、不覆盖未知修改。

这使“ChatGPT 发布”和“用户本地刚 push”尽可能不会互相踩掉。

但远端无法感知**尚未 push 的本地文件**。如果用户明确告诉我们本地存在未 push 的 `content/problems.ts` 修改，先让用户 push，或切换到分支/PR 工作流。

### Phase F：提交策略

默认单题内容更新直接提交 `main`，因为这是低风险、单文件、可逆的内容变更，也是“一句话发布”体验的关键。

commit 示例：

```text
docs(leetcode): publish 070 climbing-stairs
docs(leetcode): update 001 two-sum
```

以下情况自动升级为 branch + PR，而不是直接改 main：

- 需要调整 TypeScript 类型；
- 需要改 React/Next.js 组件；
- 需要改路由、样式、构建或 workflow；
- 一次修改多个核心文件；
- 发现无法安全地做局部内容增量；
- 用户明确要求先看 diff。

## 15. 幂等性：同一句“更新到博客”重复执行也不能产生重复文章

发布流程必须是 idempotent：

- `id` 和 `slug` 是主定位键；
- 已有同题时更新，不新增第二份；
- 同一次讨论重复触发“更新到博客”，只会刷新同一题内容；
- 如果目标内容与 live repo 已完全一致，允许直接报告“已是最新”，不要制造无意义 commit。

这条规则保证用户可以放心把“更新到博客”当成一个按钮，而不必记自己刚才是否已经说过一次。

## 16. GitHub Actions 是发布事务的后半段

写入 GitHub 只是 Source Commit，不等于网页上线。

对于当前站点，完整事务是：

```text
update content/problems.ts
  ↓
commit 到 main
  ↓
Deploy public mirror workflow
  ↓
npm ci
  ↓
npm run build:pages
  ↓
上传 ./out
  ↓
GitHub Pages deploy
  ↓
线上页面验收
```

Agent 必须区分三种状态：

- **Committed**：源码已写入，但构建尚未结束；
- **Deployed**：Actions 成功，Pages 已部署；
- **Verified**：线上页面已实际检查到新内容。

最终回执优先报告最高达到的真实状态，不得把 Committed 说成 Verified。

如果 Actions 失败：

1. 找到与本次 commit 对应的 workflow run；
2. 找失败 job；
3. 读失败 step / log；
4. 判断是本次内容导致的 TS/build 问题，还是外部 infra 问题；
5. 若是本次变更导致，自动修复并提交 corrective commit；
6. 再等待部署；
7. 若无法安全修复，再向用户报告具体阻塞点。

## 17. 线上验收标准

部署成功后，继续验证：

1. `https://walchwil.github.io/` 首页能看到/检索到该题；
2. `https://walchwil.github.io/problems/<slug>/` 可访问；
3. 页面标题与题号正确；
4. 难度、topics、状态正确；
5. foundation 正规定义与祛魅映射没有丢失；
6. Python 代码完整、缩进正常；
7. takeaways 与本次讨论一致；
8. 没有把聊天中的私密/无关内容发布出去。

如果 Pages 部署刚成功但 CDN/缓存仍显示旧内容，可短暂重试；在实际读到新内容前，只报告 `Deployed`，不报告 `Verified`。

## 18. 用户体验：把整条链路压缩成一句话

用户侧理想交互只有：

```text
用户：更新到博客
```

Agent 内部完成：

```text
读取当前讨论
→ 整理学习成果
→ 读取 live repo
→ 找到当前题
→ 映射 schema
→ 安全写入
→ commit
→ 看 Actions
→ 必要时修 build
→ 验证 Pages
→ 回执
```

正常情况下不要把 git 操作步骤反过来交给用户。

只有遇到真正无法自行解决的外部状态时才打断，例如：

- GitHub 未连接/无写权限；
- 当前题无法确定；
- 用户明确存在未 push 的冲突本地修改；
- 发布需要网站架构级改造；
- GitHub/Pages 服务异常。

这就是本 Skill 的最后一公里目标：**学习结束后，用户只负责表达“我要发布”，Agent 负责把已经形成的高质量学习成果安全地送到公开博客。**
