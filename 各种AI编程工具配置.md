各种AI编程工具配置

Zhongjun Qiu元婴开发者

 2026-02-09 01:00:13 2026-02-14 00:52:13 [AI](https://decade.net.cn/categories/AI/)[编程工具](https://decade.net.cn/tags/编程工具/)[配置](https://decade.net.cn/tags/配置/) 1.2k Words 4 Mins 15

介绍 AI（Claude Code、OpenCode、Cursor、Windsurf 等）中各种“让 AI 记住你的习惯”的机制（Rules、Memories、Skills/Workflows、MCP），以及我个人的使用经验和配置建议。



说实话，现在主流的 AI 编码助手（Cursor、Windsurf、Claude Code、OpenCode、Gemini CLI、VS Code Copilot 等）在“让 AI 记住你的习惯”这件事上，叫法五花八门：Rules、Memories、Workflows、Skills、MCP……但剥开表层，它们本质上都在解决同一个核心痛点：

**怎么让 AI 别每次对话都像失忆症患者一样从零开始猜你的需求？**

我从 2024 年 6 月开始密集使用这类工具，到现在主力已经切换到 OpenCode + Claude Code，期间也深度试用过 Cursor、Windsurf 等。实际踩坑一年半后，我把这些机制按“刚性程度”和“使用场景”做了分层。下面分享我目前真实在用的四层体系，以及为什么我把 Cursor 和 Windsurf 基本打入冷宫了。

### 第一层：Rules —— 铁律，打底用的“家规”

几乎每家都有，几乎叫法也差不多：

- Cursor → `.cursorrules`（现在支持拆成 `.rules/` 目录）
- Windsurf → `.windsurfrules`
- Claude Code → `CLAUDE.md`（或 `.claude/rules/` 目录）
- OpenCode 也有类似规则文件机制

适合放什么？真正不变的、必须死守的规范。比如：

- 永远不要用 moment.js，用 date-fns
- 所有组件必须 PascalCase，禁止 default export
- 必须用 pnpm，不准出现 npm install
- 禁用 console.log，统一用自定义 logger
- 代码风格：单引号、semi: false、尾逗号等

**经验**：Rules 写得太多会让上下文爆炸，建议控制在 300–600 token 左右。超过这个量就该拆文件，或者挪到下一层（Memories）。

### 第二层：Memories —— 活的经验教训 + 个人偏好

这是 AI 自己“学”到的东西，比 Rules 更灵活，但也没那么铁面无私。

典型场景：

- 你纠正了它三次“我们项目用 Zustand，不用 Redux”，它慢慢就记住了
- 永远不要自己提交到 main 分支，必须开 PR
- “我们鉴权统一用 JWT + refresh token 机制，别再给我搞 session”
- “这个团队讨厌 any，遇到 any 必须追问类型”
- “UI 组件库优先 shadcn/ui，其次 radix + tailwind，别自己造轮子”

很多工具现在都支持跨会话的 Memories（Claude 的项目记忆、Windsurf 的 memories 目录、OpenCode 的持久化存储等）。它不像 Rules 那样每次强制塞 prompt，而是更像“经验库”，需要时被检索或总结后带入。

**我的建议**：凡是“说过三次以上但不是铁律”的东西，都扔到 Memories 里。它能有效减少你重复纠正的次数。

### 第三层：Workflow vs Skill —— 可重复的 SOP 与智能流程

这层最容易和前两层混淆，但其实功能最强。

- Windsurf 的 **Workflow** 最直白：就是一个手动触发的 SOP。比如 `/deploy` 就自动跑 lint → build → test → deploy 全流程。
- Claude Code 和 OpenCode 的 **Skill** 更进一步：不仅能写步骤，还能带脚本、子流程，甚至支持**语义触发**（你随便说“帮我接入支付”，它自动唤起 payment skill）。

**什么时候该做成 Skill/Workflow？**

只要某个操作你重复做了三次以上，就值得封装：

- 生成单测模板（vitest + react-testing-library 风格）
- 自动写 changelog（根据 git diff 分类 feat/fix/breaking）
- 标准化 PR 描述模板
- 初始化新模块（创建文件夹 + index + types + stories + test）
- 一键修复常见 lint 错误 + 格式化

**我的偏好排序**（2026 年视角）：

1. Claude Code 的 Skill 系统（语义触发最聪明，支持自动调用）
2. OpenCode 的 Skill/Agent 机制（终端友好，灵活度高）

### 第四层：MCP —— 不是内容，是“动手能力”的协议

前面三层都是“告诉 AI 怎么想/怎么写”，MCP（Model Context Protocol）完全不同——它是让 AI **真的能去动手**的通道。

通过 MCP，AI 可以：

- 查数据库（读写 Supabase、PlanetScale）
- 读 Sentry 错误日志
- 操作 Notion 页面
- 调用内部 API
- 甚至控制浏览器、发邮件、改 Figma

MCP 不是你写的内容，而是一个标准协议 + 运行中的 server。Claude Code 和 OpenCode 对 MCP 支持最好，Cursor/Windsurf 也在跟进。

简单类比：

- Rules / Memories / Skills → 给 AI 大脑装知识和流程
- MCP → 给 AI 装手和脚，让它能真正执行外部操作

### 我现在的典型配置（2026 年 2 月）

- **Rules**（`CLAUDE.md` + `.rules/`）：放最硬的规范，控制在 400 token 内
- **Memories**：记录踩过的坑、偏好、历史决策（AI 自动总结 + 我手动补充）
- **Skills**：封装 8–12 个高频复杂流程（支付接入、组件初始化、发布流程、单测生成等），语义触发 + 手动 /skill 都行
- **MCP Servers**：接了 Sentry、Notion、内部 API、数据库查询，基本覆盖了“需要看外部状态”的场景

用下来最爽的两家：**Claude Code**（Skill 生态最成熟，语义触发最准）和 **OpenCode**（终端原生、模型自由度高、轻量），以及 VScode 的 **Copilot**（图形化界面做的很好，最近也有了上下文框架占用查看）