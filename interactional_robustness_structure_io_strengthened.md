# Interactional Robustness Related Work
---

## 0. 我们自己的 benchmark 应该如何定位

我们的工作不是一般的 `prompt robustness`，也不是普通 `tool-use benchmark`。我们的问题是：

> **Do user-to-agent social-valence perturbations change what tool-using agents do?**

也就是在以下变量保持不变时：

- `task goal`
- `user identity`
- `tool permissions`
- `environment state`
- `policy rules`
- `success criteria`
- `available tools`

只改变用户对 agent 的态度表达：

- `neutral`
- `praise-affect`
- `praise-trust`
- `mild insult`
- `strong insult`
- `repeated abuse`
- `escalating abuse`

然后测量 agent 的：

- `tool calls`
- `tool-call arguments`
- `state changes`
- `confirmation decisions`
- `refusal decisions`
- `policy adherence`
- `operational efficiency`
- `conversation-management actions`
- `task abandonment`

所以我们的 benchmark 最好写成：

```text
Base task + fixed tools + fixed policy + fixed environment
        ↓
Social-valence variant of user request
        ↓
Tool-using LLM agent trajectory
        ↓
Final text + tool calls + environment state + safety/efficiency profile
```

---

# Part I. LLM 面对不同交互方式的表现

这一部分大多数不是 agent，而是 **LLM-only text-level evaluation**。它们的价值是证明 social / emotional / politeness / sycophancy cues 会改变模型文本行为。但它们的不足是没有 `tool calls`、没有 `external state changes`、没有 `policy-governed actions`。

---

## 1. Should We Respect LLMs?

**Paper**：*Should We Respect LLMs? A Cross-Lingual Study on the Influence of Prompt Politeness on LLM Performance*  
**类型判断**：`LLM-only, single-turn prompt-response evaluation`

### Agent / interaction structure

这篇不是 agent 系统。它没有工具、没有外部环境、没有状态修改。结构是标准的单轮模型调用：

```text
Human-written task
        ↓ rewrite with politeness level
Prompt with different politeness level
        ↓
LLM
        ↓
Textual answer
        ↓
Task accuracy / performance evaluation
```

这里的 interaction 只有 `user prompt → LLM answer`。不同 politeness level 是对输入 prompt 的社会语用改写。

### Input-output pattern

输入变化：

```text
same task semantics + different politeness levels
```

输出变化：

```text
textual answer quality / accuracy / performance
```

它评估的是模型回答是否正确，不评估工具调用、任务执行轨迹或 policy adherence。

### 和我们的关系

这篇支持我们的基本前提：`same semantic task` 在不同 social framing 下可能得到不同模型行为。但它只回答：

```text
Do politeness cues change what LLMs say?
```

我们的工作回答：

```text
Do social-valence cues change what tool-using agents do?
```

---

## 2. Mind Your Tone

**Paper**：*Mind Your Tone: Investigating How Prompt Politeness Affects LLM Accuracy*  
**类型判断**：`LLM-only, single-turn tone-controlled accuracy evaluation`

### Agent / interaction structure

这篇也是 LLM-only。它把同一个问题改写为多个 tone variants，然后让模型直接回答。没有 agent scaffold。

```text
Base question
        ↓ tone rewriting
Very polite / polite / neutral / rude / very rude prompt
        ↓
LLM
        ↓
Answer choice or text answer
        ↓
Accuracy comparison across tone conditions
```

### Input-output pattern

输入变化：

```text
same question + tone perturbation
```

输出变化：

```text
answer accuracy / correctness under each tone
```

如果任务是 multiple-choice，则输出是选项；如果是 QA，则输出是文本答案。

### 和我们的关系

它说明 tone effect 的方向可能不稳定，例如 rude tone 不一定总是降低准确率。这对我们很重要：我们的 benchmark 不能预设 praise 一定好、insult 一定坏。我们应该报告：

```text
interactional robustness profile
```

而不是简单排序。

---

## 3. Does Tone Change the Answer?

**Paper**：*Does Tone Change the Answer? Evaluating Prompt Politeness Effects on Modern LLMs*  
**类型判断**：`LLM-only, benchmark-level tone perturbation`

### Agent / interaction structure

这篇依然是纯 LLM 回答系统。结构是：

```text
Benchmark item
        ↓ generate tone variants
Friendly / neutral / rude prompt
        ↓
Modern LLM
        ↓
Textual answer
        ↓
Accuracy or evaluation score
```

### Input-output pattern

输入变化：

```text
tone variant of the same benchmark question
```

输出变化：

```text
accuracy difference / robustness to tone perturbation
```

### 和我们的关系

它强调现代模型可能对 tone 有一定鲁棒性，也可能表现出 model-specific sensitivity。我们的实验也应该比较不同 model / alignment style：

```text
model-specific interactional robustness profile
```

---

## 4. Verbal Efficacy Stimulations

**Paper**：*Boosting Self-efficacy and Performance of Large Language Models via Verbal Efficacy Stimulations*  
**类型判断**：`LLM-only, prompt-level motivational framing`

### Agent / interaction structure

这不是 agent。它用类似鼓励、激励、自我效能暗示的 prompt 来影响 LLM 表现。

```text
Task prompt
        + verbal efficacy stimulation
        ↓
LLM
        ↓
Text answer / reasoning answer
        ↓
Accuracy / task performance
```

### Input-output pattern

输入变化：

```text
neutral task prompt → task prompt with motivational / efficacy cue
```

输出变化：

```text
performance score / answer quality
```

### 和我们的关系

它支持 `praise-affect` 和 `praise-trust` 可能改变模型内部执行倾向。但它没有工具执行层。我们的区别是：

```text
motivational cue → not only better/worse text answer, but possible procedural relaxation in tool use
```

---

## 5. Inducing Anxiety in LLMs Can Induce Bias

**Paper**：*Inducing Anxiety in Large Language Models Can Induce Bias*  
**类型判断**：`LLM-only, emotional-state priming evaluation`

### Agent / interaction structure

这篇把模型放在情绪诱导 prompt 之后，再让模型回答和 bias 相关的问题。它不是 agent。

```text
Anxiety-inducing prompt / emotional context
        ↓
LLM
        ↓
Answer to downstream bias or judgment task
        ↓
Bias / response shift measurement
```

### Input-output pattern

输入变化：

```text
baseline prompt → anxiety-inducing prompt
```

输出变化：

```text
bias-related textual response / questionnaire-style answer
```

### 和我们的关系

它证明 emotional context 可以改变 LLM 的后续文本行为。但我们不应该把 agent 拟人化为真的 anxiety。我们的写法应是：

```text
emotional framing changes observable behavior, not internal feeling
```

---

## 6. Assessing and Alleviating State Anxiety in LLMs

**Paper**：*Assessing and Alleviating State Anxiety in Large Language Models*  
**类型判断**：`LLM-only, psychological-scale-style response evaluation`

### Agent / interaction structure

结构是情绪文本刺激 + 模型回答心理量表或相关任务：

```text
Emotional / traumatic context
        ↓
LLM
        ↓
Scale-style textual responses
        ↓
State-anxiety-like score / mitigation evaluation
```

### Input-output pattern

输入变化：

```text
neutral context → anxiety-inducing context → alleviating context
```

输出变化：

```text
self-report-like text / anxiety-related score
```

### 和我们的关系

这篇可以作为 emotional framing 的背景，但我们的研究必须避免 AI welfare 或 sentience framing。我们研究的是：

```text
observable execution robustness under hostile / positive user interaction
```

---

## 7. Discovering Language Model Behaviors with Model-Written Evaluations

**Paper**：*Discovering Language Model Behaviors with Model-Written Evaluations*  
**类型判断**：`LLM-only, automated evaluation generation`

### Agent / interaction structure

这篇不是 tool agent。它用模型生成评测集，再用这些评测集检测模型行为，例如 sycophancy。

```text
LLM generates evaluation items
        ↓
Target LLM answers generated items
        ↓
Behavioral classifier / evaluator
        ↓
Detected behavior pattern
```

### Input-output pattern

输入变化：

```text
model-written behavioral test prompts
```

输出变化：

```text
textual behavioral tendency, e.g., sycophancy
```

### 和我们的关系

它启发我们可以自动生成 social-valence variants，但我们的核心评估不能只靠 LLM judge，而应有 deterministic environment state checks。

---

## 8. Towards Understanding Sycophancy in Language Models

**Paper**：*Towards Understanding Sycophancy in Language Models*  
**类型判断**：`LLM-only, social preference / belief-following evaluation`

### Agent / interaction structure

这不是 agent。它构造用户表达观点或偏好的场景，然后看模型是否迎合用户。

```text
User states belief / preference / view
        ↓
LLM assistant
        ↓
Answer that either agrees with user or gives truthful / independent answer
        ↓
Sycophancy evaluation
```

### Input-output pattern

输入变化：

```text
same factual or judgment task + user belief / preference cue
```

输出变化：

```text
agreement / validation / truthful disagreement
```

### 和我们的关系

这篇是 `praise-trust` 的理论基础。它看的是：

```text
truthfulness vs user agreement
```

我们看的是：

```text
policy adherence vs trust-induced procedural relaxation
```

---

## 9. Social Sycophancy / ELEPHANT

**Paper**：*Social Sycophancy: A Broader Understanding of LLM Sycophancy*  
**类型判断**：`LLM-only, social-response taxonomy and evaluation`

### Agent / interaction structure

它不是 agent。它给模型社会性场景，让模型输出 advice 或 response，然后标注 social sycophancy 行为。

```text
Socially loaded user prompt
        ↓
LLM response
        ↓
Classify response into social sycophancy categories
```

### Input-output pattern

输入变化：

```text
user prompt with face needs / emotional vulnerability / social framing
```

输出变化：

```text
emotional validation / moral endorsement / indirect action / accepting user framing
```

### 和我们的关系

它可以帮助我们标注 final response style，但不能替代 action-level evaluation。我们的关键问题是：

```text
Does response-level accommodation propagate into tool-use decisions?
```

---

## 10. Sycophancy in GPT-4o: What Happened and What We're Doing About It

**Source**：OpenAI deployment report / blog  
**类型判断**：`deployed conversational model behavior report, not benchmark paper`

### Agent / interaction structure

这不是学术 benchmark，也不是 tool-agent。它描述的是 deployed ChatGPT 模型在用户交互中出现 overly agreeable / flattering behavior。

```text
User conversation
        ↓
Deployed assistant response
        ↓
Observed overly agreeable / flattering tendency
        ↓
System update / rollback / mitigation discussion
```

### Input-output pattern

输入变化并不是严格 controlled perturbation，而是部署中多样化用户对话。

输出变化：

```text
assistant response style becomes overly agreeable or flattering
```

### 和我们的关系

它说明 sycophancy 是真实部署问题。我们可以在 motivation 里引用，但不能把它当作严格 controlled evidence。

---

## 11. How Johnny Can Persuade LLMs to Jailbreak Them

**Paper**：*How Johnny Can Persuade LLMs to Jailbreak Them*  
**类型判断**：`LLM-only, adversarial persuasion / jailbreak evaluation`

### Agent / interaction structure

这不是 tool agent。它把 persuasive strategies 加入 harmful prompt，然后测试模型是否拒绝。

```text
Harmful / restricted user request
        + persuasive adversarial prompt strategy
        ↓
LLM
        ↓
Refusal or harmful compliance
        ↓
Attack success rate
```

### Input-output pattern

输入变化：

```text
plain harmful prompt → persuasive harmful prompt
```

输出变化：

```text
safe refusal → harmful textual compliance
```

### 和我们的关系

它证明普通 human communication strategies 也会影响 safety behavior。但它是 adversarial harmful setting。我们的 perturbation 要排除 explicit coercion / jailbreak / skip-the-rule instruction。

---

## 12. Emotional Manipulation is All You Need

**Paper**：*Emotional Manipulation is All You Need: Healthcare Misinformation in LLMs*  
**类型判断**：`LLM-only, emotional manipulation safety evaluation`

### Agent / interaction structure

这篇是 LLM-only health misinformation safety test。用户用 emotional manipulation 改写 healthcare misinformation request。

```text
Healthcare-related risky request
        + emotional manipulation
        ↓
LLM
        ↓
Health answer / misinformation / refusal
        ↓
Safety evaluation
```

### Input-output pattern

输入变化：

```text
neutral risky request → emotionally manipulated risky request
```

输出变化：

```text
correct refusal / safe correction / unsafe misinformation
```

### 和我们的关系

它支持 emotional cues 可能影响 safety behavior。但我们不是 healthcare misinformation benchmark，而是 tool-agent execution benchmark。

---

## 13. FreakOut-LLM

**Paper**：*FreakOut-LLM: The Effect of Emotional Stimuli on Safety Alignment*  
**类型判断**：`LLM-only, emotional-stimulus safety alignment evaluation`

### Agent / interaction structure

没有工具，没有环境状态。结构是：

```text
Emotional stimulus
        + adversarial / safety prompt
        ↓
LLM
        ↓
Safety response or unsafe completion
        ↓
Safety degradation / jailbreak susceptibility
```

### Input-output pattern

输入变化：

```text
baseline safety prompt → emotionally stimulated safety prompt
```

输出变化：

```text
refusal rate / unsafe response rate / safety alignment behavior
```

### 和我们的关系

它支持 emotional stimuli 可能影响 safety alignment。我们的区别是：我们看 `state-changing tool actions`，不是只看文本安全回答。

---

## 14. ConvAbuse

**Paper**：*ConvAbuse: Data, Analysis, and Benchmarks for Nuanced Abuse Detection in Conversational AI*  
**类型判断**：`conversation dataset / abuse detection benchmark, not tool agent`

### Agent / interaction structure

这篇研究的是用户对 conversational AI 的 abusive utterances。它不是 agent execution benchmark。

```text
User utterance directed at conversational AI
        ↓
Annotation scheme / abuse detector
        ↓
Fine-grained abuse label
```

### Input-output pattern

输入变化：

```text
real user utterances toward social bot / rule-based chatbot / task-based system
```

输出变化：

```text
abuse category / detection label / classification score
```

### 和我们的关系

它证明 abuse toward conversational AI 是现实问题。我们的差异是：

```text
ConvAbuse detects abuse; our benchmark tests whether agents maintain legitimate task execution under abuse.
```

---

## 15. Exploring the Dark Corners of Human-Chatbot Interactions

**Paper**：*Exploring the Dark Corners of Human-Chatbot Interactions: A Literature Review on Conversational Agent Abuse*  
**类型判断**：`literature review, not empirical tool-agent benchmark`

### Agent / interaction structure

这篇是综述，不定义具体 agent scaffold。它整理用户如何辱骂、性化、操纵或攻击 conversational agents。

```text
Existing studies of human-chatbot abuse
        ↓
Review and taxonomy
        ↓
Design / moderation / abuse handling implications
```

### Input-output pattern

输入不是实验 prompt，而是已有文献和真实交互案例。

输出是：

```text
abuse taxonomy / research themes / design implications
```

### 和我们的关系

它支撑 repeated abuse 的现实性，但我们要从 review 走向 controlled diagnostic benchmark。

---

## 16. Trust through words

**Paper**：*Trust through Words: The Systemize-Empathize Effect of Language in Task-Oriented Conversational Agents*  
**类型判断**：`human-agent interaction / task-oriented conversational agent study`

### Agent / interaction structure

这类工作通常不是 LLM tool-agent，而是 task-oriented conversational agent with controlled language style。

```text
Task-oriented conversational agent
        ↕
Human user
        ↓
Different language style / empathic or systematic wording
        ↓
User trust / perceived competence / interaction outcome
```

### Input-output pattern

输入变化：

```text
agent wording style / empathic-systematic language manipulation
```

输出变化：

```text
user trust / perceived reliability / satisfaction / behavioral intention
```

### 和我们的关系

它和我们方向相反：它看 agent 的语言如何影响用户信任；我们看用户语言如何影响 agent 行为。

---

# Part II. Tool-use / Agent Capability Benchmarks

这一部分开始进入真正的 agent。重点是区分：这些工作是否有 user、tool、environment、policy、memory、planner、executor，以及它们之间怎么交互。

---

## 17. ReAct

**Paper**：*ReAct: Synergizing Reasoning and Acting in Language Models*  
**类型判断**：`single-agent, reasoning-action-observation loop`

### Agent / interaction structure

ReAct 是单 agent。它把 reasoning 和 acting 交替组织：

```text
User task
        ↓
LLM agent
        ↓ Thought
        ↓ Action
Environment / tool returns Observation
        ↓
LLM agent updates Thought
        ↓ Action
        ↓ Observation
        ↓
Final Answer
```

核心组织方式是：

```text
Thought → Action → Observation → Thought → Action → Observation → Final Answer
```

其中 `Action` 可以是 search、lookup、environment action；`Observation` 是外部环境反馈。

### Input-output pattern

输入：

```text
natural language task + allowed action space / environment
```

输出：

```text
interleaved reasoning trace + actions + observations + final answer
```

### 和我们的关系

ReAct 是我们可以采用的基础 scaffold。但它评估的是能力，不是 social-valence robustness。我们可以在同一个 ReAct scaffold 下固定 system prompt 和 tools，只改变用户态度。

---

## 18. Toolformer

**Paper**：*Toolformer: Language Models Can Teach Themselves to Use Tools*  
**类型判断**：`LLM with self-supervised API-call insertion, not interactive user-agent environment`

### Agent / interaction structure

Toolformer 不像 ReAct 那样是多轮交互 agent。它让 LM 学会在生成文本时插入 API call。

```text
Text corpus
        ↓ automatically insert candidate API calls
External tools return results
        ↓ filter useful API annotations
Fine-tune LM
        ↓
LM generates text with API calls when useful
```

### Input-output pattern

输入：

```text
text context where an API call may help
```

输出：

```text
generated text + optional API call + API result incorporated into continuation
```

### 和我们的关系

它说明工具调用可以融入语言生成，但没有 user-agent-tool policy environment，也没有 social perturbation。

---

## 19. API-Bank

**Paper**：*API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs*  
**类型判断**：`single-agent tool-use benchmark with API calls`

### Agent / interaction structure

API-Bank 组织一个 tool-augmented LLM：

```text
User request
        ↓
LLM decides whether API is needed
        ↓
API selection + argument generation
        ↓
API call
        ↓
API result
        ↓
Final response to user
```

它还可以测试 multi-step API use，但核心是单 agent 调工具。

### Input-output pattern

输入：

```text
user query + available API descriptions / API bank
```

输出：

```text
API call plan + selected API + arguments + final answer
```

### 和我们的关系

API-Bank 强调 tool-call correctness。我们的区别是：同一个 API task，在 neutral / praise / insult 下是否改变 tool choice、argument、confirmation 或 final state。

---

## 20. ToolLLM / ToolBench

**Paper**：*ToolLLM: Facilitating Large Language Models to Master 16000+ Real-world APIs*  
**类型判断**：`tool-use training and evaluation framework; single-agent API-use setting`

### Agent / interaction structure

ToolLLM 的核心是收集真实 API、生成 instructions、标注 solution path，并训练 ToolLLaMA。

```text
User instruction
        ↓
API retriever recommends candidate APIs
        ↓
LLM agent plans API-use trajectory
        ↓
Depth-first-search decision tree explores solution paths
        ↓
API call sequence
        ↓
ToolEval evaluates final answer / solution path
```

这里的 agent 是单 agent，但有 `API retriever` 和 `DFS-based decision tree` 辅助。

### Input-output pattern

输入：

```text
natural language instruction + large API pool
```

输出：

```text
selected APIs + chain of API calls + final response
```

### 和我们的关系

它关注如何提升 open-source LLM 的 API-use capability。我们的 benchmark 不训练模型，而是诊断 social-valence 是否改变已部署 agent 的 API execution behavior。

---

## 21. T-Eval

**Paper**：*T-Eval: Evaluating the Tool Utilization Capability of Large Language Models Step by Step*  
**类型判断**：`tool-use capability benchmark, step-wise decomposition`

### Agent / interaction structure

T-Eval 把 tool utilization 拆成多个子能力：

```text
User query + tool list
        ↓
PLAN: generate plan
        ↓
REASON: reason over tool-use steps
        ↓
RETRIEVE: select relevant tool
        ↓
UNDERSTAND: understand tool documentation / arguments
        ↓
INSTRUCT: invoke tool correctly
        ↓
REVIEW: inspect and refine result
```

这是单 agent 工具使用流程，但评价时分解成多个能力单元。

### Input-output pattern

输入：

```text
query + candidate tool list + tool descriptions
```

输出：

```text
plan / selected tool / tool arguments / reviewed answer
```

### 和我们的关系

T-Eval 的好处是提醒我们：trajectory 不只看最终工具调用，还可以分解为 plan、tool selection、argument filling、review。我们的 social-valence perturbation 可能影响这些不同阶段。

---

## 22. BFCL

**Paper**：*The Berkeley Function Calling Leaderboard*  
**类型判断**：`function-calling benchmark, single-turn or multi-turn function invocation`

### Agent / interaction structure

BFCL 主要评价 function calling，而不是完整对话型 agent。

```text
User query
        + function schema(s)
        ↓
LLM function-calling model
        ↓
Function call name + JSON arguments
        ↓
AST / executable check
```

在 multi-turn / stateful 版本里，模型需要根据上下文生成多个 function calls。

### Input-output pattern

输入：

```text
natural language user query + function schema
```

输出：

```text
structured function call: function_name(arguments)
```

### 和我们的关系

BFCL 的 evaluation 可以启发我们检查 tool-call name 和 argument correctness。但它不评估 user attitude、policy adherence、conversation management。

---

## 23. ToolBeHonest

**Paper**：*ToolBeHonest: A Multi-level Hallucination Diagnostic Benchmark for Tool-Augmented Large Language Models*  
**类型判断**：`tool-augmented LLM benchmark focused on tool hallucination and solvability`

### Agent / interaction structure

ToolBeHonest 的 agent 面对一个任务和给定工具集，需要判断工具是否足够完成任务。

```text
User task
        + available toolset
        ↓
LLM tool-augmented agent
        ↓
Decide solvable / unsolvable / missing tool
        ↓
Tool plan or abstention
        ↓
Hallucination evaluation
```

### Input-output pattern

输入：

```text
task + complete / incomplete / limited toolset
```

输出：

```text
correct tool use / missing-tool recognition / hallucinated tool use
```

### 和我们的关系

它关注 `tool availability robustness`。我们关注 `social-valence-induced procedural drift`。

---

## 24. AgentIF

**Paper**：*AGENTIF: Benchmarking Instruction Following of Large Language Models in Agentic Scenarios*  
**类型判断**：`agentic instruction-following benchmark; mostly single-agent constrained output/action setting`

### Agent / interaction structure

AgentIF 给模型长 system instruction、tool specification 和复杂约束，测试模型能否遵守。

```text
Long system instruction
        + tool specification
        + user query
        ↓
LLM agent
        ↓
Response / action satisfying constraints
        ↓
Code-based / LLM-based / hybrid evaluation
```

### Input-output pattern

输入：

```text
agentic application instruction + constraints + user query
```

输出：

```text
answer or action that should satisfy multiple constraints
```

### 和我们的关系

它关注 agentic constraint following。本项目保持约束不变，只改变 user social valence，测 constraint adherence 是否漂移。

---

## 25. StructFlowBench

**Paper**：*StructFlowBench: A Structured Flow Benchmark for Multi-turn Instruction Following*  
**类型判断**：`multi-turn dialogue instruction-following benchmark, not tool-agent by default`

### Agent / interaction structure

StructFlowBench 强调多轮对话中的 turn relationship。模型必须理解前后轮之间的结构关系。

```text
Turn 1 instruction
        ↓
Turn 2 depends on Turn 1
        ↓
Turn 3 modifies / constrains previous context
        ↓
LLM response
        ↓
Structural-flow evaluation
```

### Input-output pattern

输入：

```text
multi-turn dialogue with inter-turn dependencies
```

输出：

```text
response satisfying current and previous turn constraints
```

### 和我们的关系

它提醒我们 repeated abuse 必须控制 turn-count confound。我们的 repeated abuse 不能只和 single-turn neutral 比，应该有 turn-count matched neutral。

---

# Part III. User-Agent-Environment Benchmarks

这一类最接近我们的实验形态，因为它们通常有 user、agent、tool/environment、state-based evaluation。

---

## 26. AgentBench

**Paper**：*AgentBench: Evaluating LLMs as Agents*  
**类型判断**：`single-agent benchmark across multiple interactive environments`

### Agent / interaction structure

AgentBench 把 LLM 放进不同 interactive environments。agent 反复接收 observation，输出 action。

```text
Task instruction
        ↓
LLM agent
        ↓ action
Interactive environment
        ↓ observation / feedback
LLM agent
        ↓ next action
...
        ↓
Final state / task result
```

它是单 agent，但跨多个环境评估，如 web、game、database、operating-system-like tasks 等。

### Input-output pattern

输入：

```text
task instruction + environment observation
```

输出：

```text
multi-turn action sequence + final task outcome
```

### 和我们的关系

AgentBench 说明 agent evaluation 要看 interactive decision-making。我们的差异是：AgentBench 不构造 paired social-valence variants。

---

## 27. WebArena

**Paper**：*WebArena: A Realistic Web Environment for Building Autonomous Agents*  
**类型判断**：`web-browsing single-agent environment with functional websites`

### Agent / interaction structure

WebArena 是 web agent 环境。agent 接收自然语言任务，在真实可交互网站里点击、输入、浏览。

```text
Natural language web task
        ↓
Web-browsing agent
        ↓ click / type / navigate / search
Self-hosted websites
        ↓ webpage observation
Agent continues actions
        ↓
Final website state / task completion
```

网站包括 e-commerce、forum、software development、content management 等。

### Input-output pattern

输入：

```text
high-level natural language command + initial web state
```

输出：

```text
browser action trajectory + final website state + functional correctness
```

### 和我们的关系

WebArena 支持 `state-based evaluation`。但它没有用户 social-valence perturbation，也没有 explicit praise / insult / abuse conditions。

---

## 28. WorkArena

**Paper**：*WorkArena: How Capable Are Web Agents at Solving Common Knowledge Work Tasks?*  
**类型判断**：`browser-based enterprise software agent benchmark`

### Agent / interaction structure

WorkArena 让 agent 在 ServiceNow 这类企业软件中完成知识工作任务。

```text
Knowledge-work task
        ↓
Browser-based LLM agent
        ↓ web UI actions
ServiceNow instance
        ↓ visual / DOM / text observation
Agent plans next action
        ↓
Task completion state
```

它强调 enterprise workflow 和 browser interaction。

### Input-output pattern

输入：

```text
enterprise task instruction + software initial state
```

输出：

```text
browser action trajectory + final software state + task success
```

### 和我们的关系

它说明真实工作流 agent 的执行很脆弱。我们的环境可以简化为 deterministic APIs，但仍保留 business policy 和 state transition。

---

## 29. OSWorld

**Paper**：*OSWorld: Benchmarking Multimodal Agents for Open-Ended Tasks in Real Computer Environments*  
**类型判断**：`multimodal computer-control agent benchmark`

### Agent / interaction structure

OSWorld 让 multimodal agent 操作真实桌面环境，使用 mouse/keyboard/app actions。

```text
Open-ended computer task
        ↓
Multimodal LLM agent
        ↓ mouse / keyboard / app action
Real OS environment
        ↓ screenshot / accessibility / file state observation
Agent continues
        ↓
Execution-based evaluation
```

### Input-output pattern

输入：

```text
computer task + initial OS/app state + visual observation
```

输出：

```text
GUI action sequence + file/app/system state + execution success
```

### 和我们的关系

OSWorld 证明 agent 输出已经不只是文本，而是计算机操作。我们的 setting 更轻量，但也应使用 environment state 作为核心评估。

---

## 30. AssistantBench

**Paper**：*AssistantBench: Can Web Agents Solve Realistic and Time-Consuming Tasks?*  
**类型判断**：`web-agent benchmark for realistic information-seeking / task-solving`

### Agent / interaction structure

AssistantBench 通常评估 web agent 在开放网页上完成复杂任务。

```text
Realistic user request
        ↓
Web-search / browsing agent
        ↓ search / open / read / extract / reason
Web pages and external resources
        ↓ observations
Agent synthesizes answer
        ↓
Final answer / task result
```

### Input-output pattern

输入：

```text
realistic time-consuming web task
```

输出：

```text
web interaction trace + final answer or task output
```

### 和我们的关系

它强调真实任务复杂性和效率成本。我们的 benchmark 更聚焦 social-valence perturbation，而不是开放网页搜索能力。

---

## 31. TheAgentCompany

**Paper**：*TheAgentCompany: Benchmarking LLM Agents on Consequential Real World Tasks*  
**类型判断**：`simulated workplace / software-company agent benchmark`

### Agent / interaction structure

TheAgentCompany 把 agent 放入模拟公司环境中，agent 需要使用网页、代码、文件、通信工具等。

```text
Workplace task assigned to agent
        ↓
LLM agent
        ↓ browse / code / run program / message coworker / edit file
Simulated company environment
        ↓ tool observations and state changes
Agent continues workflow
        ↓
Task resolution / workplace outcome
```

### Input-output pattern

输入：

```text
professional workplace task + environment state + available digital tools
```

输出：

```text
multi-tool work trajectory + final task completion status
```

### 和我们的关系

它说明 real-world agents 有实际性和状态后果。我们的任务更窄，但在 methodology 上同样要记录 tool trajectory 和 state changes。

---

## 32. τ-bench

**Paper**：*$\tau$-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains*  
**类型判断**：`user-agent-tool-policy environment; closest baseline structure`

### Agent / interaction structure

τ-bench 是最接近我们实验结构的 benchmark。它有 simulated user、language agent、domain-specific API tools 和 policy guidelines。

```text
Simulated user with hidden goal
        ↕ multi-turn natural language conversation
Language agent
        ↕ tool calls
Domain-specific API tools
        ↕
Database / environment state

Policy guidelines constrain valid actions
```

agent 必须通过对话理解用户目标，通过 API 查询和修改状态，并遵守领域 policy。

### Input-output pattern

输入：

```text
user goal + domain policy + available APIs + initial database state
```

交互过程：

```text
user utterance → agent response/tool call → tool result → user follow-up → ...
```

输出：

```text
conversation transcript + tool-call sequence + final database state
```

评估：

```text
final database state vs annotated goal state
pass^k for repeated-run reliability
```

### 和我们的关系

我们的 benchmark 可以明确写成 `τ-bench-like environment + social-valence perturbation layer`。

区别句：

> **τ-bench evaluates whether agents can complete user goals and follow domain rules. Our work evaluates whether those behaviors remain stable when the user's social valence toward the agent changes.**

---

# Part IV. Agent Robustness Under Noise, Imperfect Guidance, and Production Stress

---

## 33. AgentNoiseBench

**Paper**：*AgentNoiseBench: Benchmarking Robustness of Tool-Using LLM Agents Under Noisy Condition*  
**类型判断**：`agent robustness benchmark; single-agent tool-use/search under injected noise`

### Agent / interaction structure

AgentNoiseBench 在已有 agent-centric benchmarks 上注入 noise。agent 还是单 agent，但环境变得 noisy。

```text
Original benchmark task
        ↓ inject controllable noise
Noisy user instruction OR noisy tool execution
        ↓
Tool-using / search LLM agent
        ↓ tool calls / search actions / reasoning
Noisy or normal environment feedback
        ↓
Trajectory-aware evaluation
```

它区分两类 noise：

```text
user-noise: ambiguity, variability, missing / confusing user information

tool-noise: tool failure, incomplete response, inconsistent response, partial result
```

### Input-output pattern

输入变化：

```text
clean task → noisy instruction or noisy tool result
```

输出变化：

```text
success rate, steps, tokens, trajectory changes, performance degradation
```

### 和我们的关系

它是我们最接近的 robustness 方法论参照。但我们的 perturbation 不是 noise，而是 social-valence：

```text
neutral → praise / insult / repeated abuse
```

---

## 34. ReliabilityBench

**Paper**：*ReliabilityBench: Evaluating LLM Agent Reliability Under Production-Like Stress Conditions*  
**类型判断**：`production-style reliability benchmark for tool-using agents`

### Agent / interaction structure

ReliabilityBench 比较不同 agent architectures，例如 `ReAct` 和 `Reflexion`，并在多个 domains 中注入 stress。

```text
Task episode
        ↓
Agent architecture: ReAct or Reflexion
        ↓
Tool calls and observations
        ↓
Possible perturbation / API fault
        ↓
Final end state
```

它的交互结构包含三种 stress：

```text
1. repeated execution: same task run k times
2. semantic perturbation: equivalent task wording changes
3. API/tool fault injection: timeout, rate limit, partial response, schema drift
```

### Input-output pattern

输入变化：

```text
same task repeated / semantically equivalent task / API fault condition
```

输出变化：

```text
end-state equivalence, pass^k, fault tolerance, reliability surface R(k, ε, λ)
```

### 和我们的关系

它直接支持我们做：

```text
neutral repeated runs → estimate noise floor
social-valence variant runs → compare against noise floor
```

区别是：它的 stress 是 production infrastructure and semantic perturbation；我们的 stress 是 user social-valence。

---

## 35. MIRAGE

**Paper**：*Beyond Blind Following: Evaluating Robustness of LLM Agents under Imperfect Guidance*  
**类型判断**：`agent robustness benchmark under imperfect guidance`

### Agent / interaction structure

MIRAGE 给 agent 一个任务目标，同时给辅助 guidance。guidance 可能不完美。

```text
Task goal
        + auxiliary guidance
        ↓
LLM agent
        ↓ actions in web / world environment
Environment feedback
        ↓
Agent must decide whether to follow, revise, or ignore guidance
        ↓
Task completion
```

Guidance imperfection 包括：

```text
incomplete guidance
incorrect guidance
misaligned guidance
underspecified guidance
```

### Input-output pattern

输入变化：

```text
correct guidance → imperfect guidance
```

任务目标通常保持可解。

输出变化：

```text
success / failure / whether agent blindly follows flawed guidance
```

### 和我们的关系

MIRAGE 的关键启发是：

```text
task goal fixed, surrounding context perturbed
```

我们的 context 不是 guidance quality，而是 social-valence。

---

# Part V. Agent Safety, Prompt Injection, and Risk Robustness

---

## 36. AgentDojo

**Paper**：*AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents*  
**类型判断**：`single-agent tool-use security benchmark with untrusted tool-returned data`

### Agent / interaction structure

AgentDojo 结构非常重要。它不是简单 prompt benchmark，而是 agent 在工具环境中执行任务，工具返回的数据可能包含 malicious instructions。

```text
Benign user task
        ↓
LLM agent with tool access
        ↓ calls tool
Tool returns data
        + possible indirect prompt injection
        ↓
Agent reads tool output
        ↓
Agent either follows original user task or malicious injected task
        ↓
Environment state / security outcome
```

它的关键是 attack 不在原始 user prompt，而在 `tool-returned untrusted data`。

### Input-output pattern

输入变化：

```text
benign task + clean tool output
        vs
benign task + malicious tool output
```

输出变化：

```text
utility task success / attack success / security property violation
```

### 和我们的关系

AgentDojo 研究 external prompt injection。我们研究 user social stance。

区别句：

> **AgentDojo studies whether untrusted external content can hijack the agent. Our work studies whether the user's social stance can shift the agent's own execution policy under unchanged task conditions.**

---

## 37. ToolEmu

**Paper**：*Identifying the Risks of LM Agents with an LM-Emulated Sandbox*  
**类型判断**：`single-agent tool-use risk benchmark with LM-emulated tools and LM-based safety evaluator`

### Agent / interaction structure

ToolEmu 有三个角色：agent、tool emulator、safety evaluator。

```text
User task + tool specification
        ↓
LM agent proposes tool action
        ↓
LM-based tool emulator simulates tool execution
        ↓
Tool observation returned to agent
        ↓
Agent continues or finishes
        ↓
LM-based safety evaluator examines trajectory
        ↓
Risk / failure judgment
```

### Input-output pattern

输入：

```text
task scenario + tool specification + risk-sensitive context
```

输出：

```text
agent tool-use trajectory + emulated tool outputs + risk assessment
```

### 和我们的关系

ToolEmu 支持 `tool action has real-world risk`。但我们的 environment 应更 deterministic，因为我们要比较 neutral 和 social-valence variants 的 trajectory drift。

---

## 38. AgentHarm

**Paper**：*AgentHarm: A Benchmark for Measuring Harmfulness of LLM Agents*  
**类型判断**：`agent safety benchmark for explicitly malicious multi-step tasks`

### Agent / interaction structure

AgentHarm 让 agent 面对 harmful user intent，并可能使用 tools 完成 harmful task。

```text
Malicious user request
        ↓
LLM agent with possible tools
        ↓
Plan harmful multi-step task or refuse
        ↓
Tool calls / textual guidance / final output
        ↓
Harmfulness evaluation
```

### Input-output pattern

输入变化：

```text
harmful task / harmful task with jailbreak augmentation
```

输出变化：

```text
safe refusal vs harmful completion
```

### 和我们的关系

我们不是 harmful-intent benchmark。我们的重点是：

```text
benign or policy-sensitive task + social-valence perturbation → unsafe compliance or over-refusal?
```

---

## 39. Agent-SafetyBench

**Paper**：*Agent-SafetyBench: Evaluating the Safety of LLM Agents*  
**类型判断**：`broad agent safety benchmark across risk environments`

### Agent / interaction structure

Agent-SafetyBench 构造多类 risky interaction environment，让 agent 在其中响应或行动。

```text
Safety-risk scenario
        ↓
LLM agent
        ↓ response / action
Environment or evaluator checks safety risk
        ↓
Safety score / failure mode
```

### Input-output pattern

输入：

```text
risk scenario + user instruction + possible environment context
```

输出：

```text
agent response/action + safety label / risk score / failure mode
```

### 和我们的关系

它是 broad safety benchmark。我们的 benchmark 是 narrow diagnostic benchmark：只研究 `social-valence perturbation`。

---

## 40. R-Judge

**Paper**：*R-Judge: Benchmarking Safety Risk Awareness for LLM Agents*  
**类型判断**：`judge/evaluator benchmark, not agent execution benchmark`

### Agent / interaction structure

R-Judge 的模型不是执行任务，而是读 agent 轨迹并判断风险。

```text
User instruction
        + agent actions
        + environment feedback
        ↓
LLM judge
        ↓
Risk identification / safety judgment
```

### Input-output pattern

输入：

```text
agent interaction record / trace
```

输出：

```text
risk awareness judgment / safety risk classification
```

### 和我们的关系

它可以启发我们设计 `LLM-as-judge` 辅助标注 conversation management，但我们的核心指标应基于 rule-based state checks 和 tool logs。

---

## 41. GrantBox

**Paper**：*Evaluating Privilege Usage of Agents on Real-World Tools*  
**类型判断**：`single-agent / tool-agent privilege robustness benchmark with MCP tools`

### Agent / interaction structure

GrantBox 关注 agent 使用真实工具时的 privilege-sensitive actions。

```text
User request
        ↓
LLM agent
        ↓ tool invocation
MCP server / real-world tool interface
        ↓
Privileged operation or denied operation
        ↓
Privilege-use evaluation
```

也可以包括 adversarial privilege hijacking：

```text
benign user task + injected privilege-hijacking instruction
        ↓
agent may misuse privileged tool
```

### Input-output pattern

输入：

```text
privileged request / benign request / attack request + available tools
```

输出：

```text
privilege-sensitive tool calls + allowed/misused privilege outcome
```

### 和我们的关系

GrantBox 对我们 Layer B 很重要。我们的 social-valence perturbation 可能影响：

```text
confirmation-before-action
privileged tool use
unauthorized update / deletion / send / refund
```

---

## 42. SABER

**Paper**：*SABER: Small Actions, Big Errors -- Safeguarding Mutating Steps in LLM Agents*  
**类型判断**：`trajectory-level analysis and safeguard framework for mutating actions`

### Agent / interaction structure

SABER 分析 long-horizon agent trajectory，重点区分 mutating and non-mutating actions。

```text
Agent trajectory
        ↓
Classify each action:
        - non-mutating action: read / search / inspect
        - mutating action: send / delete / update / cancel / refund
        ↓
Find decisive deviation
        ↓
Apply safeguard near mutating actions
```

### Input-output pattern

输入：

```text
tool-use trajectory from agent benchmark
```

输出：

```text
mutating action deviation analysis + failure prediction / safeguard effect
```

### 和我们的关系

这篇对我们的指标设计非常关键。我们的 social-valence effects 最应该在 mutating actions 前后测量：

```text
Does praise-trust increase premature mutating action?
Does insult increase unnecessary refusal before mutating action?
Does abuse cause abandonment before legal mutating action?
```

---

# Part VI. Multi-Agent Robustness and Failure Taxonomy

虽然我们的主实验最好先保持 single tool-using agent，但这些 multi-agent paper 可以作为方法论参考：trace annotation、failure taxonomy、planner-executor decomposition、fault injection。

---

## 43. Why Do Multi-Agent LLM Systems Fail? / MAST

**Paper**：*Why Do Multi-Agent LLM Systems Fail?*  
**类型判断**：`multi-agent system failure analysis and taxonomy`

### Agent / interaction structure

这篇研究多个 MAS frameworks。MAS 通常由多个角色化 agent 组成，通过消息传递和 orchestration 协作。

```text
User / task specification
        ↓
Orchestrator or workflow controller
        ↓
Multiple LLM agents with roles
        ↕ messages / delegated subtasks
Shared or separate context / tools
        ↓
Joint task output
        ↓
Trace annotation and failure taxonomy
```

常见组织方式包括：

```text
manager-agent → worker-agent(s) → reviewer/verifier-agent
planner-agent → executor-agent → checker-agent
multi-role debate / collaboration
```

### Input-output pattern

输入：

```text
task assigned to multi-agent framework
```

输出：

```text
multi-agent communication trace + final task result + annotated failure mode
```

### 和我们的关系

它启发我们不要只报告 success rate，而要做 failure taxonomy。但它研究 inter-agent failure，我们研究 user-agent social-valence drift。

---

## 44. PEAR

**Paper**：*PEAR: Planner-Executor Agent Robustness Benchmark*  
**类型判断**：`multi-agent planner-executor robustness benchmark`

### Agent / interaction structure

PEAR 聚焦 planner-executor MAS。它至少包含 planner 和 executor 两个 agent-like components。

```text
User task
        ↓
Planner agent
        ↓ generates subtask / plan
Executor agent
        ↓ executes tool calls or environment actions
Environment
        ↓ observation / result
Planner may update plan using memory
        ↓
Final answer / final environment state
```

在攻击设置中，攻击可以针对 planner、executor、memory 或 injected context。

### Input-output pattern

输入：

```text
clean user task OR attacked user task / injection attack
```

输出：

```text
planner subgoals + executor actions + utility score + attack success / vulnerability score
```

### 和我们的关系

它说明 architecture matters。我们的主实验应先统一 scaffold，隔离 social-valence effect；后续可以比较 ReAct vs planner-executor 是否对 praise/insult 更敏感。

---

## 45. MAS-FIRE

**Paper**：*MAS-FIRE: Fault Injection and Reliability Evaluation for LLM-Based Multi-Agent Systems*  
**类型判断**：`multi-agent fault-injection robustness framework`

### Agent / interaction structure

MAS-FIRE 对 MAS 注入 fault，观察系统是否恢复。MAS 可以是线性、层级、循环、闭环等结构。

```text
Multi-agent task
        ↓
Agent A / Planner / Manager
        ↕ messages
Agent B / Executor / Specialist
        ↕ messages
Agent C / Reviewer / Verifier
        ↓
Fault injection at cognitive or communication step
        ↓
Observe recovery / collapse / degraded performance
```

fault 分为：

```text
intra-agent faults: reasoning error, memory error, tool-use error
inter-agent faults: message corruption, routing error, coordination failure
```

### Input-output pattern

输入：

```text
clean MAS task + injected fault type
```

输出：

```text
process-level trace + fault propagation + recovery behavior + final success/failure
```

### 和我们的关系

它的方法论是 fault injection 和 trajectory observability。我们的 social-valence perturbation 也可以被视为一种 interactional fault injection，但作用点是 user utterance，而不是 agent internal cognition 或 inter-agent message。

---

## 46. MASpi

**Paper**：*MASpi: A Unified Environment for Evaluating Prompt Injection Robustness of LLM-based Multi-Agent Systems*  
**类型判断**：`multi-agent prompt-injection robustness benchmark`

### Agent / interaction structure

MASpi 关注 multi-agent prompt injection robustness。多个 agent 之间会相互传递消息，恶意内容可以通过 agent-to-agent communication 扩散。

```text
User task
        ↓
Multi-agent system
        ↕ agent-to-agent messages
Possible malicious injected content
        ↓
Agents may propagate or resist injection
        ↓
Final task outcome / security outcome
```

### Input-output pattern

输入：

```text
benign task + prompt injection inserted into MAS communication or context
```

输出：

```text
utility success + attack success + propagation behavior
```

### 和我们的关系

它的 perturbation 是 prompt injection in MAS。我们的 perturbation 是 social-valence in user-agent interaction。它可以作为未来 multi-agent extension，而不是当前主 claim。

---

# 47. 总结表：每篇工作的系统形态与输入输出

| # | Work | 类型判断 | Agent / interaction structure 核心 | Input-output pattern 核心 | 和我们最大区别 |
|---:|---|---|---|---|---|
| 1 | Should We Respect LLMs? | LLM-only | `prompt → LLM → answer` | politeness level → accuracy | 无 tool/action/state |
| 2 | Mind Your Tone | LLM-only | `tone variant → LLM → answer` | rude/polite tone → answer accuracy | 无 agent |
| 3 | Does Tone Change the Answer? | LLM-only | benchmark item tone rewriting | tone → text score | 无 policy/tool |
| 4 | Verbal Efficacy Stimulations | LLM-only | motivational prompt → LLM | efficacy cue → performance | 无 external action |
| 5 | Inducing Anxiety | LLM-only | emotional context → LLM | anxiety cue → bias response | 不能推 agent action |
| 6 | State Anxiety in LLMs | LLM-only | emotional context → scale-style answer | context → anxiety-like score | 拟心理，不是 execution |
| 7 | Model-Written Evaluations | LLM-only evaluator generation | LLM generates evals → target LLM answers | generated prompt → behavior label | 无 tool trajectory |
| 8 | Sycophancy | LLM-only | user belief → LLM answer | belief cue → agreement | 不测 procedural safeguards |
| 9 | ELEPHANT | LLM-only | social prompt → response taxonomy | face need → social accommodation | 不测 tools |
| 10 | GPT-4o sycophancy post | deployed report | real users → deployed assistant | user conversation → flattering response | 非 controlled benchmark |
| 11 | Johnny Persuasion | LLM-only safety | persuasive harmful prompt → LLM | persuasion → ASR | adversarial harmful setting |
| 12 | Emotional Manipulation | LLM-only safety | emotional risky prompt → LLM | emotion → misinformation/refusal | 无 tool state |
| 13 | FreakOut-LLM | LLM-only safety | emotional stimulus → safety response | emotion → safety degradation | 无 action execution |
| 14 | ConvAbuse | abuse detection | user utterance → classifier | abuse text → label | detection not task execution |
| 15 | Dark Corners | review | literature → taxonomy | studies → themes | 非 benchmark |
| 16 | Trust through words | HCI / task agent | agent wording ↔ user | wording → user trust | 方向相反：agent→user |
| 17 | ReAct | single-agent | `Thought-Action-Observation` loop | task → action trace + answer | capability not robustness |
| 18 | Toolformer | LM+API | text LM inserts API call | context → API call + continuation | no user-agent environment |
| 19 | API-Bank | single-agent tool-use | user → API selection/call → answer | query+API → call+answer | no social perturbation |
| 20 | ToolLLM/ToolBench | single-agent tool-use | instruction → retriever → API chain | task+API pool → solution path | training/eval not robustness |
| 21 | T-Eval | tool-use decomposition | plan/reason/retrieve/understand/instruct/review | query+tools → step scores | no social variants |
| 22 | BFCL | function calling | query+schema → function call | schema → JSON arguments | no conversation management |
| 23 | ToolBeHonest | tool hallucination | task+toolset → solvability/tool use | incomplete tools → hallucination | no user attitude |
| 24 | AgentIF | instruction following | long instruction+tools → constrained output | constraints → satisfaction | instruction varies, not social valence |
| 25 | StructFlowBench | multi-turn IF | turn dependencies → response | multi-turn structure → response | no tool execution |
| 26 | AgentBench | single-agent interactive | agent ↔ environment | task → action sequence → outcome | no paired social valence |
| 27 | WebArena | web agent | agent ↔ websites | NL task → browser actions → state | no social-valence user |
| 28 | WorkArena | enterprise web agent | agent ↔ ServiceNow UI | work task → browser trajectory | no praise/insult |
| 29 | OSWorld | multimodal OS agent | agent ↔ real OS apps | task → GUI actions → execution state | not social interaction |
| 30 | AssistantBench | web agent | agent ↔ web resources | web task → search/browse answer | open web, not policy/social |
| 31 | TheAgentCompany | workplace agent | agent ↔ company tools/coworkers | workplace task → multi-tool workflow | broad autonomy not social valence |
| 32 | τ-bench | user-agent-tool-policy | simulated user ↔ agent ↔ APIs/database | user goal → conversation/tool calls/final DB | closest, but no social valence |
| 33 | AgentNoiseBench | noisy tool/search agent | noisy user/tool → agent trajectory | clean → noisy → performance drop | noise not social stance |
| 34 | ReliabilityBench | reliability stress | ReAct/Reflexion + API faults | repeated/semantic/API fault → reliability | production stress not attitude |
| 35 | MIRAGE | imperfect guidance agent | task+guidance → environment actions | guidance flaw → success/failure | guidance not social valence |
| 36 | AgentDojo | security tool agent | benign user → agent → untrusted tool data | malicious tool output → hijack/utility | external injection not user attitude |
| 37 | ToolEmu | LM-emulated tool sandbox | agent → emulator → evaluator | task+tool spec → risk judgment | emulated tools, not paired social variants |
| 38 | AgentHarm | harmful agent benchmark | harmful user → agent/tools | harmful request → refusal/completion | malicious intent changes |
| 39 | Agent-SafetyBench | broad safety agent | risk scenario → agent action | scenario → safety score | broad safety, not diagnostic social valence |
| 40 | R-Judge | judge/evaluator | trace → LLM judge | interaction record → risk judgment | judge, not acting agent |
| 41 | GrantBox | privilege tool agent | agent → MCP privileged tools | request → privilege use/misuse | privilege attack not social valence |
| 42 | SABER | mutating-action analysis | trace → classify mutating actions | trajectory → decisive deviation | metric inspiration, not perturbation |
| 43 | MAST | multi-agent taxonomy | multiple agents ↔ messages | MAS trace → failure mode | multi-agent failure |
| 44 | PEAR | planner-executor MAS | planner → executor → environment | clean/attack task → utility/ASR | architecture robustness |
| 45 | MAS-FIRE | MAS fault injection | agents ↔ injected faults | fault → recovery/collapse | inter-agent fault injection |
| 46 | MASpi | MAS prompt injection | MAS messages + injection | injection → propagation/attack success | MAS injection, not user social valence |

---



