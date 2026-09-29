<h1 align="center">LucidFlow</h1>

<p align="center">Don't guess, ask first — a request-handling flow that forces AI to clarify before executing.</p>

<p align="center">
  <a href="https://github.com/yanzelin99/LucidFlow/releases/tag/v1.0.0"><img src="https://img.shields.io/github/v/release/yanzelin99/LucidFlow" alt="release" /></a>
  <a href="./LICENSE"><img src="https://img.shields.io/github/license/yanzelin99/LucidFlow" alt="license" /></a>
</p>

<p align="center">English | <a href="./README_ZH-CN.md">简体中文</a></p>

This repo's `AGENTS.md` is a drop-in optimized version (≈841 tokens)

## Why

When AI fails, it is usually not incompetence — it is guessing under incomplete information: inventing a default value, betting on the most likely interpretation, trying execution privately and asking only after something breaks.
A correct guess saves one question; a wrong guess wastes a full execution round — and may mutate state or burn huge amounts of tokens.
This flow puts "asking" first: stop when intent is unclear, ask everything at once, then execute with an unambiguous understanding.

## Problems before this flow

- **Blind-guessed parameters**: object, quantity, format unstated → AI invents defaults and builds the wrong thing.
- **Act-first-ask-later**: private trial execution, confirmation only after breakage; state already mutated, rework required.
- **Probability gambling**: one sentence, two readings → AI unilaterally picks the "most likely" one and answers the wrong question.
- **Wasteful burn**: target not even specified, yet a full-scope search runs first — a token and time black hole.
- **Fragmented interrogation**: one question per round; five rounds later the picture is still incomplete.

## Problems solved after adopting it

- **Ask everything at once**: the clarification phase lists ALL pending questions; one answered round locks the picture. No fragmented follow-ups.
- **Doubt means ask**: the clarity check routes anything doubtful to "must clarify" — no more gambling on probabilities.
- **Circuit breaker**: about to violate a prohibition at any step → stop immediately, go clarify. Ambiguity never reaches execution.
- **Reviewable and traceable**: replies go back through the Step 2 re-check; only a state satisfying "direct execution" counts as locked.
- **Token-cheap**: optimized version 841 tokens (was 1151), affordable on every request.

## Rule flow (illustration of the flow only — do not copy and use)

```text
[用户请求输入]
  │
  ├─► [例外判定] 是否为意图明确的纯知识问答 / 闲聊？
  │     │
  │     ├─► 是（无执行风险） ───► [直接回答输出]（流程结束）
  │     │
  │     └─► 否（涉及状态改变/工具执行/外部操作）
  │           │
  │           ▼
  │   【步骤 1：意图解析】（结构化提取五要素）
  │     ├─ 目标：用户想达成什么结果
  │     ├─ 对象：操作或处理的具体目标
  │     ├─ 参数：数量、格式、范围、时间等
  │     ├─ 环境：平台、工具、上下文条件
  │     └─ 约束：限制条件与禁止事项
  │           │
  │           ▼
  │   【步骤 2：清晰度判定】（信息完备性分流点）
  │     │
  │     ├─► [分支 A：判定为可直接执行]
  │     │     │
  │     │     ├─ 满足充分条件（须同时成立）：
  │     │     │    ├─ 五要素齐全（或可从上下文直接推断）
  │     │     │    ├─ 仅存在一种合理解释
  │     │     │    └─ 无歧义表述、无指代不明
  │     │     │
  │     │     └─► 直通 ──► 【步骤 4：直接执行任务】 ──► [交付结果]
  │     │
  │     └─► [分支 B：判定为必须澄清]
  │           │
  │           ├─ 触发判定条件（命中任一即触发）：
  │           │    ├─ 存在两种或以上合理解释
  │           │    ├─ 关键参数缺失（对象/数量/格式未指定）
  │           │    ├─ 指代不明（如"那个文件"无法直接定位用户所指）
  │           │    └─ 操作有不可逆风险但范围未界定
  │           │
  │           ▼
  │   【步骤 3：澄清提问】（前置控熵，严格交互纪律）
  │     │
  │     ├─ 提问执行原则：
  │     │    ├─ 一次性全量列出所有待确认问题（严禁碎片化追问）
  │     │    ├─ 问题简短、具体、可直接回答
  │     │    └─ 多解时给出编号选项（如：选项1 / 选项2）供选择
  │     │
  │     ├─ 防御性约束：
  │     │    └─ 本阶段只提问，绝对不执行任务，不输出无关内容
  │     │
  │     ▼
  │   [等待用户明确答复与确认]
  │     │
  │     └─► 用户反馈补充要素 / 选中选项
  │           │
  │           ▼
  │   【步骤 4：确认后执行】（闭环落地）
  │     │
  │     └─► 基于已锁定的无歧义理解执行任务 ──► [交付结果]（流程结束）


─────────────────────────────────────────────────────────────
沿途严禁行为（全分支熔断机制）
─────────────────────────────────────────────────────────────
  ✖ [禁止盲猜] 严禁自行脑补缺失参数的默认值并直接执行
  ✖ [禁止先斩后奏] 严禁“先私下尝试执行，出问题再回来确认”
  ✖ [禁止概率盲赌] 严禁在存在多解时，单方面选自认为概率最高的一项执行
  ✖ [禁止无效耗损] 严禁在目标未指明时盲目全盘搜索探查（杜绝Token与时间黑洞）
```

## Quick start
Copy the text below and send it to your AI tool:

```text
Please deploy this project's global rules for me. If global rules already exist, ask me about replacing them. https://github.com/yanzelin99/LucidFlow/
```
