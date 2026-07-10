# Codex 额度耗尽了？先别急着下班 ☺️

你有没有遇到过这种情况：

- Codex 正写到关键位置，额度突然用完；
- 本地线程里堆了大量上下文，Web Chat 却像失忆一样；
- 代码改了一半、测试跑了一半、错误定位了一半；
- 眼看思路还热着，却只能被迫停工。

于是我整理了一套略显“狡猾”，但很实用的临时救急方案：

> 把 Git 仓库当作长期记忆，把 `HANDOFF.md` 当作接力棒，把 Web Chat 当作 Codex 暂时离场后的替补队员。

核心思路很简单：

1. Desktop Codex 在额度耗尽前，把当前目标、已完成内容、错误、修改文件、测试结果和下一步写入结构化交接文件；
2. 代码、文档、Patch 和中间产物进入一个经过脱敏的 AI 镜像仓库；
3. ChatGPT Web 读取仓库与交接材料，继续做分析、审查、方案设计和 Patch 生成；
4. Web 端产物通过 Draft PR、云盘收件箱、GitHub MCP 或自动化工作流写回；
5. Codex 恢复后，再从同一个仓库状态继续工作。

它不是什么官方“无限续杯”功能，也不会凭空增加 Codex 额度。

它只是尽量把一次被迫中断，变成一次相对平滑的换班。

## 先搞清楚：Codex 用尽，不等于整份 ChatGPT 套餐报废

这里有一个很容易被忽略、但对救急方案至关重要的事实：

> ChatGPT 订阅不是只有一个装着所有功能的“总额度桶”。不同产品、模型和工具可能使用不同的 allowance、重置周期与限制机制。

目前官方说明中，Codex、ChatGPT Work、ChatGPT for Excel 和 Workspace Agents 在可用时，会消耗同一个 **agentic usage / credit pool**。也就是说：

```text
Codex Desktop / CLI / IDE / Web
ChatGPT Work
ChatGPT for Excel
Workspace Agents
          ↓
共享 agentic usage / credits
```

因此，Codex 额度耗尽时，受到影响的主要是这一组需要代理执行、长时间操作工具和处理工程任务的功能。

但这并不自动意味着：

- 普通 ChatGPT Web 对话也一起停用；
- GPT-5.6 Sol 的标准聊天额度必然同时归零；
- 文件、图片、语音等其他产品额度也同时归零；
- 你的 ChatGPT 订阅已经失效；
- 除了付费购买额外 credits，什么都不能再做。

OpenAI 也明确说明，ChatGPT 的文件上传、图片生成和语音有各自独立的使用限制、重置周期与提示；这些限制并不直接套用到 Codex。反过来，Codex 的 agentic allowance 用尽，也不能简单理解成所有 ChatGPT 功能一起耗尽。

所以经常会出现这种看起来很神奇、其实符合额度结构的情况：

```text
Codex：额度已用尽，暂时不能继续跑工程任务

ChatGPT Web：
✓ 仍可正常聊天
✓ 仍可读取你手动提供的 HANDOFF、Diff 和源码片段
✓ 仍可分析报错与设计修改方案
✓ 仍可能继续使用 GPT-5.6 Sol
✓ 仍可生成 Patch、测试计划和审查意见
```

也就是说，Codex 下班了，不代表整个 ChatGPT 都下班了。把上下文接过去，Web 端依然可能美美使用 GPT-5.6 Sol 接着干活 ☺️

当然，Web 端能否选择某个具体模型，仍取决于你的套餐、该模型自己的 allowance、灰度开放状态以及当时显示的重置时间。

## Plus、100 美元 Pro 和 200 美元 Pro，到底买到了什么

官方目前把个人 Pro 分成两个档位：

| 套餐 | 相对 Plus 的总体使用 allowance | 核心能力 |
|---|---:|---|
| Plus，20 美元/月 | 基准 | 包含 ChatGPT 高级功能与一定量 Codex/Work 使用 |
| Pro，100 美元/月 | Plus 的 5 倍 | 与 200 美元 Pro 基本拥有相同的核心能力，但 allowance 较低 |
| Pro，200 美元/月 | Plus 的 20 倍 | 目前最高使用档位 |

需要注意：这里的“5 倍”和“20 倍”表达的是官方定义的使用 allowance 档位，**不是承诺每一个功能都精确提供固定条数，也不等于你能把某个任务次数机械地乘以 5 或 20**。

Codex 每次任务消耗多少，会受到以下因素影响：

- 仓库大小；
- 会话长度；
- 任务复杂度；
- 所选模型；
- 本地任务还是云端任务；
- 是否长时间运行、反复搜索或调用工具。

一个短脚本和一次跨大型仓库、持续运行很久的重构，当然不可能按“各算一条消息”来理解。

更准确的购买逻辑是：

### Plus：偶尔使用 Codex

适合每周做几次相对集中的编程任务。官方对 Plus 的描述就是“一周内完成少量聚焦的 coding sessions”。它也包含 GPT-5.6 模型家族在 Codex 中的访问，以及用 credits 延长使用的选项。

### 100 美元 Pro：核心能力基本齐，但续航低于 200 美元档

100 美元 Pro 不是“半个 Pro”或缺少高级模型的阉割版。官方说明两档 Pro 拥有相同的核心能力，主要区别是 allowance：

- 100 美元 Pro：Plus 的 5 倍；
- 200 美元 Pro：Plus 的 20 倍。

因此，100 美元档更像是：

> 我需要 Pro 模型、Codex、Deep Research、文件、Memory 等完整高级能力，但不需要 200 美元档那么高的持续使用量。

### 200 美元 Pro：适合高频、长时间使用

200 美元档是最高使用档位，适合经常跑 Codex、长推理和复杂工作的用户。但它也不等于“所有模型永久绝对无限”。官方明确说明，Pro 上部分模型仍然拥有独立 allowance；达到某个模型的 allowance 后，该模型可能暂时不可用，直到重置。

所以，即使是 200 美元 Pro，也应理解成：

```text
更高的多个额度池
+ 更多高级功能访问
+ 更高的 Codex / agentic usage
≠ 一个永远不会耗尽的万能额度池
```

## GPT-5.6 Sol 为什么还能继续用

GPT-5.6 在标准 ChatGPT 对话中的使用方式，与 Codex 的 agentic usage 不是同一个概念。

目前标准聊天中：

- Medium、High 和 Extra High 由 GPT-5.6 Sol 提供；
- Pro 档由 GPT-5.6 Sol Pro 提供；
- Plus 可使用 Medium 和 High；
- Pro 可使用 Medium、High、Extra High 和 Pro。

手动选择 GPT-5.6 Sol 的推理档位，会消耗标准 ChatGPT reasoning allowance。达到该模型的额度后，ChatGPT 可能回退到较轻量的 Thinking mini，或者暂时隐藏该模型，直到额度重置。

关键就在这里：

> Codex 的 agentic allowance 已经耗尽，并不能据此推断你的 GPT-5.6 Sol 标准聊天 allowance 也已经耗尽。

因此，只要 Web 模型选择器里仍可使用 GPT-5.6 Sol，你就可以让它继续完成很多不依赖本地终端的工作：

- 阅读和总结 `HANDOFF.md`；
- 审查统一 Diff；
- 根据错误日志定位根因；
- 设计修复方案；
- 生成代码 Patch；
- 起草单元测试；
- 检查 API 与类型设计；
- 更新文档和决策记录；
- 为 Codex 恢复后的下一轮执行准备精确任务单。

这正是本方案能成立的额度基础：不是绕过 Codex 限制，而是**把剩余工作重新分配给订阅中仍然可用的另一类能力**。

## 哪些事情 Web 能接，哪些事情别硬装

Web Chat 比较擅长接手：

```text
分析
审查
规划
生成 Patch
编写测试
整理上下文
阅读连接器中的仓库
输出 WEB_HANDOFF
```

但它未必能完整替代 Codex 的：

```text
访问完整本地文件系统
直接运行项目
调用本地终端
真实安装依赖
启动开发服务器
复现操作系统相关 Bug
在本地环境反复测试和修改
```

因此最理想的分工不是假装 Web 就是另一个 Codex，而是：

```text
Codex 有额度时：执行、测试、修改、验证
Codex 没额度时：Web 分析、审查、准备 Patch 和接力材料
Codex 恢复后：验证并落地 Web 产物
```

## 这套方案比较得意的地方 ☺️

不是把整段聊天复制来复制去，也不是每次都向新模型重新讲一遍项目背景。

真正被保存的是：

- 当前项目状态；
- 已确认的技术事实；
- 已做出的设计决策；
- 未完成任务；
- 当前 Diff；
- 测试与报错；
- 下一步最小行动；
- Web 与 Desktop 之间可以验证、可以审计的中间产物。

换句话说，聊天窗口可以换，模型可以换，客户端可以换，但项目状态不要跟着一起失忆。

## 推荐组合

```text
Git 仓库
+ AGENTS.md
+ PROJECT / STATE / DECISIONS / TASKS
+ HANDOFF.md
+ diff.patch
+ ChatGPT Web Project
+ AI 脱敏镜像仓库
+ Draft PR 写回流程
```

读仓库时，优先采用只读 GitHub 连接；写回时，不建议让模型直接碰主分支，而是：

```text
Web 生成修改
→ 创建临时分支
→ 创建 Draft PR
→ 本地测试与人工审查
→ 再决定是否合并
```

这样即使模型理解错了，最多也只是多出一个待审查的 PR，而不是悄悄把主仓库改坏。

## 适合谁

这套方案比较适合：

- 使用 Codex Desktop 做长期项目的人；
- 经常在 Desktop 和 Web 之间切换的人；
- 不想因为额度突然耗尽而丢失工作节奏的人；
- 希望 AI 能读代码，但又不愿意把完整私有仓库和敏感信息全部暴露出去的人；
- 愿意接受“AI 提交修改，人类最终审批”的工作方式的人。

## 先说好：它只是救急方案

这套方法不保证效果。

不同模型对大型仓库、复杂 Patch 和长期上下文的理解能力并不一致；Web Chat 也不能完全替代本地 Codex 的文件操作、终端执行、测试和调试能力。

它也不保证能用多久。

ChatGPT、Codex、GitHub Connector、MCP、Actions、额度规则和产品权限都可能调整。今天可行的路径，未来可能被限制、改名、迁移，或者被更好的官方功能取代。

所以请把它理解为：

> 在当前产品边界下，为 Codex 额度耗尽准备的一条临时逃生通道，而不是永久漏洞，更不是无限额度方案。

能多救回来一轮思路、多完成一次审查、少重新解释一次项目背景，就已经赚到了 ☺️

## 官方依据（截至 2026-07-10）

- OpenAI Help：Using Codex with your ChatGPT plan  
  https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan
- OpenAI Help：About ChatGPT Pro tiers  
  https://help.openai.com/en/articles/9793128-about-chatgpt-pro-tiers
- OpenAI Help：GPT-5.6 in ChatGPT  
  https://help.openai.com/en/articles/20001354
- OpenAI Codex pricing  
  https://developers.openai.com/codex/pricing

额度与产品规则变化很快，请以账号中实际显示的 usage 页面、模型选择器、额度提示和重置时间为准。

## 完整工程方案

详细的上下文结构、脱敏镜像仓库、Secret 扫描、GitHub MCP、n8n、云盘 Patch 收件箱和安全写回流程，参见同目录下：

`chatgpt-web-codex-handoff-and-secure-writeback.md`
