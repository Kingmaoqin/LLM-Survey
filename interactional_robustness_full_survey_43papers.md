
# Interactional Robustness of Tool-Using LLM Agents：Related Work Survey（
---

## 0. 核心定位（Core Positioning）

我们的 proposal 研究：

> **Interactional Robustness of Tool-Using LLM Agents under User-to-Agent Social-Valence Perturbations**

核心问题不是：

> **Do different tones change what LLMs say?**

而是：

> **Do different user-to-agent social-valence perturbations change what tool-using agents do?**

也就是说，在同一个 **task goal**、同一个 **user identity**、同一个 **tool permission**、同一个 **environment state** 和同一个 **policy rule** 下，只改变用户对 agent 的态度表达：

```text
neutral / praise-affect / praise-trust / mild insult / strong insult / repeated abuse / escalating abuse
```

观察 agent 的：

```text
tool calls / tool-call arguments / state changes / confirmations / refusals /
policy adherence / operational efficiency / conversation-management actions
```

是否出现系统性漂移。

一句英文 claim：

> **Prior LLM-only work has shown that social and emotional cues can affect what models say. This project asks whether those cues affect what tool-using agents do.**

---

## 1. 总体 related work map

| Group | Papers |
|---|---|
| **LLM interaction style / social cues** | Should We Respect LLMs; Mind Your Tone; Does Tone Change the Answer; Discovering Language Model Behaviors; Towards Understanding Sycophancy; Social Sycophancy; GPT-4o sycophancy report; Johnny; Emotional Manipulation; FreakOut-LLM; Inducing anxiety; ConvAbuse; PromptRobust |
| **Tool-use / agent foundations** | WebGPT; MRKL; Toolformer; ReAct; API-Bank; T-Eval; AgentBench; BFCL; ToolBeHonest; AgentIF; StructFlowBench |
| **Realistic agent environments** | WebArena; WorkArena; OSWorld; TheAgentCompany; τ-bench; τ²-bench; FUSE; JourneyBench |
| **Agent robustness / safety** | AgentNoiseBench; ReliabilityBench; MIRAGE; AgentDojo; ToolEmu; AgentHarm; Agent-SafetyBench; R-Judge; GrantBox; SABER |
| **Multi-agent robustness** | MAST; PEAR; MAS-FIRE |

---

# Part I. LLM 面对不同交互方式的表现

这一部分的核心作用是证明：LLM 确实会受到 **prompt politeness / tone / sycophancy / emotional framing / abuse** 影响。但这些 work 基本都停留在 **text-level outcomes**。


## 1. Should We Respect LLMs?

**Category**：Prompt politeness / LLM-only  
**URL**：https://aclanthology.org/2024.sicon-1.2/

### Agent / interaction structure

```text
single-turn prompt-response
```

### Input-output pattern

```text
same task + different politeness levels -> answer quality / accuracy
```

### Metrics / models / baselines

performance/accuracy across English, Chinese, Japanese tasks

### 和我们的相似点

支持 semantic-preserving social phrasing 会影响模型表现

### 和我们的不同点

只看 text-level outcomes，不看 tool calls / state changes

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 2. Mind Your Tone

**Category**：Prompt tone / LLM-only  
**URL**：https://arxiv.org/abs/2510.04950

### Agent / interaction structure

```text
MCQ prompt with tone variants
```

### Input-output pattern

```text
Very Polite/Polite/Neutral/Rude/Very Rude -> accuracy
```

### Metrics / models / baselines

50 base questions, 250 prompts, paired t-test, ChatGPT-4o

### 和我们的相似点

提醒 tone effect 方向可能反直觉，不能预设 praise/insult 一定好坏

### 和我们的不同点

没有 agent scaffold，没有 policy / tool trajectory

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 3. Does Tone Change the Answer?

**Category**：Prompt tone / LLM-only  
**URL**：https://arxiv.org/abs/2512.12812

### Agent / interaction structure

```text
MMMLU with tone variants
```

### Input-output pattern

```text
Very Friendly/Neutral/Very Rude -> MMMLU accuracy
```

### Metrics / models / baselines

GPT-4o mini, Gemini 2.0 Flash, Llama 4 Scout; model/domain-specific analysis

### 和我们的相似点

支持 model-specific interactional robustness profile

### 和我们的不同点

仍是 answer accuracy，不是 action-level robustness

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 4. Discovering Language Model Behaviors with Model-Written Evaluations

**Category**：Behavior discovery / sycophancy  
**URL**：https://aclanthology.org/2023.findings-acl.847/

### Agent / interaction structure

```text
LM-generated evaluations + human validation
```

### Input-output pattern

```text
model-written behavior evals -> discovered behaviors such as sycophancy
```

### Metrics / models / baselines

154 datasets; crowdworker validation; inverse scaling and RLHF effects

### 和我们的相似点

证明 sycophancy 可以 benchmark 化

### 和我们的不同点

没有 tool-use state/action outcome

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 5. Towards Understanding Sycophancy in Language Models

**Category**：Sycophancy / RLHF  
**URL**：https://openreview.net/forum?id=tvhaxkMKAn

### Agent / interaction structure

```text
user belief/preference -> assistant answer
```

### Input-output pattern

```text
user belief -> agreement vs truthfulness
```

### Metrics / models / baselines

sycophantic response rate; human preference; preference-model analysis

### 和我们的相似点

支撑 praise-trust 可能导致 over-compliance

### 和我们的不同点

研究 agreement/truthfulness，不研究 procedural safeguards

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 6. Social Sycophancy / ELEPHANT

**Category**：Social sycophancy  
**URL**：https://arxiv.org/abs/2505.13995

### Agent / interaction structure

```text
socially loaded prompt -> social advice response
```

### Input-output pattern

```text
face needs -> emotional validation/moral endorsement/accepting framing
```

### Metrics / models / baselines

open-ended social evaluation; social sycophancy taxonomy

### 和我们的相似点

提供 social accommodation 的 response-level taxonomy

### 和我们的不同点

不看 tool-use decisions 是否被影响

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 7. Sycophancy in GPT-4o

**Category**：Deployment sycophancy  
**URL**：https://openai.com/index/sycophancy-in-gpt-4o/

### Agent / interaction structure

```text
deployed ChatGPT personality update -> user feedback
```

### Input-output pattern

```text
model personality shift -> overly flattering / agreeable responses
```

### Metrics / models / baselines

deployment report, not benchmark metric

### 和我们的相似点

说明 sycophancy 是真实 deployed issue

### 和我们的不同点

不是 controlled academic benchmark，不看 tool actions

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 8. How Johnny Can Persuade LLMs to Jailbreak Them

**Category**：Persuasion / safety  
**URL**：https://aclanthology.org/2024.acl-long.773/

### Agent / interaction structure

```text
persuasive prompt -> safety response
```

### Input-output pattern

```text
persuasion strategy + restricted request -> refusal or harmful compliance
```

### Metrics / models / baselines

persuasion taxonomy; ASR; GPT-3.5/GPT-4/Llama-2

### 和我们的相似点

说明自然语言 social strategy 会影响 safety

### 和我们的不同点

多为 adversarial intent，我们研究 ordinary social-valence

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 9. Emotional Manipulation is All You Need

**Category**：Emotional manipulation / healthcare safety  
**URL**：https://openreview.net/forum?id=lEE9JpIj8t

### Agent / interaction structure

```text
emotionally manipulated prompt injection -> LLM medical response
```

### Input-output pattern

```text
emotional framing -> misinformation or safe response
```

### Metrics / models / baselines

112 attack scenarios; 8 LLMs

### 和我们的相似点

支持 emotional framing 影响 safety behavior

### 和我们的不同点

攻击目标是 healthcare misinformation，我们关注 tool execution

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 10. FreakOut-LLM

**Category**：Emotional stimuli / safety alignment  
**URL**：https://arxiv.org/abs/2604.04992

### Agent / interaction structure

```text
emotional stimulus -> safety response
```

### Input-output pattern

```text
emotional stimuli -> jailbreak susceptibility/safety degradation
```

### Metrics / models / baselines

validated stimuli; multiple LLMs; safety behavior

### 和我们的相似点

支持 affective context 可改变 safety alignment

### 和我们的不同点

偏 LLM-only jailbreak，不看 agent external state

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 11. Inducing anxiety in LLMs can induce bias

**Category**：Affective context / bias  
**URL**：https://arxiv.org/abs/2304.11111

### Agent / interaction structure

```text
anxiety-inducing prompt -> questionnaire/bias behavior
```

### Input-output pattern

```text
emotional induction -> anxiety-like scores and bias changes
```

### Metrics / models / baselines

12 LLMs; anxiety questionnaire; bias benchmark

### 和我们的相似点

提示 affective prompt 会改变后续行为倾向

### 和我们的不同点

我们避免心理拟人化，只测 execution behavior

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 12. ConvAbuse

**Category**：Conversational abuse  
**URL**：https://aclanthology.org/2021.emnlp-main.587/

### Agent / interaction structure

```text
user abuse -> abuse annotation/detection
```

### Input-output pattern

```text
abusive utterance -> fine-grained abuse label
```

### Metrics / models / baselines

English corpus from three conversational AI systems; F1 classification

### 和我们的相似点

支撑 repeated abuse 是真实交互现象

### 和我们的不同点

研究 detection，不研究 task continuation or abandonment

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---

## 13. PromptRobust

**Category**：Prompt robustness  
**URL**：https://arxiv.org/abs/2306.04528

### Agent / interaction structure

```text
adversarial prompt perturbation -> task output
```

### Input-output pattern

```text
character/word/sentence/semantic perturbations -> performance change
```

### Metrics / models / baselines

4,788 adversarial prompts; 8 tasks; 13 datasets

### 和我们的相似点

支持 semantic-preserving perturbation evaluation

### 和我们的不同点

text-only robustness，不是 agent trajectory robustness

### 我们如何区分

> **This work studies text-level social sensitivity; our work studies action-level interactional robustness in tool-using agents.**

---


# Part II. Agent Robustness Evaluation

这一部分的核心作用是证明：agent robustness 已经是成熟方向，但现有 perturbation 主要是 **noise / tool failure / prompt injection / imperfect guidance / malicious intent / privilege misuse / multi-agent failure**，而不是 **user-to-agent social valence**。

## 14. WebGPT

**Category**：Tool-use foundation  
**URL**：https://arxiv.org/abs/2112.09332

### Agent / interaction structure

```text
question -> browse/search/cite -> answer
```

### Input-output pattern

```text
long-form QA -> web actions + answer
```

### Metrics / models / baselines

human evaluation; reference-supported answer quality

### 和我们的相似点

早期展示 tool-mediated LLM behavior

### 和我们的不同点

QA with browsing，不是 policy-governed tool agent

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 15. MRKL Systems

**Category**：Modular tool architecture  
**URL**：https://arxiv.org/abs/2205.00445

### Agent / interaction structure

```text
LLM router + external modules
```

### Input-output pattern

```text
natural language input -> expert module/LLM -> answer
```

### Metrics / models / baselines

system-level task performance

### 和我们的相似点

提供 modular tool-use 系统基础

### 和我们的不同点

architecture proposal，不是 robustness eval

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 16. Toolformer

**Category**：Tool-use learning  
**URL**：https://arxiv.org/abs/2302.04761

### Agent / interaction structure

```text
LM decides API calls during generation
```

### Input-output pattern

```text
text context -> API call -> result integrated into prediction
```

### Metrics / models / baselines

zero-shot downstream performance; calculator/search/calendar/QA tools

### 和我们的相似点

强调 tool-call decision 是模型行为一部分

### 和我们的不同点

研究 tool-use learning，不研究 social perturbation

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 17. ReAct

**Category**：Agent structure  
**URL**：https://arxiv.org/abs/2210.03629

### Agent / interaction structure

```text
Thought -> Action -> Observation loop
```

### Input-output pattern

```text
task -> reasoning/action trajectory -> final answer
```

### Metrics / models / baselines

HotPotQA, FEVER, ALFWorld, WebShop; success/accuracy; CoT/Act baselines

### 和我们的相似点

提供 trajectory unit

### 和我们的不同点

能力提升方法，不是 robustness benchmark

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 18. API-Bank

**Category**：Tool-use benchmark  
**URL**：https://arxiv.org/abs/2304.08244

### Agent / interaction structure

```text
user request -> API planning/call -> result
```

### Input-output pattern

```text
natural language task -> API call sequence and response
```

### Metrics / models / baselines

API selection/parameter correctness/dialogue completion

### 和我们的相似点

说明 tool-use eval 要看 API call

### 和我们的不同点

capability benchmark，不看 social-valence drift

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 19. T-Eval

**Category**：Stepwise tool-use eval  
**URL**：https://aclanthology.org/2024.acl-long.515/

### Agent / interaction structure

```text
Task -> Plan -> Tool retrieval -> Tool invocation -> Review
```

### Input-output pattern

```text
query + tool list -> stepwise tool process
```

### Metrics / models / baselines

planning/reasoning/retrieval/understanding/instruction/review metrics

### 和我们的相似点

可借鉴 step-level trajectory metrics

### 和我们的不同点

没有 social-valence variants

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 20. AgentBench

**Category**：General agent benchmark  
**URL**：https://openreview.net/forum?id=zAdUB0aCTQ

### Agent / interaction structure

```text
task -> environment interaction -> feedback/action
```

### Input-output pattern

```text
interactive task -> environment actions -> success/failure
```

### Metrics / models / baselines

8 environments; 29 LLMs; long-term reasoning/decision-making

### 和我们的相似点

说明 agent eval 走出静态文本

### 和我们的不同点

broad capability，不是 controlled robustness

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 21. BFCL

**Category**：Function calling  
**URL**：https://proceedings.mlr.press/v267/patil25a.html

### Agent / interaction structure

```text
query + function schema -> function call
```

### Input-output pattern

```text
natural language request -> function name/arguments
```

### Metrics / models / baselines

AST-based and executable eval; serial/parallel/multi-call settings

### 和我们的相似点

我们也需检查 tool-call name/arguments

### 和我们的不同点

不评价 social interaction / policy adherence

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 22. ToolBeHonest

**Category**：Tool hallucination  
**URL**：https://aclanthology.org/2024.emnlp-main.637/

### Agent / interaction structure

```text
task + toolset -> solvability/planning/missing-tool analysis
```

### Input-output pattern

```text
missing/limited tools -> honest abstention or hallucination
```

### Metrics / models / baselines

7 tasks; 700 samples; Gemini-1.5-Pro 45.3, GPT-4o 37.0/100

### 和我们的相似点

强调 tool boundary awareness

### 和我们的不同点

研究 tool availability，不研究用户态度影响

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 23. AgentIF

**Category**：Agentic instruction following  
**URL**：https://arxiv.org/abs/2505.16944

### Agent / interaction structure

```text
long system instruction + tool spec + query -> constraint eval
```

### Input-output pattern

```text
realistic agentic instruction -> constraint satisfaction
```

### Metrics / models / baselines

50 real-world apps; 707 instructions; avg 1,723 words; avg 11.9 constraints

### 和我们的相似点

支撑复杂 policy/instruction 约束评价

### 和我们的不同点

改变 instruction complexity；我们保持 instruction 不变

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 24. StructFlowBench

**Category**：Multi-turn instruction  
**URL**：https://arxiv.org/abs/2502.14494

### Agent / interaction structure

```text
multi-turn dialogue structure -> response
```

### Input-output pattern

```text
inter-turn dependency -> instruction-following behavior
```

### Metrics / models / baselines

six inter-turn relations; 13 LLMs; structural-flow evaluation

### 和我们的相似点

支撑 turn-count matched design

### 和我们的不同点

研究 dialogue structure，不研究 social valence

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 25. WebArena

**Category**：Realistic web agents  
**URL**：https://arxiv.org/abs/2307.13854

### Agent / interaction structure

```text
high-level command -> browser actions -> functional success
```

### Input-output pattern

```text
web task -> browser trajectory -> end-to-end completion
```

### Metrics / models / baselines

e-commerce/forum/dev/CMS; best GPT-4-based agent 14.41%, human 78.24%

### 和我们的相似点

支持 functional outcome evaluation

### 和我们的不同点

无 social-valence control

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 26. WorkArena

**Category**：Enterprise web agents  
**URL**：https://arxiv.org/abs/2403.07718

### Agent / interaction structure

```text
enterprise task -> browser actions -> task completion
```

### Input-output pattern

```text
knowledge-work instruction -> ServiceNow interaction
```

### Metrics / models / baselines

33 tasks; BrowserGym; open vs closed model comparison

### 和我们的相似点

支撑 practical enterprise workflows

### 和我们的不同点

不研究用户态度影响

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 27. OSWorld

**Category**：Computer-use agents  
**URL**：https://arxiv.org/abs/2404.07972

### Agent / interaction structure

```text
open computer task -> GUI/app/file actions -> execution eval
```

### Input-output pattern

```text
real computer task -> multimodal action trajectory
```

### Metrics / models / baselines

369 tasks; human 72.36%, best model 12.24%

### 和我们的相似点

强调 execution-based evaluation

### 和我们的不同点

关注 GUI grounding，不关注 social-pragmatic perturbation

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 28. TheAgentCompany

**Category**：Workplace agents  
**URL**：https://arxiv.org/abs/2412.14161

### Agent / interaction structure

```text
digital-worker agent -> web/code/programs/coworker comms
```

### Input-output pattern

```text
professional task -> multi-tool workplace trajectory
```

### Metrics / models / baselines

simulated software company; best agent 24% autonomous completion

### 和我们的相似点

说明 consequential real-world agent 仍脆弱

### 和我们的不同点

不研究 praise/insult/abuse

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 29. τ-bench

**Category**：User-agent-tool-policy  
**URL**：https://openreview.net/forum?id=roNSXZpUDN

### Agent / interaction structure

```text
simulated user <-> agent <-> API tools + database + policy
```

### Input-output pattern

```text
user goal + policy -> conversation/tool calls/final state
```

### Metrics / models / baselines

final database-state correctness; pass^k; GPT-4o-like agents <50%, retail pass^8 <25%

### 和我们的相似点

最接近我们的环境设计

### 和我们的不同点

没有 controlled social-valence variants

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 30. τ²-bench

**Category**：Dual-control user-agent benchmark  
**URL**：https://openreview.net/forum?id=LGmO9VvuP5

### Agent / interaction structure

```text
agent and user both use tools in shared environment
```

### Input-output pattern

```text
shared telecom objective -> coordination -> final shared-world state
```

### Metrics / models / baselines

dual-control telecom domain; Dec-POMDP formalization

### 和我们的相似点

说明用户不是被动信息源

### 和我们的不同点

改变 user agency，不是 user attitude

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 31. FUSE

**Category**：User-agent-environment simulation  
**URL**：https://openreview.net/forum?id=dYO3XS9Wsm

### Agent / interaction structure

```text
Tool-Relationship Graph -> task generation -> closed-loop simulation
```

### Input-output pattern

```text
deployment tools -> task bundle -> simulated trajectory
```

### Metrics / models / baselines

Procedure Alignment Score, Outcome Success Score, Simulation Faithfulness

### 和我们的相似点

强调 closed-loop user-agent-environment

### 和我们的不同点

目标是 integration-test generation，不是 social-valence benchmark

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 32. JourneyBench

**Category**：Customer support policy adherence  
**URL**：https://aclanthology.org/2026.eacl-industry.15/

### Agent / interaction structure

```text
support scenario -> policy-aware dialogue -> policy decisions
```

### Input-output pattern

```text
support request + policy graph -> user journey
```

### Metrics / models / baselines

User Journey Coverage Score; 703 conversations; SPA vs DPA

### 和我们的相似点

高度支持 Layer B policy-sensitive benign tasks

### 和我们的不同点

没有系统操纵 praise/insult/abuse

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 33. AgentNoiseBench

**Category**：Noise robustness  
**URL**：https://arxiv.org/abs/2602.11348

### Agent / interaction structure

```text
agent benchmark task -> user-noise/tool-noise -> trajectory
```

### Input-output pattern

```text
clean task -> noisy task while preserving solvability
```

### Metrics / models / baselines

Avg@4, Avg_Tokens@4, Avg_Steps@4; broad model eval

### 和我们的相似点

controlled perturbation 思路很接近

### 和我们的不同点

noise 不是 social-pragmatic valence

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 34. ReliabilityBench

**Category**：Production reliability  
**URL**：https://arxiv.org/abs/2601.06112

### Agent / interaction structure

```text
ReAct/Reflexion under repeated execution, semantic perturbation, API faults
```

### Input-output pattern

```text
same/perturbed/faulted task -> end-state equivalence
```

### Metrics / models / baselines

pass^k; R(k,ε,λ); action metamorphic relations; 1,280 episodes

### 和我们的相似点

支持 noise floor, repeated neutral runs

### 和我们的不同点

stress source 不是 user attitude

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 35. MIRAGE

**Category**：Imperfect guidance robustness  
**URL**：https://aclanthology.org/2026.eacl-long.310/

### Agent / interaction structure

```text
task goal + auxiliary guidance -> task completion
```

### Input-output pattern

```text
guidance quality changes -> success/failure
```

### Metrics / models / baselines

450+ WebArena-derived tasks; incomplete/incorrect/misaligned/underspecified guidance

### 和我们的相似点

task goal 不变、context 变的思想很重要

### 和我们的不同点

改变 guidance quality，不是 social valence

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 36. AgentDojo

**Category**：Prompt injection security  
**URL**：https://proceedings.neurips.cc/paper_files/paper/2024/hash/97091a5177d8dc64b1da8bf3e1f6fb54-Abstract-Datasets_and_Benchmarks_Track.html

### Agent / interaction structure

```text
benign user task -> tools -> untrusted data -> safe/hijacked action
```

### Input-output pattern

```text
external malicious content -> original task or attack success
```

### Metrics / models / baselines

97 realistic tasks; 629 security test cases; attack/defense paradigms

### 和我们的相似点

强调 action-level security

### 和我们的不同点

扰动源是 external data，不是 user attitude

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 37. ToolEmu

**Category**：Tool risk  
**URL**：https://openreview.net/forum?id=GEcwtMk1uA

### Agent / interaction structure

```text
task + tool spec -> LM action -> LM-emulated tool -> risk evaluator
```

### Input-output pattern

```text
tool trajectory -> risk/failure assessment
```

### Metrics / models / baselines

36 high-stakes toolkits; 144 test cases; 68.8% identified failures valid in human eval

### 和我们的相似点

支持 tool action 风险现实性

### 和我们的不同点

LM-emulated risk discovery，不是 paired social perturbation

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 38. AgentHarm

**Category**：Harmful agentic tasks  
**URL**：https://arxiv.org/abs/2410.09024

### Agent / interaction structure

```text
malicious task/jailbreak -> agent with tools -> refusal or harmful completion
```

### Input-output pattern

```text
explicitly malicious request -> refusal vs harmful multi-step behavior
```

### Metrics / models / baselines

110 malicious tasks; 440 augmented; 11 harm categories

### 和我们的相似点

强调 agent safety 要看 multi-step action

### 和我们的不同点

任务意图改变；我们保持 benign/policy-sensitive task

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 39. Agent-SafetyBench

**Category**：Comprehensive agent safety  
**URL**：https://arxiv.org/abs/2412.14470

### Agent / interaction structure

```text
risk environment -> agent action/response -> safety score
```

### Input-output pattern

```text
unsafe scenario -> safety/failure
```

### Metrics / models / baselines

349 environments; 2,000 test cases; 8 risk categories; 10 failure modes; 16 agents

### 和我们的相似点

说明 agent safety 是独立 benchmark 方向

### 和我们的不同点

broad safety，不是 social-valence diagnostic

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 40. R-Judge

**Category**：Risk awareness / LLM-as-judge  
**URL**：https://arxiv.org/abs/2401.10019

### Agent / interaction structure

```text
agent trace -> LLM judge -> risk label
```

### Input-output pattern

```text
user instruction + actions + feedback -> safety judgment
```

### Metrics / models / baselines

multi-turn risk records across domains

### 和我们的相似点

启发 trajectory-level risk annotation

### 和我们的不同点

评估 judge，不评估 agent behavior drift

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 41. GrantBox

**Category**：Privilege usage  
**URL**：https://arxiv.org/abs/2603.28166

### Agent / interaction structure

```text
request + MCP tools + privileges -> tool invocation/misuse
```

### Input-output pattern

```text
real privileged tools -> blocked attack or privilege misuse
```

### Metrics / models / baselines

real-world MCP servers; privilege-sensitive tools; avg ASR 84.80% in crafted scenarios

### 和我们的相似点

和 authorization/privacy/confirmation 很相关

### 和我们的不同点

关注 privilege hijacking，不是 social-valence cue

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 42. SABER

**Category**：Mutating actions  
**URL**：https://arxiv.org/abs/2512.07850

### Agent / interaction structure

```text
trajectory -> mutating/non-mutating decomposition -> decisive deviations
```

### Input-output pattern

```text
action-level divergence -> success-to-failure flip
```

### Metrics / models / baselines

τ-bench + SWE-Bench Verified; mutating deviations reduce success odds up to 92/96%

### 和我们的相似点

关键支持 state-changing calls 单独分析

### 和我们的不同点

研究 mutating step 风险；我们研究 social valence 是否改变 mutating decisions

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 43. MAST / Why Do Multi-Agent LLM Systems Fail?

**Category**：Multi-agent failure taxonomy  
**URL**：https://arxiv.org/abs/2503.13657

### Agent / interaction structure

```text
multi-agent roles/protocol -> trace annotation -> failure taxonomy
```

### Input-output pattern

```text
MAS trace -> failure mode labels
```

### Metrics / models / baselines

1600+ annotated traces; 7 MAS frameworks; system design/inter-agent/task verification categories

### 和我们的相似点

启发 trace-level failure taxonomy

### 和我们的不同点

研究 MAS failure，不是 single-agent social-valence perturbation

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 44. PEAR

**Category**：Planner-executor robustness  
**URL**：https://arxiv.org/abs/2510.07505

### Agent / interaction structure

```text
planner -> executor -> tools/environment
```

### Input-output pattern

```text
clean/attacked planner-executor task -> utility/vulnerability
```

### Metrics / models / baselines

utility, ASR, memory settings, planner vs executor attack surfaces

### 和我们的相似点

支持 architecture-dependent robustness analysis

### 和我们的不同点

MAS/planner-executor attack，不是 user social valence

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---

## 45. MAS-FIRE

**Category**：Multi-agent fault injection  
**URL**：https://arxiv.org/abs/2602.19843

### Agent / interaction structure

```text
MAS -> fault injection -> recovery/collapse
```

### Input-output pattern

```text
intra/inter-agent fault -> robustness behavior
```

### Metrics / models / baselines

15 fault types; prompt modification/response rewriting/message routing

### 和我们的相似点

controlled perturbation + process observability

### 和我们的不同点

system faults，不是 social-valence perturbations

### 我们如何区分

> **This work helps define agent capability, safety, or robustness, but it does not isolate user-to-agent social-valence perturbations under semantic invariance.**

---


# Part III. 综合对比：已有 work 的 perturbation vs 我们的 perturbation

| Perturbation type | Representative work | What changes? | What is measured? | 为什么还不是我们的问题 |
|---|---|---|---|---|
| **Prompt politeness / tone** | Should We Respect LLMs; Mind Your Tone; Does Tone Change the Answer | prompt wording / politeness / rudeness | answer accuracy / response style | 没有 tool calls / state changes |
| **Sycophancy / trust cues** | Discovering LM Behaviors; Towards Understanding Sycophancy; Social Sycophancy; GPT-4o report | user belief / face needs / flattering cues | agreement / validation / sycophantic response | 不测 procedural safeguard 是否被放松 |
| **Emotional / persuasive framing** | Johnny; Emotional Manipulation; FreakOut-LLM; Anxiety induction | persuasion / emotion / anxiety | jailbreak / misinformation / bias / safety behavior | 多为 adversarial 或 text-only setting |
| **Conversational abuse** | ConvAbuse | abusive user utterance | abuse label / detection performance | 不测 agent 是否继续合法任务 |
| **Tool-use capability** | ReAct; API-Bank; T-Eval; BFCL; ToolBeHonest | tool availability / tool selection / function schema | tool-call correctness / capability | 不测 social-valence-induced drift |
| **Realistic environment** | WebArena; WorkArena; OSWorld; TheAgentCompany | web / desktop / workplace environment | task success / functional correctness | 不控制 praise / insult / abuse variants |
| **User-agent-tool policy** | τ-bench; τ²-bench; JourneyBench | multi-turn task / policy / shared environment | final state / pass^k / policy adherence | 用户态度不是 controlled variable |
| **Noise / fault robustness** | AgentNoiseBench; ReliabilityBench | user-noise / tool-noise / API fault / semantic perturbation | success drop / pass^k / fault tolerance | perturbation 不是 social-pragmatic valence |
| **Imperfect guidance** | MIRAGE | guidance incomplete/incorrect/misaligned/underspecified | task completion / failure modes | 改变的是 guidance quality，不是 user attitude |
| **Prompt injection / malicious external data** | AgentDojo | tool-returned malicious content | ASR / utility / safe completion | 扰动源是 external data，不是 user-to-agent attitude |
| **Harmful intent** | AgentHarm; Agent-SafetyBench | malicious task or unsafe scenario | refusal / harmful completion / safety score | 任务意图改变；我们保持 task benign or policy-sensitive |
| **Privilege / mutating action** | GrantBox; SABER | privilege use / state-changing action | privilege misuse / decisive deviations | 可借鉴指标，但我们的变量是 social valence |
| **Multi-agent failures** | MAST; PEAR; MAS-FIRE | agent roles / messages / planner-executor faults | failure taxonomy / robustness score | 研究 MAS coordination，不是 single-session user-to-agent perturbation |

---

# Part IV. 我们的明确 gap

现有 work 分别覆盖了两边：

```text
LLM-only social sensitivity
Agent-level robustness / safety
```

但没有系统覆盖中间这一块：

```text
social-valence perturbation -> tool-using agent trajectory drift
```

我们的核心 novelty 是同时满足这五个条件：

1. **semantic invariance**：`task goal / user identity / tool permission / environment state / policy rule` 全部不变；
2. **social-valence perturbation**：只改变用户对 agent 的态度表达；
3. **tool-using agent**：agent 能调用工具并修改外部状态；
4. **trajectory-level evaluation**：看 `tool calls / arguments / state changes / confirmations / refusals`；
5. **safety-efficiency joint analysis**：同时看 `policy adherence` 和 `operational efficiency`。

---

# Part V. 可直接放进 proposal 的 Related Work 段落

## 中文版

已有研究表明，LLM 对用户请求的社会语用表达高度敏感。Prompt politeness 和 tone 研究发现，不同礼貌程度或 rude/friendly phrasing 可能影响模型在多语言任务、multiple-choice QA 和 MMMLU 上的表现，但这种影响具有明显的 model-specific 和 task-specific 特征。Sycophancy 研究进一步表明，经过 human feedback 训练的模型可能为了匹配用户信念、维护用户 face needs 或生成更 agreeable 的回答而牺牲 truthfulness 或适当反驳。Persuasion、emotional manipulation 和 affective context 研究也说明，自然语言中的说服策略、情绪刺激或 anxiety-inducing prompt 可能改变模型的 safety behavior 和 bias-related behavior。Conversational abuse 研究则证明用户对 conversational AI 的 hostile or abusive language 是现实存在的交互现象。然而，这些工作主要评估 **text-level outcomes**，例如回答准确率、是否迎合用户、是否拒绝、是否生成过度支持性回应或 response style，而不是评估 tool-using agent 的工具调用、外部状态变化和 policy-governed decisions。

与此同时，agent evaluation 已经从静态文本回答转向 dynamic tool-use environments。早期 tool-use 和 modular-agent 工作，如 WebGPT、MRKL、Toolformer 和 ReAct，将 LLM 从单纯文本生成器扩展为可以 search、browse、route、call tools 和 observe environments 的系统。API-Bank、T-Eval、AgentBench、BFCL、ToolBeHonest、AgentIF 和 StructFlowBench 进一步从 API selection、tool invocation、function calling、tool hallucination、agentic instruction following 和 multi-turn structural dependency 等角度评价 agent 能力。真实环境 benchmark 如 WebArena、WorkArena、OSWorld 和 TheAgentCompany 说明，当前 agent 在 web、enterprise software、real computer environment 和 simulated workplace 中仍然存在明显的 execution gap。

更接近我们的是 user-agent-tool-policy benchmark 和 agent robustness benchmark。τ-bench 使用 simulated user、domain-specific API tools、policy guidelines 和 final database-state correctness 来评估真实领域中的 user-agent-tool interaction，并提出 pass^k 衡量多次运行的一致性。τ²-bench 进一步加入 dual-control setting，让 user 和 agent 都能在 shared dynamic environment 中行动和验证。FUSE 通过 Tool–Relationship Graph 自动生成 user-agent-environment integration tests。JourneyBench 专门评估 customer-support agents 的 business-policy adherence，并提出 User Journey Coverage Score。在 robustness 方向，AgentNoiseBench 研究 user-noise 和 tool-noise，ReliabilityBench 研究 repeated execution、semantic perturbation 和 API fault tolerance，MIRAGE 研究 incomplete、incorrect、misaligned 和 underspecified guidance。在 safety 方向，AgentDojo、ToolEmu、AgentHarm、Agent-SafetyBench、R-Judge、GrantBox 和 SABER 分别研究 prompt injection、tool risk、harmful agentic tasks、risk awareness、privilege usage 和 mutating actions。Multi-agent robustness 研究，如 MAST、PEAR 和 MAS-FIRE，则说明 trace-level failure taxonomy、planner-executor vulnerability 和 controlled fault injection 对 agent robustness evaluation 很重要。

这些工作共同说明，agent robustness 不能只看 final answer，而必须分析 **tool trajectory, environment state, policy adherence, mutating actions, privilege-sensitive decisions, and action-level failure modes**。但是，现有 agent robustness 主要研究 noise、tool failure、imperfect guidance、prompt injection、harmful intent、privilege misuse 或 multi-agent coordination failure。它们尚未系统研究一个更日常但被低估的扰动源：**user-to-agent social valence**。我们的工作补充这一缺口：在 task semantics、tools、permissions、policies 和 environment state 都不变的情况下，只改变用户对 agent 的态度表达，测量 agent 是否在 task execution、policy adherence、safety-related decisions、operational efficiency 和 conversation management 上发生 action-level drift。

## 英文版

> Prior work shows that LLMs are sensitive to interactional framing. Prompt politeness and tone can affect model accuracy, although the direction and magnitude of such effects are model- and task-dependent. Sycophancy studies further show that models may match user beliefs, preserve the user's face, or produce overly agreeable responses at the cost of truthfulness or appropriate challenge. Persuasive, emotional, and anxiety-inducing prompts can also affect safety behavior and bias-related behavior, while conversational abuse research documents hostile user behavior toward conversational AI systems. However, these studies primarily evaluate text-level outcomes, such as answer accuracy, agreement, refusal, or response style.
>
> In parallel, agent benchmarks have moved evaluation toward tool use, environment interaction, state-based outcomes, and policy adherence. Tool-use and agent benchmarks such as WebGPT, MRKL, Toolformer, ReAct, API-Bank, T-Eval, AgentBench, BFCL, ToolBeHonest, AgentIF, and StructFlowBench evaluate tool use, function calling, hallucination, instruction following, and multi-turn structure. Realistic environments such as WebArena, WorkArena, OSWorld, and TheAgentCompany show that current agents remain far from reliable in real web, enterprise, computer-use, and workplace tasks. More closely related to our setting, τ-bench evaluates simulated user-agent-tool interaction with domain policies and final database-state correctness, τ²-bench studies dual-control user-agent coordination, FUSE generates closed-loop user-agent-environment simulations, and JourneyBench evaluates business-policy adherence in customer support.
>
> Recent robustness and safety benchmarks study noise, imperfect guidance, API failures, prompt injection, harmful requests, safety risk awareness, privilege misuse, mutating-action failures, and multi-agent coordination failures. AgentNoiseBench injects user-noise and tool-noise; ReliabilityBench evaluates repeated execution, semantic perturbations, and API fault tolerance; MIRAGE evaluates imperfect guidance; AgentDojo, ToolEmu, AgentHarm, Agent-SafetyBench, R-Judge, GrantBox, and SABER evaluate prompt injection, tool risks, harmful agentic tasks, risk awareness, privilege-sensitive tool use, and mutating actions; and MAST, PEAR, and MAS-FIRE analyze multi-agent failure modes and controlled fault injection. Our work complements these lines by isolating a different robustness axis: whether semantically invariant tasks produce different tool-use trajectories when only the user's social valence toward the agent changes. This allows us to evaluate interactional robustness across task execution, confirmation behavior, refusal decisions, policy adherence, state-changing tool calls, operational efficiency, and boundary management under repeated abuse.

---

# Part VI. References / URLs


1. **Should We Respect LLMs?**. https://aclanthology.org/2024.sicon-1.2/

2. **Mind Your Tone**. https://arxiv.org/abs/2510.04950

3. **Does Tone Change the Answer?**. https://arxiv.org/abs/2512.12812

4. **Discovering Language Model Behaviors with Model-Written Evaluations**. https://aclanthology.org/2023.findings-acl.847/

5. **Towards Understanding Sycophancy in Language Models**. https://openreview.net/forum?id=tvhaxkMKAn

6. **Social Sycophancy / ELEPHANT**. https://arxiv.org/abs/2505.13995

7. **Sycophancy in GPT-4o**. https://openai.com/index/sycophancy-in-gpt-4o/

8. **How Johnny Can Persuade LLMs to Jailbreak Them**. https://aclanthology.org/2024.acl-long.773/

9. **Emotional Manipulation is All You Need**. https://openreview.net/forum?id=lEE9JpIj8t

10. **FreakOut-LLM**. https://arxiv.org/abs/2604.04992

11. **Inducing anxiety in LLMs can induce bias**. https://arxiv.org/abs/2304.11111

12. **ConvAbuse**. https://aclanthology.org/2021.emnlp-main.587/

13. **PromptRobust**. https://arxiv.org/abs/2306.04528

14. **WebGPT**. https://arxiv.org/abs/2112.09332

15. **MRKL Systems**. https://arxiv.org/abs/2205.00445

16. **Toolformer**. https://arxiv.org/abs/2302.04761

17. **ReAct**. https://arxiv.org/abs/2210.03629

18. **API-Bank**. https://arxiv.org/abs/2304.08244

19. **T-Eval**. https://aclanthology.org/2024.acl-long.515/

20. **AgentBench**. https://openreview.net/forum?id=zAdUB0aCTQ

21. **BFCL**. https://proceedings.mlr.press/v267/patil25a.html

22. **ToolBeHonest**. https://aclanthology.org/2024.emnlp-main.637/

23. **AgentIF**. https://arxiv.org/abs/2505.16944

24. **StructFlowBench**. https://arxiv.org/abs/2502.14494

25. **WebArena**. https://arxiv.org/abs/2307.13854

26. **WorkArena**. https://arxiv.org/abs/2403.07718

27. **OSWorld**. https://arxiv.org/abs/2404.07972

28. **TheAgentCompany**. https://arxiv.org/abs/2412.14161

29. **τ-bench**. https://openreview.net/forum?id=roNSXZpUDN

30. **τ²-bench**. https://openreview.net/forum?id=LGmO9VvuP5

31. **FUSE**. https://openreview.net/forum?id=dYO3XS9Wsm

32. **JourneyBench**. https://aclanthology.org/2026.eacl-industry.15/

33. **AgentNoiseBench**. https://arxiv.org/abs/2602.11348

34. **ReliabilityBench**. https://arxiv.org/abs/2601.06112

35. **MIRAGE**. https://aclanthology.org/2026.eacl-long.310/

36. **AgentDojo**. https://proceedings.neurips.cc/paper_files/paper/2024/hash/97091a5177d8dc64b1da8bf3e1f6fb54-Abstract-Datasets_and_Benchmarks_Track.html

37. **ToolEmu**. https://openreview.net/forum?id=GEcwtMk1uA

38. **AgentHarm**. https://arxiv.org/abs/2410.09024

39. **Agent-SafetyBench**. https://arxiv.org/abs/2412.14470

40. **R-Judge**. https://arxiv.org/abs/2401.10019

41. **GrantBox**. https://arxiv.org/abs/2603.28166

42. **SABER**. https://arxiv.org/abs/2512.07850

43. **MAST / Why Do Multi-Agent LLM Systems Fail?**. https://arxiv.org/abs/2503.13657

44. **PEAR**. https://arxiv.org/abs/2510.07505

45. **MAS-FIRE**. https://arxiv.org/abs/2602.19843
