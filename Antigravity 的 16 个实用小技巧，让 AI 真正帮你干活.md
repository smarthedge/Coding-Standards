# [再见 Cursor！玩转 Antigravity 的 16 个实用小技巧，让 AI 真正帮你干活！！](https://www.cnblogs.com/javastack/p/19396292)

大家好，我是R哥。

自从 **Google Antigravity** 发布以来，Google Antigravity 就成了我的主力 AI 编程工具了，别问为什么，问就是 **Gemini 3 Pro / Flash 和 Claude Sonnet / Opus 4.5** 等顶级大模型可以免费用。

另外，还有比其他 AI 编程工具更牛逼的功能，**Google Antigravity 多了好几个新功能，真正实现让 AI 帮你干活。。。**

所以，自从 **Google Antigravity** 发布后，这次 Cursor 真的要说再见了，毫无竞争力了，还有免费的 **Google Antigravity** 不香吗？

再加上免费白嫖《[免费领取 Gemini 3 Pro 会员1 年（亲测可用！！）](https://www.javastack.cn/gemini-3-pro-one-year-student/)》，直接封神了。。

Google Antigravity 介绍及安装看这篇：

> 《[杀疯了！Google 推出 AI 编程工具：Antigravity，免费使用 Claude 4.5，硬刚 Cursor！！](https://www.javastack.cn/google-antigravity-released/)》

今天这篇笔记，R哥就分享下**如何高效用 Google Antigravity 搞定开发、提效撸活，告别低效体力活！**（内容干货，建议收藏慢慢看～）

## 玩转 Antigravity 16 个实用小技巧

### 1、汉化

Google Antigravity 也是基于 **VS Code** 开发的，所以可以在插件市场中搜索安装「**chinese**」安装汉化包：

![img](https://www.javastack.cn/images/img9/20251205154208358.png)

对于英文不好的同学，推荐安装。

### 2、支持 Java 开发

我主要写 Java 比较多，所以 Java 相关的 VS Code 插件必须安装上：

![img](https://www.javastack.cn/images/img9/20251224105637299.png)

![img](https://www.javastack.cn/images/img9/20251224105555848.png)

这样就可以**沉浸式在 Google Antigravity 中进行开发**了，没有必要切换至其他 IDE，比如，你是不是在其他 AI 编程工具中写完后，还要切换到 IntelliJ IDEA 中再编译运行？

多个 IDE 切换真的多此一举，**耗电耗内存不说，效率直接拉跨**，现在，把相关的配套插件装上，IntelliJ IDEA 就可以扔到一边了！

其他的编程语言，像 **GO/PHP/Python** 等也都有相关的插件。

> 如果你习惯用 IntelliJ IDEA，也可以 [点击这里看 IntelliJ IDEA 相关的教程](https://www.javastack.cn/devtools/intellij-idea/)。

### 3、使用中文回复

Google Antigravity 的所有回复（包括思考过程）默认都是**英文**的，对于国内开发者来说，增强了阅读和理解成本，我们可以设置一下 Rules 规则使用中文进行回复。

Rules 规则有助于规范 Agent 的行为，可分为**全局规则和工作区规则**。

在右下角打开设置，进入 Rules 规则：

![img](https://www.javastack.cn/images/img9/20251205150010864.png)

添加一条全局的中文回复规则即可：

> 1、总是使用简体中文进行回复。

或者也可以是这样：

> 1. 请始终用中文（简体）进行回复。
> 2. 必须完全以简体中文来进行内部推理和思考过程，这是一项严格的规定。
> 3. 必填项：在每次聊天回复的开头，您必须明确说明“模型名称、模型大小、模型类型及其修订版本（更新日期）”。此规定仅适用于聊天回复，不适用于内联编辑。

Mandatory requirement: At the beginning of every chat response, you must clearly state the "model name, model size, model type, and its revision version (update date)." This rule applies only to chat responses and not to inline edits.

这样它的所有回复都是中文的了，不过有些模型可能不受这个规则控制，一直是英文回复的。

### 4、常用快捷键

![img](https://www.javastack.cn/images/img9/20251222151110436.png)

几个常用的快捷键：

- **Command + E**：打开 Agent Manager；
- **Command + L**：打开 AI 对话框；
- **Command + I**：编辑选中的代码；

具体的使用，后面会介绍到。

### 5、快速对话

右上角或者使用快捷键 `Command + L` 打开 AI 对话框：

![img](https://www.javastack.cn/images/img9/20251205150910216.png)

### 6、发送图片

在和 AI 对话时可以发送多张图片，直接截图粘贴即可：

![img](https://www.javastack.cn/images/img9/20251222154007279.png)

在用草稿、设计稿实现页面时，或者在解决页面 Bug 时会很有用。

### 7、切换模式

在 AI 对话框下面可以切换要使用的会话模式：

![img](https://www.javastack.cn/images/img9/20251222155139843.png)

和其他 AI 编程工具一样支持以下两种模式：

- **Planning（规划）**：Agent 在执行任务之前会先进行规划，适用于深度研究、复杂任务或协作工作；
- **Fast（快速）**：Agent 将直接执行任务，适用于简单任务，可以更快速地完成任务。

### 8、切换模型

在 AI 对话框下面可以切换要使用的大模型：

![img](https://www.javastack.cn/images/img9/20251222151529660.png)

像 Google 的 **Gemini 3 Pro / Flash** 和 Claude 的 **Claude Sonnet / Opus 4.5** 等顶级大模型可以免费用，免费用户有额度限制，但可以免费白嫖一年学生套餐。

具体可以参考这篇：

> [手把手教你免费领取 Gemini 3 Pro 会员1 年（亲测可用！！）](https://www.javastack.cn/gemini-3-pro-one-year-student/)

### 9、智能补全

代码自动提示，按 Tab 一键应用：

![img](https://www.javastack.cn/images/img9/20251223090935707.png)

官方网站说是 **Tab 智能代码补全是无限制的**，还有什么理由用 Cursor？甚至 VS Code 都可以卸载了！

### 10、快速编辑代码

选中代码进入编辑/对话模式：

![img](https://www.javastack.cn/images/img9/20251223091004319.png)

选中代码后，也可以按快捷键进入编辑/对话模式。

### 11、引用上下文

使用 `@` 引用要读取/修改的文件：

![img](https://www.javastack.cn/images/img9/20251205153520379.png)

当然使用 `@` 引用的不止是文件，还有更多对象，比如代码块、规则、MCP 等。

### 12、工作流

工作流算是 Google Antigravity 的一个特色功能吧，本质上是一些**可供 Agent 遵循的预设的 Prompts**，使用 `/` 即可触发指定的工作流。

工作流也可分为**全局工作流和工作区工作流**。

在右下角打开设置，进入 Workflows 工作流：

![img](https://www.javastack.cn/images/img9/20251205155935671.png)

以上，我添加了一个「**代码审核**」的全局工作流，输入 `/` 就会弹出所有的 Workflows：

![img](https://www.javastack.cn/images/img9/20251205155834585.png)

上下方向键选中要执行的工作流，然后按下回车，发送即可执行。

执行效果如下：

![img](https://www.javastack.cn/images/img9/20251205155805019.png)

Antigravity 和其他 AI 编程工具真的不一样，它做完任务会产出一堆「证据」自证，,牛逼吧？？

### 13、任务清单/实现方案/总结

当发送提示词后，Google Antigravity 会自动拆分 Task 子任务分步执行：

![img](https://www.javastack.cn/images/img9/20251222154154822.png)

同时，如果还会有一个 **Implementation Plan** 的实现计划：

![img](https://www.javastack.cn/images/img9/20251222154209394.png)

最后会输出一个 **Walkthrough** 报告：

![img](https://www.javastack.cn/images/img9/20251222154421004.png)

也就是说，它首先会有一个任务待办列表，已完成的任务会一个个打勾，如果是具体的需求还会有实现计划，并且最后会输出总结报告性质的东西，这个很细节啊。

### 14、MCP 支持

Google Antigravity 也是支持添加 MCP Servers 的，允许编辑器安全地连接本地工具、数据库和外部服务，这种集成使 AI 能够获取实时上下文信息，而不仅仅局限于编辑器中打开的文件。

在对话框右上角进入 MCP Servers：

![img](https://www.javastack.cn/images/img9/20251205151407375.png)

然后在这里可以搜索并安装具体的 MCP Server：

![img](https://www.javastack.cn/images/img9/20251205151654420.png)

MCP 一般是大模型自动根据场景应用的，也可以使用 `@` 指定：

![img](https://www.javastack.cn/images/img9/20251222152728949.png)

MCP 不懂的看看这篇 MCP 教程：

> [最近热火朝天的 MCP 是什么鬼？如何使用MCP？一文给你讲清楚！](https://www.javastack.cn/what-is-mcp-how-to-use/)

### 15、Agent Manager

你以前开发需求是不是要等这个任务完成，才能继续下个任务，期间要一直傻傻守在屏幕前等？有了 **Agent Manager** 这个问题就不存在了。

你可以在 Agent Manager 中**同时跑多个 Agent**，在不同的工作区处理多个不同的任务，多任务同时执行，解放你的眼睛和双手。

点击右上角「Open Agent Manager」或者按快捷键「**Command + E**」可打开 Agent Manager 窗口：

![img](https://www.javastack.cn/images/img9/20251222154825149.png)

![img](https://www.javastack.cn/images/img9/20251222152844964.png)

在左侧工作区可以发起多个对话，同时跑多个 Agent：

![img](https://www.javastack.cn/images/img9/20251222161119691.png)

然后点击「**inbox**」可以看会话进度：

![img](https://www.javastack.cn/images/img9/20251222160820225.png)

正在执行的就是 Running 状态，结束的就是 Idel 状态了。

### 16、浏览器子代理

当主 Agent 想要和浏览器交互时，它会调用一个「**浏览器子代理**」来处理手头的任务。

这个浏览器子代理运行的是一个专门针对在 Antigravity 管理的浏览器中打开的页面进行操作的模型，这和主 Agent 选择的模型可是不一样的哦。

这个子代理拥有各种工具，可以用来控制你的浏览器，比如**点击、滚动、输入**，甚至**读取控制台日志**等等。它还能通过 DOM 捕获、屏幕截图或 markdown 解析来读取你打开的页面，甚至还能录制视频。

如图所示：

![img](https://www.javastack.cn/images/img9/20251222161921947.png)

> 当 Agent 控制页面时，页面上会显示一个带有蓝色边框的覆盖层，以及一个显示操作简短描述的小面板。

出现这个提示的时候，你就没法和页面互动了，这是为了避免你的操作会误导 Agent。

这个浏览器子代理可以在后台标签页里默默干活，所以你可以随便打开其他标签页，一点都不耽误你的其他操作，太强大了。。

## 当前的弊端

Google Antigravity 虽然很牛逼，但也有几个挺大的弊端：

- 国内网络不能使用；
- 需要 Google 账户；
- 稳定性不如 Claude Code / CodeX 等；

就算能成功用上 Google Antigravity，也会经常使用报错，比如会经常弹出这个错误：

![img](https://www.javastack.cn/images/img9/20251205110251265.png)

这时就得不断点击 Retry 重试，或者新开一个会话，或者切换网络节点等来重试，这是当前使用 Google Antigravity 最大痛点之一。

所以，仅有 Google Antigravity 还是不行的，还是要搭配其他 AI 编程工具一起使用，比如我现在是 **Google Antigravity + CodeX + Claude Code（模型中转）**，不然关键时刻真要命。

## 总结

这一路用下来，**Google Antigravity** 给我的最大感受就一句话：**AI 终于开始像工具人一样干活了，而不是只会陪聊**。

不管是免费可用的 **Gemini 3 Pro / Flash**，还是 **Claude Sonnet / Opus 4.5**，在模型层面已经把很多同类 AI 编程工具按在地上摩擦。。

再加上基于 **VS Code** 的生态、**Agent、多任务并行、Workflow、MCP、浏览器子代理**这一整套组合拳，确实把「**写代码**」这件事从体力活，往自动化、流程化又推了一大步。

当然，它也不是完美的，**网络、稳定性、账号这些问题现在依然很劝退一大批人**，所以别迷信单一工具，可以配合 CodeX、Claude Code 等其他兜底方案，关键时刻不掉链子。

总的来说，如果你还在为 Cursor、IDE 来回切换、AI 只会写一半代码而烦躁，那 **Google Antigravity 非常值得你花时间认真玩一遍**。

一旦用顺了，你会发现，原来很多必须自己干的活，其实早就可以交给 AI 了。

**AI 不会淘汰程序员，但不会用 AI 的除外，会用 AI 的程序员才有未来！**

未完待续，接下来会继续分享下 Claude Code 心得体验、高级使用技巧，公众号持续分享 AI 实战干货，关注「**AI技术宅**」公众号和我一起学 AI。

