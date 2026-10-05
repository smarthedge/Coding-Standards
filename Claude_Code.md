Claude Code顺手到飞起的10个技巧

Claude Code 最近是真的火。GitHub 上讨论度一直在涨，程序员圈里几乎人手一个，连非技术岗的朋友都来问我这是什么东西。 我从一开始把它当聊天机器人使，到现在它成了干活时的半个搭档，中间踩了不少坑。这篇不讲安装配置，直接上我用下来真觉得实用的 10 个技巧，全放在图里了。

![image-20260803221436641](./Claude_Code.assets/image-20260803221436641.png)

![image-20260803221441724](./Claude_Code.assets/image-20260803221441724.png)

![image-20260803221453827](./Claude_Code.assets/image-20260803221453827.png)

![image-20260803221459335](./Claude_Code.assets/image-20260803221459335.png)

![image-20260803221504542](./Claude_Code.assets/image-20260803221504542.png)

![image-20260803221512112](./Claude_Code.assets/image-20260803221512112.png)



![image-20260803221518475](./Claude_Code.assets/image-20260803221518475.png)

![image-20260803221524477](./Claude_Code.assets/image-20260803221524477.png)

![image-20260803221533261](./Claude_Code.assets/image-20260803221533261.png)

![image-20260803221544492](./Claude_Code.assets/image-20260803221544492.png)

![image-20260803221551043](./Claude_Code.assets/image-20260803221551043.png)


![](Claude_Code.assets/76bfab2f-4f75-4587-9be9-5dc7abfe5e8f.png)


![](Claude_Code.assets/b3cd53b1-16c8-433f-8d3b-0d01a20550c3.png)

![](Claude_Code.assets/737df586-67ac-48f1-be99-0e176db5ba62.png)

Claude Code的“项目结构”
AI 编程真正拉开差距的，可能不是模型本身，而是有没有把项目“整理好”
	
很多人用 Claude Code，就是打开项目，然后一句：
	
“帮我实现这个功能。”
	
短期确实很爽，但项目一大、对话一长，AI 就开始忘记上下文，代码风格也慢慢飘了
	
后来我开始认真整理 Claude Code 的项目结构。
	
CLAUDE.md，我会把它当成项目的“总说明书”。
	
技术栈是什么、项目怎么启动、目录怎么组织、代码遵循什么原则、哪些事情不能做，都尽量在这里讲清楚。
	
rules/ 则继续往下拆。
	
比如：
	
code-style.md 管代码规范
testing.md 管测试要求
api-conventions.md 管接口约定
	
这样就不用每次写 Prompt 都重新解释一遍。
	
commands/ 我更喜欢把它理解成团队自己的快捷工作流。
	
代码 Review、修复 Issue、检查代码，都可以沉淀成固定命令。
	
skills/ 又是另外一个层次。
	
部署、测试、数据处理，这些相对复杂、可以重复使用的能力，可以逐渐变成 Skill。
	
agents/ 则适合把一些专业工作单独拆出去。
	
Code Reviewer 就专心 Review，Security Auditor 就专门检查安全问题。
	
还有 hooks/。
	
在工具执行前后自动做校验、Lint、格式化，甚至直接拦截一些不符合规范的操作。
	
这套东西真正让我感兴趣的地方，不是目录本身。
	
而是它正在把“怎么使用 AI 写代码”，慢慢变成一种工程资产。
	
以前团队沉淀的是代码规范、开发文档、CI/CD。
	
以后可能还要多一层：
	
“怎么让 AI 理解这个项目。”
	
一个成熟的软件项目，不应该每换一个程序员就重新解释一遍。
	
同样，也不应该每开一次 Claude Code，都从零开始教 AI。
	
我现在越来越愿意花时间做这些看起来“不直接产生代码”的事情。
	
因为代码写得快已经越来越容易了。
	
真正难的，是让人和 AI 都能长期、稳定、可控地把代码写下去。

![](Claude_Code.assets/79e940d9-929a-4b40-9235-145ec83ee7ff.png)

![](Claude_Code.assets/d32f01b1-5507-49dd-a7ff-22eff2acdae3.png)

![](Claude_Code.assets/cd467fef-6ed8-48d2-a8d5-3d255fe4f31d.png)

![](Claude_Code.assets/52804b14-28dd-4678-9798-3bff49c7b5c1.png)

![](Claude_Code.assets/696242f0-8260-4e4a-9b5a-d601bfab007e.png)

![](Claude_Code.assets/323be829-870d-4b7e-84f3-308e6d1a494b.png)

![](Claude_Code.assets/0a1f608d-2f8b-4ef7-8d4a-438159658179.png)

![](Claude_Code.assets/ce1f41ec-98bd-4be4-987c-0e1eac2d5199.png)

![](Claude_Code.assets/85fa04f8-63a3-4e2d-9818-288bae429ccb.png)

![](Claude_Code.assets/53660302-1e20-49b7-a68d-b7cc2ac493a6.png)

Anthropic官方教Agent记忆：4 个 md 就够了
用过 Claude Code 两周以上的都知道：它会失忆。执行快，但记忆断片、固执得离谱，换最好的模型、最长的 context
也只能改善，无法根治。
	
最近 Anthropic 官方给 Managed Agents（云端开发者平台）上了 memory stores + dreaming，把 Agent
记忆做成了产品。但有个坑：这俩只在云端，本地 Claude Code 用不了，dreaming 还得 developer access。
	
好消息是——同一套思路，4 个 md + hooks 在本地就能复刻：
· CLAUDE.md 立规矩（每次对话自动加载）
· Memory.md 记笔记（session 结束写一笔，下次先读）
· Learning.md 错题本（踩坑、被纠正、aha moment 都 append）
· Wiki.md 共享墙（多 agent 看同一份背景和约定）
再配 SessionStart / PostToolUse / SessionEnd 三个 hook 强制执行，Claude 才真的不失忆。
	
对照官方：
· memory stores ≈ Memory.md / Wiki.md（官方挂文件系统托管，本地就是几个文本文件 + hooks 自动读写）
· dreaming ≈ 定期让 Claude 重读 Learning.md 去重补全（官方后台自动，本地先手动跑「土版」，以后能脚本定时）
	
记忆不是云端专利，本地 4 个 md + hooks 今天就能跑出持久记忆 + 自我进化。
	
图里是完整 9 页拆解。关注刘铁柱，下期讲本地 agent 实战。

Memory 只放稳定事实，Learning 放被纠正过的坑，Wiki 放共享约定，临时任务千万别写进去，不然记忆会反过来污染决策。

![](Claude_Code.assets/7f6e4191-a3b9-4a19-bbec-563cbb8ff7c7.png)

![](Claude_Code.assets/796bfdac-d5d3-4845-85c4-99fa200a9075.png)

![](Claude_Code.assets/3ee79161-4bbb-4618-94d4-064a8339fd2f.png)

![](Claude_Code.assets/f84d6726-5482-4033-8c07-db8628c8d711.png)

![](Claude_Code.assets/aae1b2e8-0156-49dd-8d7d-d8a8428923e8.png)

![](Claude_Code.assets/18de3ec3-d4f8-418c-b23a-4c4b2ecefb65.png)

