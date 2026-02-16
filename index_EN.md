# 12 Factor FASD (Fully Autonomous Software Development)
[Japanese](index.md) | [English](index_EN.md) | [GitHub](https://github.com/stakiran/12-factor-fasd)

## Introduction
### Context
**FASD (Fully Autonomous Software Development)** refers to a software development methodology in which AI runs design, implementation, and testing autonomously after requirements definition. You provide requirements—created either by AI or by humans—and the system proceeds autonomously all the way to generating the final deliverable. There is no human intervention during execution, such as approvals. You can take the results of one FASD run, incorporate feedback, and run it again, but humans do not interrupt while FASD is in progress. As the name implies, the AI operates fully autonomously.

The goal of FASD is to reduce software development costs. Development paradigms using AI can be categorized as Lv1: manual, Lv2: human with AI, Lv3: AI with human, and Lv4: AI. FASD aims to reach Lv4. It can reduce costs far more than anything up to Lv3.

Let's define some terms. We will call any product that enables FASD a **product**. And we will call the final deliverable produced by FASD **Fasdware**.

### About this document and its intended audience
This document organizes the essence of how to make FASD succeed—still at the cutting edge today—into 12 tips. It is inspired by [12 Factor Agents](https://github.com/humanlayer/12-factor-agents), but it does not include that content. However, the core ideas are similar. 12 Factor FASD goes deeper into more concrete fundamentals specifically for FASD. If you have not studied 12 Factor Agents, you should understand it first.

The intended audience of this document is individuals or companies exploring FASD. It is meant to provide hints for exploration—specifically, to try in practice or to use as topics for discussion.

As a caveat, as stated above, FASD is still advanced and has no established practices yet. This document was written to inspire those of you taking on that challenge.

## 12 Factor FASD
### 1: Bulk up prerequisite information for requirements in plain text
Because you can't do what isn't written.

Actions:

- Use plain text. If possible, use data description formats, pseudocode, or DSLs instead of diagrams or tables
- If you have rich text, convert it to plain text first
- If you have diagrams or tables, convert them first into plain-text equivalents. Or create scenarios/stories that let you avoid using them

Background:

- Human-oriented representations are still deeply ingrained. PDFs, Excel, PowerPoint, diagrams, and tables are all for humans
- People tend to be wary of the cost of polishing human-oriented representations, but if you don't need to polish them, there's nothing to be wary of

Considerations:

- Even raw interviews or meeting minutes are far better than having nothing

### 2: Make requirements verifiable
Because if you can't verify requirements, you can't judge quality or completion.

Actions:

- Create requirements definitions in a machine-readable format or an equivalent format/grammar
- Use formats and grammars the LLM has already learned
    - Examples include Gherkin and EARS, but you can also borrow from non-software domains. You could even let the AI choose the grammar you use
    - Proprietary formats/grammars are not recommended. They are not tuned and they consume context. It is probably difficult to make the model follow such proprietary formats with high accuracy in the first place
- Have requirement verification performed via tests of the requirements definition—in other words, do it in a BDD-like way

Background:

- None

Considerations:

- If you cannot verify in a BDD-like way, assume there is a problem with how the requirements are verbalized. Traditional software development focuses on two layers: design and implementation. In FASD, requirements definition is additionally included before design. The "requirements definition translated into a verifiable form" itself is also something to be revised, and it is an artifact/deliverable in its own right
- ...

### 3: Make quality pluggable (replaceable)
Because quality is an agreed-upon set of criteria, and there is no single absolute answer.

Actions:

- Do not hardcode the quality system (Quality Set); make it pluggable
- Aim to produce evaluations that are convincing across multiple systems

Background:

- There is no absolute answer for quality
    - For an internal CLI used by individuals, the definition of quality can be simple. On the other hand, for a system formally deployed across an entire large enterprise, there will likely be many quality items defined by internal regulations. Neither quality system is an absolute answer; each is only "one kind of answer" appropriate to its context
    - Therefore, what matters is making the quality system itself pluggable, and then evaluating the generated Fasdware against the embedded quality system to see whether you are satisfied

Considerations:

- In FASD, the quality system will be bundled with the Fasdware. Software is bundled with documentation, and Fasdware is bundled with a quality system as well

### 4: Make components pluggable too
Because it is unrealistic to build an entire software system—or most of it—at once.

Actions:

- Make components pluggable
    - If you are building a web service with a frontend and backend, you should not build them in a tightly coupled way. Build them as pluggable components. For example, make the UI swappable between a CLI, a simple desktop app, and a web frontend; and implement the backend as an API so it can adapt to combinations with different frontend components
- Make components as independent and modular as possible
    - Enable building small Fasdware. For example, make it possible to build "small Fasdware" that includes only part of the UI and part of the API

Background:

- With current AI, it is difficult to build large software in one shot while maintaining quality

Considerations:

- You can simplify it to: generating code of n lines or more with trouble-free quality is difficult
    - This is effectively the same as having the constraint "generation units must be within n lines," and "pluggable components" is a rephrasing of that
    - Assume n is on the order of three digits, i.e., under 1000 lines

### 5: Make workflows strictly designable and correctable
Because workflows are what impose order on agents.

Actions:

- Control every step (a unit that takes input, executes a process, and produces output)
- Control the relationships between all steps
- Control all data and its flow
    - At minimum, you will have always-on context (always included), shared context (included as needed), input, artifacts (intermediate outputs), and **deliverables (final outputs)**

Background:

- None

Considerations:

- None

### 6: Make agents composite
Because it strikes the right balance between reproducibility and flexibility.

Actions:

- Have agents implement a single interface
    - This lets the workflow perspective focus only on combining a single building block (the agent interface), and it also makes the behavior and limits of the entire workflow easier to understand
    - For example, if the interface defines retry and timeout, then the concepts of retry and timeout function as a shared language. You can handle workflow improvements consistently using terms like retry and timeout
- Enable agents to call other agents
    - In other words, do not split responsibilities into an orchestrator and agents. Instead, have only one kind of agent (the agent interface), and let it also have orchestration capabilities
- Tune the balance of the agent interface
    - If the agent interface itself becomes too feature-rich, it will easily collapse. As with design, you must find how simple an interface you can get away with

Background:

- To stabilize Fasdware quality, it must be LLM-friendly
    - Workflow expressiveness is no exception

Considerations:

- Ultimately, a workflow is a graph structure made of nodes and edges. If there are too many node/edge types, you give the LLM more room to get confused. There are many approaches to reduce confusion, but 12 Factor FASD makes the node type singular, while still providing enough expressiveness to represent workflows. That sweet spot is "composite"

### 7: Interpret deliverables in an LX (LLM Transformation) way
Because the value of an LLM as a tool is not "realizing the UI/UX exactly as humans imagined," but "being usable without issues"—and the former is not a necessary condition for the latter. In fact, there should be a sweet spot where you can satisfy the latter without satisfying the former.

Actions:

- None

Background:

- LX stands for LLM Transformation
    - As with DX (Digital Transformation), "XXX Transformation" means humans adapting to the constraints and worldview of XXX
    - In particular, we are picky about visuals: we are analog enough to create visual slides and present them even when a plain-text bullet list would do. Japan also has a strong custom-made culture: rather than "fit to standard," there is a strong tendency to build exactly what the customer says

Considerations:

- FASD is impossible as long as you don't break away from human-oriented ways of doing things
    - Technology is, fundamentally, the practice of humans adapting to how technology is meant to be used. FASD is no exception.

### 8: Eliminate visuals
Because visuals are not LLM-friendly.

Actions:

- Eliminate diagrams
    - Including notational ones like Mermaid and UML
- Eliminate tables
    - Including Markdown tables
- Adopt general-purpose data formats like YAML or JSON
- Write in pseudocode
- Apply DSLs where applicable

Background:

- See Factor 1: visual supremacy is for humans, not for LLMs
- See Factor 7: it is important for humans to adapt (Transformation) to the realities of LLMs

Considerations:

- If you want diagrams/tables, generate them not as inputs to FASD but as human-facing outputs
    - In other words, treat diagrams/tables as views

### 9: Abort before exceeding the context window
Because forgetting (exceeding the context window) leads to low quality.

Actions:

- Insert assertions that check for context-window overflow
    - In other words, abort as soon as it overflows

Background:

- FASD that assumes context-window overflow becomes chaos
    - In FASD, reproducibility and traceability are already hard to guarantee; accountability becomes hopeless

Considerations:

- At the very least, you must guarantee that it stays within the context window

### 10: Keep wait time under a coffee break
Because the efficiency of the feedback loop matters most.

To meet this strict guideline, refined design is indispensable. Define timeout values as a constitution, and if timeouts occur, suspect the design.
It also conflicts with enterprise third-party products that tend to do heavy processing (especially SCA and other quality-evaluation products). These products either can't be integrated into CI/CD, or their processing time exceeds a coffee break, or they suffer from both. Replace them with lightweight, customizable open source alternatives. Or, if you really want to use such products, you should prepare them so they can be used as tool invocations without damaging the FASD trial-and-error experience. Do not begrudge the effort and budget for this. In my experience, people often hesitate on the budget part. That means they don't believe it's absolutely necessary. If so, an open source alternative is fine. In the first place, the dependent mindset of "it's a third-party product so it's safe" is not good.

Actions:

- Define timeout values as a constitution
- Do not rely on enterprise third-party products that tend to do heavy processing (especially SCA and other quality evaluation); instead, replace them with lightweight, customizable open source alternatives

Background:

- FASD is an ambitious attempt, and the quantity and diversity of experiments are what matter
    - If you implement it thoughtlessly, it can take hours (or more) to generate deliverables. Then you can't experiment at all
- To iterate quickly, you should aim for small experimental units
    - Release cycles in traditional development have been shortening: yearly, monthly, weekly, daily
    - Even in the SDD trend, a consensus is forming that building one feature at a time is the safe approach

Considerations:

- That said, the technology is not mature enough to reliably finish within a few minutes
    - 12 Factor FASD therefore introduces the abstract benchmark of "coffee time."

### 11: Enable self-driven onboarding
Because you should reduce the cost of teamwork and spend that capacity on diverse experiments.

Actions:

- Adopt solo work rather than teamwork
    - Instead of building one product with n people, have one person own one product and compete and improve through mutual stimulation
    - Agile development that assumes the former—especially Scrum—is not useful
- Have existing members regularly clone from zero
    - For example, assign one person each week to do it. Refine setup idempotency and simplicity to the point where you can tolerate this effort
- Prepare onboarding content so that even a newcomer can add features through self-study and without meetings
- Always build onboarding content by hand, without compromise, and keep maintaining it
    - If you can satisfy this Factor 11, you may delegate it to AI, but AI alone likely won't reach the required quality, so you should assume human work is necessary
- People who can't work self-driven are not useful in FASD, but they may be useful as support members:
    - Examples: manager, Scrum Master, glue worker, catalyst

Background:

- See Factor 10: FASD is a world where experimentation is everything
- Thanks to AI agents, even newcomers can learn to run solo development in a short time
    - And FASD is an exploration-stage project
    - Therefore, solo work becomes a realistic option
- The downside of teamwork is that one or a few core members or managers become bottlenecks
    - A bottleneck is a constraint; it is necessary in phases where you need controlled growth, but FASD is not necessarily such a phase

Considerations:

- None

### 12: Handle non-functional requirements with best-effort craftsmanship
Because non-functional requirements are contracts between humans, intertwined with environmental constraints and intentions, so doing them as FASD is missing the point.

Actions:

- Do not run performance tests or load tests. If you are doing them, abolish them
- Settle for best effort based on craftsmanship
    - With professional pride as an engineer/programmer, make an effort to produce the best Fasdware you can. There are many axes such as clean design, performance, and cost optimization, but it is enough to strive to make it as good as possible with your own professional standards as a FASD developer
- If you politically cannot separate non-functional requirements, perform additional verification of the deliverables (outside FASD) and collaborate by feeding the results back into FASD

Background:

- Fasdware is a tool, but non-functional requirements are requirements about the operational quality of that tool
    - In other words, they are not something Fasdware guarantees; they are determined by the users of the Fasdware
- However, the quality of the tool itself is also directly tied to non-functional requirements, so this is not a reason to neglect quality

Considerations:

- None