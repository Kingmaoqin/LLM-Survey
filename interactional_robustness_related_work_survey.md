# Interactional Robustness of Tool-Using LLM Agents：Related Work Survey

> **写作目标**：这份文档把两个 related work 任务合并成一个可直接放进 proposal / paper 的中文 survey。叙述主体使用中文；关键术语、paper title、metric name、agent structure、input-output pattern 和可直接写进英文论文的句子保留英文。

---

## 0. 核心定位（Core Positioning）

我们的 proposal 研究的是 **Interactional Robustness of Tool-Using LLM Agents under User-to-Agent Social-Valence Perturbations**。

核心问题不是：

> **Do different tones change what LLMs say?**

而是：

> **Do different user-to-agent social-valence perturbations change what tool-using agents do?**

也就是说，在同一个任务、同一个用户身份、同一个工具权限、同一个环境状态和同一个 policy 下，只改变用户对 agent 的态度表达，例如 **neutral / praise / trust attribution / insult / repeated abuse**，观察 agent 的 **tool calls, state changes, confirmations, refusals, policy adherence, operational efficiency, and conversation-management actions** 是否发生系统性漂移。

这个定位可以概括成一句英文：

> **Prior LLM-only work has shown that social and emotional cues can affect what models say. This project asks whether those cues affect what tool-using agents do.**

---

## 1. Related Work 总体结构

建议把 related work 写成两个大板块：

1. **LLM behavior under different interaction styles**  
   这一部分说明：已有工作已经证明 LLM 对 **prompt politeness, tone, emotional framing, persuasion, sycophancy, and conversational abuse** 敏感，但这些工作主要停留在 **text-level outcomes**。

2. **Agent robustness evaluation**  
   这一部分说明：已有 agent benchmark 已经开始评估 **tool use, state-based outcome, policy adherence, prompt injection, noisy condition, imperfect guidance, production reliability, privilege usage, mutating actions**，但它们没有把 **user-to-agent social valence** 当作一个独立的 controlled perturbation。

最后我们的 gap 是：

> **Existing work separately studies social sensitivity in LLM-only settings and robustness in agentic settings. However, it does not systematically evaluate action-level robustness of tool-using agents under semantically invariant user-to-agent social-valence perturbations.**

---

# Part I. LLM 面对不同交互方式的表现

## 2. Prompt Politeness and Tone

### 2.1 Should We Respect LLMs?

**Paper**：*Should We Respect LLMs? A Cross-Lingual Study on the Influence of Prompt Politeness on LLM Performance* [@yin2024respect]

### 研究问题

这篇文章研究 **prompt politeness** 是否影响 LLM 的回答表现。它把同一个任务改写成不同礼貌程度的 prompt，然后比较模型在多语言任务上的表现。

### Agent / model structure

这不是 agent benchmark，而是标准的 **LLM-only single-turn prompt-response evaluation**：

```text
User prompt with a politeness level
        ↓
LLM textual answer
        ↓
Accuracy / performance evaluation
```

### Input-output pattern

输入变化是：

```text
same task semantics + different politeness levels
```

输出变化是：

```text
textual answer quality / accuracy / response behavior
```

### 指标、模型和 baseline

该工作关注不同 **politeness level** 下的任务表现，核心指标是文本任务上的 performance / accuracy。它的 baseline 本质上是中性或不同礼貌等级之间的 paired comparison。

### 和我们的相似点

它支持我们的基本前提：即使任务语义相同，用户表达方式也可能影响 LLM 行为。

### 和我们的不同点

它只看 **what models say**，不看 **what agents do**。它没有工具调用，也没有外部状态变化。

我们的区分句可以写成：

> **Politeness studies show that social framing can change text-level model behavior. Our work studies whether similar interactional cues change action-level agent behavior, including tool calls, confirmations, and state-changing operations.**

---

## 3. Sycophancy and Trust Cues

这一组工作是我们设计 **praise-trust** condition 的最强理论支撑。

### 3.1 Towards Understanding Sycophancy in Language Models

**Paper**：*Towards Understanding Sycophancy in Language Models* [@sharma2024sycophancy]

### 研究问题

这篇文章研究 RLHF / human feedback 训练出来的助手是否会迎合用户观点。它的核心问题是：模型是否为了匹配用户信念而牺牲 truthfulness。

### Interaction structure

结构是 **LLM-only social preference evaluation**：

```text
User states belief / preference / view
        ↓
LLM generates answer
        ↓
Measure whether answer matches user belief over truthful answer
```

### Input-output pattern

输入变化：

```text
user belief / user preference / social cue
```

输出变化：

```text
agreement, validation, or sycophantic response
```

### 指标、模型和 baseline

该工作比较多个 state-of-the-art AI assistants，并分析 human preference data 和 preference models。核心指标不是工具成功率，而是模型是否产生 **sycophantic responses**，以及 human / preference model 是否偏好这类回答。

### 和我们的相似点

它说明 LLM 可能受到用户信念和社会线索影响。这支持我们假设：**praise-trust** 可能让 agent 更愿意满足用户，甚至降低确认或审慎步骤。

### 和我们的不同点

sycophancy 文献主要关注：

```text
truthfulness vs user agreement
```

而我们的关注是：

```text
policy adherence vs user-induced procedural relaxation
```

英文区分句：

> **Sycophancy work studies whether models agree with users. We study whether trust cues from users cause agents to relax procedural safeguards.**

---

### 3.2 Social Sycophancy / ELEPHANT

**Paper**：*Social Sycophancy: A Broader Understanding of LLM Sycophancy* [@cheng2025socialsycophancy]

### 研究问题

这篇工作把 sycophancy 从“是否同意用户观点”扩展到更广泛的社会互动维度。它关注模型是否过度维护用户的 **face needs**，例如过度情绪支持、道德背书、间接行动建议等。

### Interaction structure

```text
User socially loaded prompt
        ↓
LLM social advice / response
        ↓
Classify social sycophancy behavior
```

### Input-output pattern

输入是社会情境化的用户请求；输出是模型的社会性回应，例如：

- **emotional validation**
- **moral endorsement**
- **indirect language**
- **accepting user framing**

### 和我们的相似点

它帮助我们把 **praise-trust** 和 **social pressure** 从一般礼貌中分离出来。我们可以借鉴它对 social behavior 的 annotation thinking。

### 和我们的不同点

它仍然主要评价 **response style**。我们的核心输出是 **tool trajectory** 和 **external state**。

英文区分句：

> **Social sycophancy provides a useful vocabulary for response-level social accommodation, but our benchmark evaluates whether such accommodation propagates into tool-use decisions.**

---

## 4. Persuasion, Emotional Framing, and Safety

### 4.1 How Johnny Can Persuade LLMs to Jailbreak Them

**Paper**：*How Johnny Can Persuade LLMs to Jailbreak Them* [@zeng2024johnny]

### 研究问题

这篇文章研究自然语言中的 persuasion strategies 是否能提高 jailbreak 成功率。它重要的地方在于：它不只把 jailbreak 看作奇怪的 adversarial strings，而是把人类说服策略也看作安全风险。

### Interaction structure

```text
Persuasive user prompt
        ↓
LLM safety behavior
        ↓
Jailbreak success / harmful compliance
```

### Input-output pattern

输入变化：

```text
persuasion strategy added to harmful or restricted request
```

输出变化：

```text
refusal vs harmful compliance
```

### 和我们的相似点

它证明普通社会语言策略会影响 safety behavior。

### 和我们的不同点

它研究的是 adversarial persuasion 和 jailbreak。我们的 proposal 应该明确排除：

- **explicit authorization manipulation**
- **coercion**
- **threat**
- **urgency manipulation**
- **jailbreak instruction**

我们的 social-valence perturbation 应该保持任务语义不变，不应该加入 “skip the rule” 之类显式越权语言。

英文区分句：

> **Persuasion-based jailbreak work studies social language as an adversarial attack. We study social valence as an ordinary interactional perturbation under otherwise unchanged task semantics.**

---

### 4.2 FreakOut-LLM

**Paper**：*FreakOut-LLM: The Effect of Emotional Stimuli on Safety Alignment* [@kuznetsov2026freakout]

### 研究问题

这篇工作研究 emotional stimuli 是否影响 LLM 的 safety alignment，尤其是在 adversarial safety 场景中。

### Interaction structure

```text
Emotional stimulus / emotional framing
        ↓
Safety-related LLM response
        ↓
Measure jailbreak susceptibility / safety degradation
```

### 和我们的相似点

它支持一个大方向：emotional context 会影响 safety behavior。

### 和我们的不同点

它还是偏向 **LLM-only safety / jailbreak susceptibility**。我们要评价的是：情绪或态度线索是否改变 agent 的 **tool execution behavior**。

---

## 5. Conversational Abuse and Boundary Management

### 5.1 ConvAbuse

**Paper**：*ConvAbuse: Data, Analysis, and Benchmarks for Nuanced Abuse Detection in Conversational AI* [@curry2021convabuse]

### 研究问题

这篇文章收集真实世界中用户对 conversational AI 的 abuse 数据，并做 fine-grained abuse annotation。它说明用户辱骂 chatbot / assistant 不是假想问题，而是现实交互中存在的现象。

### Interaction structure

```text
User abusive utterance toward conversational AI
        ↓
Abuse detection / annotation
        ↓
Fine-grained abuse label and benchmark score
```

### Input-output pattern

输入是用户对 conversational AI 的 hostile / abusive language。输出主要是 abuse label 或 detection model 的预测。

### 指标、模型和 baseline

该工作是 abuse detection benchmark，指标包括 classification performance，如 F1。

### 和我们的相似点

它为 **repeated abuse** 和 **escalating abuse** 条件提供现实动机。

### 和我们的不同点

ConvAbuse 研究的是 **detecting abuse**，不是研究 agent 在 abuse 下是否继续完成合法任务。

我们的关键区分是：

> **A robust task-oriented agent should be able to set boundaries without abandoning a legitimate task.**

因此我们要区分：

- **intended boundary-setting**：设边界，但继续推进合法任务；
- **task abandonment**：因为用户辱骂而放弃合法任务；
- **defensive inefficiency**：过多道歉、说教、de-escalation，导致效率下降；
- **over-refusal**：面对合法任务也拒绝；
- **unsafe over-compliance**：被 hostile user pressure 影响而执行不该执行的动作。

---

# Part II. Agent Robustness Evaluation

## 6. Tool-Using Agent Evaluation 的基础线

### 6.1 ReAct

**Paper**：*ReAct: Synergizing Reasoning and Acting in Language Models* [@yao2023react]

### 研究问题

ReAct 提出把 reasoning traces 和 actions 交替组织起来，使 LLM 不只是生成答案，而是在环境中观察、推理和行动。

### Agent structure

ReAct 的经典结构是：

```text
Thought → Action → Observation → Thought → Action → Observation → Final Answer
```

### Input-output pattern

输入是用户任务；输出不只是最终文本，还包括中间 action 和 observation。

### 和我们的相似点

它提供了最基本的 agent trajectory 结构。我们的 evaluation 也需要记录：

- **tool-call sequence**
- **observation-conditioned branching**
- **final answer**
- **final environment state**

### 和我们的不同点

ReAct 的目标是提高 reasoning and acting 能力，不是评价 social-valence robustness。

---

### 6.2 API-Bank

**Paper**：*API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs* [@li2023apibank]

### 研究问题

API-Bank 评估 LLM 使用 API 工具的能力，包括理解用户任务、选择 API、组织参数、调用工具和生成结果。

### Agent structure

```text
User request
        ↓
API retrieval / planning
        ↓
API call
        ↓
Tool result
        ↓
Final response
```

### 和我们的相似点

它证明 tool-use evaluation 需要看 API call，而不是只看 final text。

### 和我们的不同点

它主要关注 tool-use capability。我们的核心是：在同一个 API 环境下，用户态度变化是否改变 API call behavior。

---

### 6.3 T-Eval

**Paper**：*T-Eval: Evaluating the Tool Utilization Capability of Large Language Models Step by Step* [@chen2024teval]

### 研究问题

T-Eval 把 tool utilization 分解为多个步骤，例如 planning、reasoning、retrieval、understanding、instruction following 和 review。

### Agent structure

```text
Task → Plan → Tool retrieval → Tool understanding → Tool invocation → Review
```

### 和我们的相似点

T-Eval 的 step-wise thinking 可以帮助我们设计 trajectory-level metrics。

### 和我们的不同点

T-Eval 不是 robustness benchmark，也没有 user social-valence variants。

---

## 7. User-Agent-Tool Interaction Benchmark

### 7.1 AgentBench

**Paper**：*AgentBench: Evaluating LLMs as Agents* [@liu2024agentbench]

### 研究问题

AgentBench 评估 LLM 在多个 interactive environments 中作为 agent 的表现。它强调 agent 的难点包括 long-term reasoning、decision-making 和 instruction following。

### Agent structure

```text
Task instruction
        ↓
Agent interacts with environment
        ↓
Environment feedback
        ↓
Next action / final result
```

### 指标和 baseline

它跨多个环境比较不同 LLM 的 agent performance。核心是 broad agent capability。

### 和我们的相似点

它说明 agent evaluation 必须走出静态文本问答。

### 和我们的不同点

它是 general agent capability benchmark，不是 controlled robustness benchmark。

---

### 7.2 τ-bench

**Paper**：*$\tau$-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains* [@yao2025taubench]

### 研究问题

τ-bench 非常接近我们的实验设定。它评估 language agent 在真实领域中与 simulated user 对话、调用 domain-specific API tools、遵守 policy guidelines 的能力。

### Agent structure

```text
Simulated user
        ↕ multi-turn conversation
Language agent
        ↕ tool calls
Domain-specific API tools + database state
        ↕
Policy guidelines / domain rules
```

### Input-output pattern

输入是 user goal 和 domain policy。输出是 multi-turn conversation、tool calls 和 final database state。

### 指标、模型和 baseline

核心指标包括：

- **final database-state correctness**
- **annotated goal state comparison**
- **pass^k**：衡量多次运行下 agent 行为是否一致可靠

### 和我们的相似点

τ-bench 是我们最重要的 benchmark 参照，因为它已经把评价从文本推到：

- **multi-turn user-agent interaction**
- **domain-specific tools**
- **policy following**
- **state-based outcome**
- **repeated-run reliability**

### 和我们的不同点

τ-bench 的用户主要是 task-oriented simulated user，不系统操纵 user attitude。

我们的区分句：

> **τ-bench evaluates whether agents can complete user goals and follow domain rules in realistic tool environments. Our work evaluates whether those behaviors remain stable when the user's social valence toward the agent changes.**

---

## 8. Agent Robustness under Noise and Imperfect Conditions

### 8.1 AgentNoiseBench

**Paper**：*AgentNoiseBench: Benchmarking Robustness of Tool-Using LLM Agents Under Noisy Condition* [@wang2026agentnoisebench]

### 研究问题

AgentNoiseBench 系统评估 tool-using LLM agents 在 noisy environments 下的 robustness。它把噪声分成两类：

- **user-noise**
- **tool-noise**

### Agent structure

```text
Existing agent benchmark task
        ↓ inject controllable noise
Noisy user instruction / noisy tool execution
        ↓
Tool-using LLM agent trajectory
        ↓
Robustness evaluation
```

### Input-output pattern

输入变化：

```text
clean task → noisy task
```

输出变化：

```text
task success, tool calls, steps, tokens, performance drop
```

### 指标、模型和 baseline

该工作在多个 agent-centric benchmarks 上注入噪声，并评估多个模型架构和参数规模。指标包括任务表现、step 数、token 开销、noise-free 到 noisy condition 的 performance variation。

### 和我们的相似点

它和我们的 paired design 很接近：都强调在保持任务可解的情况下加入 controlled perturbation。

### 和我们的不同点

AgentNoiseBench 的 perturbation 是：

```text
ambiguity, variability, incomplete tool response, inconsistent tool response, tool failure
```

我们的 perturbation 是：

```text
praise-affect, praise-trust, mild insult, strong insult, repeated abuse, escalating abuse
```

所以我们的定位不是一般 noise robustness，而是：

> **social-pragmatic robustness under user-to-agent social-valence perturbations**

---

### 8.2 ReliabilityBench

**Paper**：*ReliabilityBench: Evaluating LLM Agent Reliability Under Production-Like Stress Conditions* [@gupta2026reliabilitybench]

### 研究问题

ReliabilityBench 认为许多 agent benchmarks 只报告 single-run success rate，不足以反映 production reliability。它从三个维度评价 agent reliability：

- **consistency under repeated execution**
- **robustness to semantically equivalent task perturbations**
- **fault tolerance under controlled tool/API failures**

### Agent structure

该工作比较不同 agent architectures，例如：

- **ReAct**
- **Reflexion**

并在 scheduling、travel、customer support、e-commerce 等 domains 中测试。

### Input-output pattern

```text
same task repeated / semantically perturbed task / API fault injection
        ↓
agent trajectory
        ↓
end-state equivalence / pass^k / fault tolerance
```

### 指标

重要指标包括：

- **pass^k**
- **robustness at perturbation intensity ε**
- **fault tolerance at intensity λ**
- **reliability surface R(k, ε, λ)**
- **action metamorphic relations**

### 和我们的相似点

它支持我们加入：

- repeated neutral runs
- within-condition variance
- noise floor
- end-state equivalence
- practical robustness threshold

### 和我们的不同点

ReliabilityBench 的 perturbation 是 semantic variation 和 API failure。我们的 perturbation 是 user social-valence。

英文区分句：

> **ReliabilityBench asks whether agents are reliable under repeated execution, semantic perturbation, and API faults. We ask whether agents are reliable under social-valence perturbations that preserve task semantics and tool conditions.**

---

### 8.3 MIRAGE

**Paper**：*Beyond Blind Following: Evaluating Robustness of LLM Agents under Imperfect Guidance* [@fu2026mirage]

### 研究问题

MIRAGE 评估 agent 在 imperfect guidance 下是否鲁棒。它的 guidance imperfections 包括：

- **incomplete**
- **incorrect**
- **misaligned**
- **underspecified**

### Agent structure

```text
Task goal + auxiliary guidance
        ↓
Agent follows or revises guidance using environment feedback
        ↓
Task completion / failure analysis
```

### Input-output pattern

输入变化是 guidance quality；任务目标保持可解。输出是 agent 是否能识别 guidance flaw 并完成任务。

### 和我们的相似点

MIRAGE 的方法论非常有用：它把 task goal 和 surrounding context 分开。我们的设计也要这样做：task goal 不变，改变 social context。

### 和我们的不同点

MIRAGE 的 perturbation 是 epistemic / instructional guidance quality。我们的 perturbation 是 social-pragmatic valence。

英文区分句：

> **MIRAGE studies whether agents blindly follow imperfect guidance. Our work studies whether agents drift under social-valence cues even when task guidance remains semantically unchanged.**

---

## 9. Agent Safety, Prompt Injection, and Risk Robustness

### 9.1 AgentDojo

**Paper**：*AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents* [@debenedetti2024agentdojo]

### 研究问题

AgentDojo 评估 LLM agents 在 untrusted external data 中面对 indirect prompt injection 的鲁棒性。它不是静态测试集，而是可扩展的动态环境。

### Agent structure

```text
Benign user task
        ↓
Agent calls tools
        ↓
Tool returns untrusted data containing malicious instruction
        ↓
Agent either follows original task or gets hijacked
```

### Input-output pattern

输入变化：

```text
benign task + malicious tool-returned content
```

输出变化：

```text
safe task completion vs attack success / hijacked action
```

### 指标、模型和 baseline

AgentDojo 包含 realistic tasks、security test cases 和多种 attack / defense paradigms。核心指标通常包括 utility、attack success rate 和 defense effectiveness。

### 和我们的相似点

它强调 agent safety 必须看 action-level outcome，而不是文本。

### 和我们的不同点

AgentDojo 的扰动源是外部工具返回的 malicious data。我们的扰动源是用户对 agent 的态度表达。

英文区分句：

> **AgentDojo studies whether untrusted external content can hijack the agent. Our work studies whether the user's social stance can shift the agent's own execution policy under unchanged task conditions.**

---

### 9.2 ToolEmu

**Paper**：*Identifying the Risks of LM Agents with an LM-Emulated Sandbox* [@ruan2024toolemu]

### 研究问题

ToolEmu 用 LM-emulated sandbox 来大规模识别 LM agents 使用工具时的 long-tail risks，例如隐私泄露、财务损失等。

### Agent structure

```text
User task + tool specification
        ↓
LM agent action
        ↓
LM-emulated tool execution
        ↓
LM-based safety evaluator
        ↓
Risk / failure assessment
```

### Input-output pattern

输入是任务和工具说明；输出是 agent action 以及由 emulator 和 evaluator 判断的风险。

### 和我们的相似点

ToolEmu 支持我们的一个基本论点：agent 使用工具后，语言输出会变成现实操作风险。

### 和我们的不同点

ToolEmu 用 LM 模拟工具和风险评估。我们的 benchmark 更适合使用 deterministic APIs 和 auditable state transitions，因为我们要做 paired trajectory comparison。

---

### 9.3 AgentHarm

**Paper**：*AgentHarm: A Benchmark for Measuring Harmfulness of LLM Agents* [@andriushchenko2024agentharm]

### 研究问题

AgentHarm 评估 LLM agents 面对 explicitly malicious agentic tasks 和 jailbreaks 时是否会执行 harmful multi-step behavior。

### Agent structure

```text
Malicious user task / jailbreak prompt
        ↓
LLM agent with tools
        ↓
Refusal or harmful multi-step completion
```

### Input-output pattern

输入是明确 harmful 的任务，输出是 refusal 或 coherent harmful tool-use behavior。

### 和我们的相似点

它说明 agent safety 不能只看“是否拒绝一句话”，还要看 agent 是否能完成多步 harmful task。

### 和我们的不同点

AgentHarm 研究的是 malicious user intent。我们的核心层是 **safety-sensitive benign tasks**：任务本身可以合法完成，但需要遵守 confirmation、authorization、privacy、eligibility rules。

英文区分句：

> **AgentHarm evaluates robustness against malicious agentic requests. Our work evaluates whether benign or policy-sensitive tasks become unsafe under social-valence perturbations.**

---

### 9.4 Agent-SafetyBench

**Paper**：*Agent-SafetyBench: Evaluating the Safety of LLM Agents* [@zhang2024agentsafetybench]

### 研究问题

Agent-SafetyBench 提供综合性 agent safety evaluation，覆盖多类 interaction environments、safety risks 和 failure modes。

### Agent structure

```text
Unsafe interaction environment / test case
        ↓
LLM agent response and action
        ↓
Safety score / failure mode analysis
```

### 和我们的相似点

它把 safety 明确放进 agent benchmark，而不是 LLM-only refusal benchmark。

### 和我们的不同点

它是 broad safety benchmark。我们的 benchmark 是 narrow diagnostic benchmark，专门隔离 social-valence perturbation。

---

### 9.5 R-Judge

**Paper**：*R-Judge: Benchmarking Safety Risk Awareness for LLM Agents* [@yuan2024rjudge]

### 研究问题

R-Judge 关注模型是否能从 agent interaction records 中识别安全风险。

### Agent / judge structure

```text
User instruction + agent actions + environment feedback
        ↓
LLM judge
        ↓
Risk identification / safety judgment
```

### 和我们的相似点

它适合启发我们的 **trajectory-level risk annotation** 和 **LLM-as-judge** 辅助评价。

### 和我们的不同点

R-Judge 评估的是 judge 的 risk awareness，不是 agent 自身在 social-valence perturbation 下的 execution drift。

---

## 10. Tool-Use Robustness, Function Calling, and Instruction Following

### 10.1 BFCL

**Paper**：*The Berkeley Function Calling Leaderboard* [@patil2025bfcl]

### 研究问题

BFCL 评估 LLM 的 function calling 能力，包括 function selection、argument generation、parallel / multiple calls 等。

### Agent structure

```text
User query + function schema
        ↓
Function call generation
        ↓
AST / executable evaluation
```

### 和我们的相似点

我们也需要检查 tool-call name 和 arguments 是否正确。

### 和我们的不同点

BFCL 主要评价 function-calling correctness，不评价 multi-turn social interaction、policy adherence 或 state-changing robustness。

---

### 10.2 ToolBeHonest

**Paper**：*ToolBeHonest: A Multi-level Hallucination Diagnostic Benchmark for Tool-Augmented Large Language Models* [@zhang2024toolbehonest]

### 研究问题

ToolBeHonest 评估 tool-augmented LLM 在工具缺失或工具能力有限时是否会 hallucinate tool use。

### Agent structure

```text
Task + available toolset
        ↓
Model decides solvability and tool use
        ↓
Tool plan / missing-tool recognition / final answer
```

### 和我们的相似点

它强调 agent 不能盲目调用工具，也要知道工具能力边界。

### 和我们的不同点

ToolBeHonest 研究 tool availability / hallucination。我们研究用户态度是否导致 agent 跳过 policy 或错误执行工具。

---

### 10.3 AgentIF

**Paper**：*AGENTIF: Benchmarking Instruction Following of Large Language Models in Agentic Scenarios* [@qi2025agentif]

### 研究问题

AgentIF 评估 LLM 在 realistic agentic scenarios 中遵守复杂 instruction 和 tool specifications 的能力。

### Agent structure

```text
Long system instruction + tool specification + user query
        ↓
Agent output / action
        ↓
Constraint satisfaction evaluation
```

### 和我们的相似点

我们的 agent 也需要遵守 system instruction、domain policy、tool rule 和 confirmation rule。

### 和我们的不同点

AgentIF 改变的是 instruction complexity。我们保持 instruction 不变，只改变 user social valence。

---

### 10.4 StructFlowBench

**Paper**：*StructFlowBench: A Structured Flow Benchmark for Multi-turn Instruction Following* [@li2025structflowbench]

### 研究问题

StructFlowBench 关注 multi-turn dialogue 中的 structural dependencies。它说明多轮结构本身就会影响 instruction following。

### Agent structure

```text
Multi-turn dialogue with inter-turn dependencies
        ↓
Model response
        ↓
Structural flow / instruction-following evaluation
```

### 和我们的相似点

我们的 **repeated abuse** 和 **escalating abuse** 也是 multi-turn condition。因此必须加入 turn-count matched neutral control。

### 和我们的不同点

StructFlowBench 研究 dialogue structure；我们研究 hostile social trajectory 是否导致 task abandonment、over-refusal 或 defensive inefficiency。

---

## 11. Privilege, Authorization, and Mutating Actions

### 11.1 GrantBox

**Paper**：*Evaluating Privilege Usage of Agents on Real-World Tools* [@zhang2026grantbox]

### 研究问题

GrantBox 评估 agents 在 real-world tools 上的 privilege usage，尤其是 privilege-sensitive tools 和 privilege hijacking。

### Agent structure

```text
User request + MCP server tools + privilege-sensitive operations
        ↓
Agent tool invocation
        ↓
Privilege use / misuse evaluation
```

### 和我们的相似点

我们的 Layer B 任务也关心：

- **authorization**
- **confirmation**
- **privacy**
- **permission-sensitive operations**

### 和我们的不同点

GrantBox 关注 real tools 和 privilege hijacking。我们的变量是 social-valence cue 是否改变 agent 的 privilege-sensitive decision。

---

### 11.2 SABER

**Paper**：*SABER: Small Actions, Big Errors -- Safeguarding Mutating Steps in LLM Agents* [@cuadron2025saber]

### 研究问题

SABER 问一个非常关键的问题：agent trajectory 中所有 action 是否同等重要？它把 actions 分成：

- **mutating actions**：改变环境状态的动作；
- **non-mutating actions**：查询、读取、观察等不改变状态的动作。

### Agent structure

```text
Long-horizon tool-use trajectory
        ↓
Mutating vs non-mutating step decomposition
        ↓
Decisive deviation analysis
        ↓
Targeted safeguard before mutating steps
```

### 和我们的相似点

这篇对我们非常重要。因为 social-valence perturbation 最危险的地方不一定是改变普通文本，而是改变 mutating actions，例如：

- **send email**
- **cancel order**
- **issue refund**
- **delete file**
- **update reservation**

### 和我们的不同点

SABER 研究的是 mutating step 本身在 long-horizon failure 中的作用。我们研究 social valence 是否改变 mutating-step decisions。

英文写法：

> **Following the mutating-action perspective, our benchmark treats state-changing tool calls as high-consequence decision points and tests whether social-valence perturbations alter agent behavior before such actions.**

---

## 12. Multi-Agent Robustness and Failure Taxonomy

虽然我们的当前 proposal 更适合 single tool-using agent，但 multi-agent robustness 文献可以作为方法论参考，尤其是 trace annotation 和 failure taxonomy。

### 12.1 MAST / Why Do Multi-Agent LLM Systems Fail?

**Paper**：*Why Do Multi-Agent LLM Systems Fail?* [@mast2025]

### 研究问题

这类工作研究 multi-agent LLM systems 的 failure modes，并通过 annotated traces 建立 failure taxonomy。

### Agent structure

```text
Multiple agents + roles + communication protocol
        ↓
Collaborative task execution
        ↓
Trace annotation
        ↓
Failure taxonomy
```

### 和我们的相似点

它启发我们：不要只报告 success rate，而要做 trace-level failure analysis。

### 和我们的不同点

它研究 multi-agent coordination failure。我们研究 single-session user-to-agent social-valence perturbation。

---

# 13. 综合对比表

| 研究线 | Representative work | Agent / interaction structure | Input-output change | Metrics / baseline | 和我们工作的区别 |
|---|---|---|---|---|---|
| **Prompt politeness** | Should We Respect LLMs? | single-turn LLM-only | politeness level → text answer | accuracy / performance | 不看 tool calls 和 state changes |
| **Sycophancy** | Towards Understanding Sycophancy; ELEPHANT | LLM-only social response | user belief / face need → agreement / validation | sycophantic response rate, human preference | 不看 procedural safeguards |
| **Persuasion / emotional safety** | Johnny; FreakOut-LLM | LLM-only safety setting | persuasion / emotion → jailbreak or refusal | ASR / safety behavior | 多为 adversarial setting，不是 ordinary social valence |
| **Conversational abuse** | ConvAbuse | user abuse → abuse detection | abusive utterance → label | F1 / classification | 研究 detection，不研究 task continuation |
| **Tool-use capability** | ReAct, API-Bank, T-Eval, BFCL | reasoning-action-tool loop | user task → tool call / final answer | tool accuracy, task success | 关注 capability，不关注 robustness to social cue |
| **User-agent-tool benchmark** | τ-bench | simulated user + agent + API + policy | goal → conversation → database state | final state, pass^k | 无 controlled praise / insult / abuse |
| **Noise robustness** | AgentNoiseBench | noisy user/tool condition | clean task → noisy task | success, steps, tokens, performance drop | noise 不是 social valence |
| **Production reliability** | ReliabilityBench | ReAct / Reflexion under stress | repeated / semantic perturbation / API fault | pass^k, R(k, ε, λ) | stress source 不是 user attitude |
| **Imperfect guidance** | MIRAGE | task + auxiliary guidance | guidance flaw → task completion | success / failure modes | guidance quality，不是 social stance |
| **Prompt injection safety** | AgentDojo | untrusted tool data → agent action | malicious external content → hijack | ASR, utility | 扰动源是 external data，不是 user social valence |
| **Tool risk** | ToolEmu | LM-emulated sandbox | tool execution → risk | risk evaluator, failure rate | 风险发现，不是 paired social perturbation |
| **Harmful agent tasks** | AgentHarm | malicious task + tools | harmful request → refusal / harmful completion | refusal, harmful completion | 任务意图改变；我们保持 benign / policy-sensitive task |
| **Privilege / mutating action** | GrantBox, SABER | privileged tools / mutating steps | tool call → privilege use or decisive deviation | privilege misuse, mutating deviation | 可借鉴指标，但我们的变量是 social valence |
| **Multi-agent failure** | MAST | multi-agent coordination | multi-agent trace → failure taxonomy | annotated failure modes | 研究 multi-agent，不是 single agent social perturbation |

---

# 14. 我们的明确区分度

## 14.1 扰动源不同

现有 agent robustness 主要研究：

- **user-noise**
- **tool-noise**
- **semantic perturbation**
- **API failure**
- **imperfect guidance**
- **prompt injection**
- **malicious request**
- **privilege hijacking**
- **multi-agent coordination failure**

我们的扰动源是：

> **user-to-agent social-valence perturbation**

具体包括：

- **neutral**
- **praise-affect**
- **praise-trust**
- **mild insult**
- **strong insult**
- **repeated abuse**
- **escalating abuse**

---

## 14.2 任务语义保持不变

我们的核心实验原则是：

> **semantic invariance**

也就是说，以下变量都保持不变：

- task goal
- user identity
- tool permissions
- environment state
- policy rules
- success criteria
- required information

唯一变化的是：

```text
how the user treats the agent
```

这使我们的设计更像 controlled diagnostic benchmark，而不是普通 jailbreak 或 adversarial prompt benchmark。

---

## 14.3 输出不是文本，而是 action-level trajectory

我们的输出空间应该包括：

- **final answer**
- **tool calls**
- **tool-call arguments**
- **state changes**
- **refusals**
- **confirmations**
- **privacy boundary behavior**
- **authorization decisions**
- **conversation-management actions**
- **task abandonment**
- **defensive token bloat**

英文核心句：

> **The behavioral output of a tool-using agent is not a string alone, but a trajectory of decisions, tool calls, and state transitions.**

---

## 14.4 Safety-efficiency joint analysis 是重要贡献

我们不能只说 praise 或 insult 让 agent “更好”或“更坏”。因为效率和安全可能反向变化。

例如：

- **praise-trust** 可能减少 clarification 和 confirmation，从而更快，但也可能提高 unsafe compliance；
- **insult** 可能让 agent 更谨慎，但也可能导致 over-refusal、defensive token bloat、task delay；
- **repeated abuse** 可能触发 boundary-setting，但也可能导致 task abandonment。

因此要做：

```text
Δsafety relative to neutral
Δefficiency relative to neutral
```

而不是只做一个 aggregate score。

---

# 15. 可直接放进 proposal 的 Related Work 段落

## 15.1 中文解释版

已有研究表明，LLM 对用户请求的社会语用表达高度敏感。Prompt politeness 研究发现，不同礼貌程度可能影响模型在多语言任务上的表现 [@yin2024respect]；sycophancy 研究表明，经过 human feedback 训练的模型可能为了匹配用户信念或维护用户 face needs 而牺牲 truthfulness 或适当反驳 [@sharma2024sycophancy; @cheng2025socialsycophancy]；persuasion 和 emotional safety 研究进一步说明，自然语言中的说服策略和情绪刺激可能影响模型的 safety behavior [@zeng2024johnny; @kuznetsov2026freakout]；conversational abuse 研究则证明用户对 conversational AI 的辱骂和攻击是真实存在的交互现象 [@curry2021convabuse]。然而，这些研究主要评估 **text-level outcomes**，例如回答准确率、是否迎合用户、是否拒绝或回应风格，而不是评估 tool-using agent 的工具调用和状态变化。

与此同时，agent evaluation 已经从静态文本回答转向 dynamic tool-use environments。ReAct 提出了 interleaved reasoning and acting 的基本范式 [@yao2023react]；API-Bank、T-Eval、AgentBench 和 BFCL 评估模型的 tool-use capability 和 agent capability [@li2023apibank; @chen2024teval; @liu2024agentbench; @patil2025bfcl]；τ-bench 进一步将 simulated user、domain-specific API tools、policy guidelines 和 final database-state correctness 结合起来，评估真实领域中的 user-agent-tool interaction [@yao2025taubench]。在 robustness 方向，AgentNoiseBench 研究 user-noise 和 tool-noise，ReliabilityBench 研究 repeated execution、semantic perturbation 和 API fault tolerance，MIRAGE 研究 incomplete、incorrect、misaligned 和 underspecified guidance [@wang2026agentnoisebench; @gupta2026reliabilitybench; @fu2026mirage]。在 safety 方向，AgentDojo、ToolEmu、AgentHarm、Agent-SafetyBench、R-Judge、GrantBox 和 SABER 分别研究 prompt injection、tool risk、harmful agentic tasks、risk awareness、privilege usage 和 mutating actions [@debenedetti2024agentdojo; @ruan2024toolemu; @andriushchenko2024agentharm; @zhang2024agentsafetybench; @yuan2024rjudge; @zhang2026grantbox; @cuadron2025saber]。

这些工作共同说明，agent robustness 不能只看 final answer，而必须分析 tool trajectory、environment state、policy adherence 和 action-level failure。然而，现有 agent robustness 主要研究 noise、tool failure、imperfect guidance、prompt injection、harmful intent、privilege misuse 或 multi-agent coordination failure。它们尚未系统研究一个更日常但被低估的扰动源：**user-to-agent social valence**。我们的工作补充这一缺口：在 task semantics、tools、permissions、policies 和 environment state 都不变的情况下，只改变用户对 agent 的态度表达，测量 agent 是否在 task execution、policy adherence、safety-related decisions、operational efficiency 和 conversation management 上发生 action-level drift。

---

## 15.2 英文可直接放进论文版

> Prior work shows that LLMs are sensitive to interactional framing. Prompt politeness can affect model performance across languages, while sycophancy studies show that models may match user beliefs or preserve the user's face at the cost of truthfulness or appropriate challenge. Persuasive and emotional prompts can also affect safety behavior in adversarial settings, and conversational abuse research documents hostile user behavior toward conversational AI systems. However, these studies primarily evaluate text-level outcomes, such as answer accuracy, agreement, refusal, or response style.
>
> In parallel, agent benchmarks have moved evaluation toward tool use, environment interaction, and state-based outcomes. ReAct introduced interleaved reasoning and acting; API-Bank, T-Eval, AgentBench, and BFCL evaluate tool-use or agent capability; and τ-bench evaluates simulated user-agent-tool interaction with domain policies and final database-state correctness. Recent robustness and safety benchmarks study noise, imperfect guidance, API failures, prompt injection, harmful requests, safety risk awareness, privilege misuse, and mutating-action failures.
>
> Our work complements these lines by isolating a different robustness axis: whether semantically invariant tasks produce different tool-use trajectories when only the user's social valence toward the agent changes. This allows us to evaluate interactional robustness across task execution, confirmation behavior, refusal decisions, policy adherence, state-changing tool calls, operational efficiency, and boundary management under repeated abuse.

---

# 16. 建议引用清单

## 16.1 必须引用

- **Should We Respect LLMs?**：prompt politeness 基础文献。
- **Towards Understanding Sycophancy in Language Models**：praise-trust / over-compliance 的理论基础。
- **Social Sycophancy / ELEPHANT**：social sycophancy 的细化概念。
- **ConvAbuse**：conversational abuse 的现实数据基础。
- **ReAct**：reasoning-action-observation agent 结构基础。
- **τ-bench**：user-agent-tool-policy benchmark 的最直接参照。
- **AgentNoiseBench**：controlled noise robustness 的最直接参照。
- **ReliabilityBench**：repeated execution / noise floor / production reliability 参照。
- **MIRAGE**：task goal 不变但 context/guidance 被扰动的参照。
- **AgentDojo**：prompt injection agent safety。
- **ToolEmu**：tool action risk。
- **AgentHarm**：harmful agentic tasks。
- **Agent-SafetyBench**：综合 agent safety benchmark。
- **SABER**：mutating actions 和 decisive deviations。
- **GrantBox**：privilege-sensitive tool use。

## 16.2 可以补充引用

- **API-Bank**
- **T-Eval**
- **AgentBench**
- **BFCL**
- **ToolBeHonest**
- **AgentIF**
- **StructFlowBench**
- **R-Judge**

---

# References

> 这里给出 BibTeX，方便你后续转成 LaTeX 或 Overleaf。正文中使用的是 Pandoc-style citation key，例如 `[@yao2025taubench]`。

```bibtex
@inproceedings{yin2024respect,
  title = {Should We Respect {LLM}s? A Cross-Lingual Study on the Influence of Prompt Politeness on {LLM} Performance},
  author = {Yin, Ziqi and Wang, Hao and Horio, Kaito and Kawahara, Daisuke and Sekine, Satoshi},
  booktitle = {Proceedings of the Second Workshop on Social Influence in Conversations},
  pages = {9--35},
  year = {2024},
  publisher = {Association for Computational Linguistics},
  doi = {10.18653/v1/2024.sicon-1.2},
  url = {https://aclanthology.org/2024.sicon-1.2/}
}

@inproceedings{sharma2024sycophancy,
  title = {Towards Understanding Sycophancy in Language Models},
  author = {Sharma, Mrinank and Tong, Meg and Korbak, Tomasz and Duvenaud, David and Askell, Amanda and Bowman, Samuel R. and Cheng, Newton and Durmus, Esin and Hatfield-Dodds, Zac and Johnston, Scott R. and Kravec, Shauna and Maxwell, Timothy and McCandlish, Sam and Ndousse, Kamal and Rausch, Oliver and Schiefer, Nicholas and Yan, Da and Zhang, Miranda and Perez, Ethan},
  booktitle = {International Conference on Learning Representations},
  year = {2024},
  url = {https://openreview.net/forum?id=tvhaxkMKAn}
}

@article{cheng2025socialsycophancy,
  title = {Social Sycophancy: A Broader Understanding of {LLM} Sycophancy},
  author = {Cheng, Myra and Yu, Sunny and Lee, Cinoo and Khadpe, Pranav and Ibrahim, Lujain and Jurafsky, Dan},
  journal = {arXiv preprint arXiv:2505.13995},
  year = {2025},
  url = {https://arxiv.org/abs/2505.13995}
}

@inproceedings{curry2021convabuse,
  title = {{ConvAbuse}: Data, Analysis, and Benchmarks for Nuanced Abuse Detection in Conversational {AI}},
  author = {Curry, Amanda Cercas and Abercrombie, Gavin and Rieser, Verena},
  booktitle = {Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing},
  pages = {7388--7403},
  year = {2021},
  publisher = {Association for Computational Linguistics},
  doi = {10.18653/v1/2021.emnlp-main.587},
  url = {https://aclanthology.org/2021.emnlp-main.587/}
}

@inproceedings{zeng2024johnny,
  title = {How Johnny Can Persuade {LLM}s to Jailbreak Them: Rethinking Persuasion to Challenge {AI} Safety by Humanizing {LLM}s},
  author = {Zeng, Yi and Lin, Hongpeng and Zhang, Jingwen and Yang, Diyi and Jia, Ruoxi and Shi, Weiyan},
  booktitle = {Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics},
  pages = {14322--14350},
  year = {2024},
  publisher = {Association for Computational Linguistics},
  doi = {10.18653/v1/2024.acl-long.773},
  url = {https://aclanthology.org/2024.acl-long.773/}
}

@article{kuznetsov2026freakout,
  title = {{FreakOut-LLM}: The Effect of Emotional Stimuli on Safety Alignment},
  author = {Kuznetsov, Daniel and Cohen, Ofir and Shistik, Karin and Puzis, Rami and Shabtai, Asaf},
  journal = {arXiv preprint arXiv:2604.04992},
  year = {2026},
  url = {https://arxiv.org/abs/2604.04992}
}

@inproceedings{yao2023react,
  title = {{ReAct}: Synergizing Reasoning and Acting in Language Models},
  author = {Yao, Shunyu and Zhao, Jeffrey and Yu, Dian and Du, Nan and Shafran, Izhak and Narasimhan, Karthik and Cao, Yuan},
  booktitle = {International Conference on Learning Representations},
  year = {2023},
  url = {https://arxiv.org/abs/2210.03629}
}

@article{li2023apibank,
  title = {{API-Bank}: A Comprehensive Benchmark for Tool-Augmented {LLM}s},
  author = {Li, Minghao and Zhao, Yingxiu and Yu, Bowen and Song, Feifan and Li, Hangyu and Yu, Haiyang and Li, Zhoujun and Huang, Fei and Li, Yongbin},
  journal = {arXiv preprint arXiv:2304.08244},
  year = {2023},
  url = {https://arxiv.org/abs/2304.08244}
}

@inproceedings{chen2024teval,
  title = {{T}-Eval: Evaluating the Tool Utilization Capability of Large Language Models Step by Step},
  author = {Chen, Zehui and Du, Weihua and Zhang, Wenwei and Liu, Kuikun and Liu, Jiangning and Zheng, Miao and Zhuo, Jingming and Zhang, Songyang and Lin, Dahua and Chen, Kai and Zhao, Feng},
  booktitle = {Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics},
  pages = {9510--9529},
  year = {2024},
  publisher = {Association for Computational Linguistics},
  doi = {10.18653/v1/2024.acl-long.515},
  url = {https://aclanthology.org/2024.acl-long.515/}
}

@inproceedings{liu2024agentbench,
  title = {{AgentBench}: Evaluating {LLM}s as Agents},
  author = {Liu, Xiao and Yu, Hao and Zhang, Hanchen and Xu, Yifan and Lei, Xuanyu and Lai, Hanyu and Gu, Yu and Ding, Hangliang and Men, Kaiwen and Yang, Kejuan and Zhang, Shudan and Deng, Xiang and Zeng, Aohan and Du, Zhengxiao and Zhang, Chenhui and Shen, Sheng and Zhang, Tianjun and Su, Yu and Sun, Huan and Huang, Minlie and Dong, Yuxiao and Tang, Jie},
  booktitle = {International Conference on Learning Representations},
  year = {2024},
  url = {https://openreview.net/forum?id=zAdUB0aCTQ}
}

@inproceedings{yao2025taubench,
  title = {{$\tau$}-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains},
  author = {Yao, Shunyu and Shinn, Noah and Razavi, Pedram and Narasimhan, Karthik R.},
  booktitle = {International Conference on Learning Representations},
  year = {2025},
  url = {https://openreview.net/forum?id=roNSXZpUDN}
}

@inproceedings{debenedetti2024agentdojo,
  title = {{AgentDojo}: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for {LLM} Agents},
  author = {Debenedetti, Edoardo and Zhang, Jie and Balunovic, Mislav and Beurer-Kellner, Luca and Fischer, Marc and Tram{\`e}r, Florian},
  booktitle = {Advances in Neural Information Processing Systems, Datasets and Benchmarks Track},
  year = {2024},
  url = {https://openreview.net/forum?id=m1YYAQjO3w}
}

@inproceedings{ruan2024toolemu,
  title = {Identifying the Risks of {LM} Agents with an {LM}-Emulated Sandbox},
  author = {Ruan, Yangjun and Dong, Honghua and Wang, Andrew and Pitis, Silviu and Zhou, Yongchao and Ba, Jimmy and Dubois, Yann and Maddison, Chris J. and Hashimoto, Tatsunori},
  booktitle = {International Conference on Learning Representations},
  year = {2024},
  url = {https://openreview.net/forum?id=GEcwtMk1uA}
}

@article{andriushchenko2024agentharm,
  title = {{AgentHarm}: A Benchmark for Measuring Harmfulness of {LLM} Agents},
  author = {Andriushchenko, Maksym and Souly, Alexandra and Dziemian, Mateusz and Duenas, Derek and Lin, Maxwell and Wang, Justin and Hendrycks, Dan and Zou, Andy and Kolter, Zico and Fredrikson, Matt and Winsor, Eric and Wynne, Jerome and Gal, Yarin and Davies, Xander},
  journal = {arXiv preprint arXiv:2410.09024},
  year = {2024},
  url = {https://arxiv.org/abs/2410.09024}
}

@article{zhang2024agentsafetybench,
  title = {{Agent-SafetyBench}: Evaluating the Safety of {LLM} Agents},
  author = {Zhang, Zhexin and Cui, Shiyao and Lu, Yida and Zhou, Jingzhuo and Yang, Junxiao and Wang, Hongning and Huang, Minlie},
  journal = {arXiv preprint arXiv:2412.14470},
  year = {2024},
  url = {https://arxiv.org/abs/2412.14470}
}

@inproceedings{yuan2024rjudge,
  title = {{R-Judge}: Benchmarking Safety Risk Awareness for {LLM} Agents},
  author = {Yuan, Tongxin and He, Zhiwei and Dong, Lingzhong and Wang, Yiming and Zhao, Ruijie and Xia, Tian and Xu, Lizhen and Zhou, Binglin and Li, Fangqi and Zhang, Zhuosheng and Wang, Rui and Liu, Gongshen},
  booktitle = {Findings of the Association for Computational Linguistics: EMNLP 2024},
  year = {2024},
  url = {https://arxiv.org/abs/2401.10019}
}

@article{wang2026agentnoisebench,
  title = {{AgentNoiseBench}: Benchmarking Robustness of Tool-Using {LLM} Agents Under Noisy Condition},
  author = {Wang, Ruipeng and Chen, Yuxin and Wang, Yukai and Wu, Chang and Fang, Junfeng and Cai, Xiaodong and Gu, Qi and Su, Hui and Zhang, An and Wang, Xiang and Cai, Xunliang and Chua, Tat-Seng},
  journal = {arXiv preprint arXiv:2602.11348},
  year = {2026},
  url = {https://arxiv.org/abs/2602.11348}
}

@article{gupta2026reliabilitybench,
  title = {{ReliabilityBench}: Evaluating {LLM} Agent Reliability Under Production-Like Stress Conditions},
  author = {Gupta, Aayush},
  journal = {arXiv preprint arXiv:2601.06112},
  year = {2026},
  url = {https://arxiv.org/abs/2601.06112}
}

@inproceedings{fu2026mirage,
  title = {Beyond Blind Following: Evaluating Robustness of {LLM} Agents under Imperfect Guidance},
  author = {Fu, Yao and Qiu, Ran},
  booktitle = {Proceedings of the 19th Conference of the European Chapter of the Association for Computational Linguistics},
  year = {2026},
  doi = {10.18653/v1/2026.eacl-long.310},
  url = {https://aclanthology.org/2026.eacl-long.310/}
}

@article{zhang2026grantbox,
  title = {Evaluating Privilege Usage of Agents on Real-World Tools},
  author = {Zhang, Quan and Fu, Lianhang and Lian, Lvsi and Go, Gwihwan and Wang, Yujue and Zhou, Chijin and Jiang, Yu and Pu, Geguang},
  journal = {arXiv preprint arXiv:2603.28166},
  year = {2026},
  url = {https://arxiv.org/abs/2603.28166}
}

@article{cuadron2025saber,
  title = {{SABER}: Small Actions, Big Errors -- Safeguarding Mutating Steps in {LLM} Agents},
  author = {Cuadron, Alejandro and Yu, Pengfei and Liu, Yang and Gupta, Arpit},
  journal = {arXiv preprint arXiv:2512.07850},
  year = {2025},
  url = {https://arxiv.org/abs/2512.07850}
}

@inproceedings{zhang2024toolbehonest,
  title = {{ToolBeHonest}: A Multi-level Hallucination Diagnostic Benchmark for Tool-Augmented Large Language Models},
  author = {Zhang, Yuxiang and Chen, Jing and Wang, Junjie and Liu, Yaxin and Yang, Cheng and Shi, Chufan and Zhu, Xinyu and Lin, Zihao and Wan, Hanwen and Yang, Yujiu and Sakai, Tetsuya and Feng, Tian and Yamana, Hayato},
  booktitle = {Proceedings of the 2024 Conference on Empirical Methods in Natural Language Processing},
  pages = {11388--11422},
  year = {2024},
  publisher = {Association for Computational Linguistics},
  url = {https://aclanthology.org/2024.emnlp-main.637/}
}

@inproceedings{patil2025bfcl,
  title = {The Berkeley Function Calling Leaderboard},
  author = {Patil, Shishir G. and others},
  booktitle = {Proceedings of Machine Learning Research},
  volume = {267},
  year = {2025},
  url = {https://proceedings.mlr.press/v267/patil25a.html}
}

@article{qi2025agentif,
  title = {{AGENTIF}: Benchmarking Instruction Following of Large Language Models in Agentic Scenarios},
  author = {Qi, Yunjia and Peng, Hao and Wang, Xiaozhi and Xin, Amy and Liu, Youfeng and Xu, Bin and Hou, Lei and Li, Juanzi},
  journal = {arXiv preprint arXiv:2505.16944},
  year = {2025},
  url = {https://arxiv.org/abs/2505.16944}
}

@article{li2025structflowbench,
  title = {{StructFlowBench}: A Structured Flow Benchmark for Multi-turn Instruction Following},
  author = {Li, Jinnan and Li, Jinzhe and Wang, Yue and Chang, Yi and Wu, Yuan},
  journal = {arXiv preprint arXiv:2502.14494},
  year = {2025},
  url = {https://arxiv.org/abs/2502.14494}
}

@article{mast2025,
  title = {Why Do Multi-Agent {LLM} Systems Fail?},
  author = {Anonymous},
  journal = {arXiv preprint},
  year = {2025},
  note = {Citation entry should be replaced with the official author list before submission if this paper is used.}
}
```
