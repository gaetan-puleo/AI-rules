# Working with LLMs

## 📚 Introduction

This guide provides practical insights into working effectively with large language models (LLMs), covering:

- What LLMs can (and can't) do  
- How to manage context efficiently  
- How to choose the right model for your use case


## 📖 Understanding LLM Capabilities

### Context Management

When interacting with an LLM, its responses are driven by the context you provide — especially the recent conversation history. Interestingly, the **first tokens** in the context tend to carry more weight.

Each model has its own context window (i.e., the number of tokens it can process at once). For example, **Gemini Pro 2.5** supports up to 1 million tokens — but that doesn't mean you should use all of it.

Providing too much information leads to:
- **Slower responses**, because more tokens require more computation
- **Lower accuracy**, as the model may get distracted from the main task

To manage context effectively:
1. **Start a new conversation for each new task or feature** — don’t carry over unrelated history.  
2. **Stay under 50,000 tokens** when possible — it keeps things fast and focused.  
3. **Avoid debating with the model** — it’s not a person. If it misinterprets instructions, reset and try again with clearer input.

> A 2023 study showed LLMs perform best when key information is placed at the beginning or end of the context.  
-  [🧠 Understanding LLM Context Windows](https://medium.com/@tahirbalarabe2/understanding-llm-context-windows-tokens-attention-and-challenges-c98e140f174d)  
- [📹 Context Rot and LLM Performance](https://www.youtube.com/watch?v=TUjQuC4ugak)
- [Deep Dive into LLMs like ChatGPT ](https://www.youtube.com/watch?v=7xTGNNLPyMI)


### Instruction Following: Use the Right Models

When developing features, planning is critical — you need to define objectives, break tasks into steps, and follow conventions.  
But not all models are good at **instruction following**, which is what matters most in this workflow.

Popular benchmarks often focus on:
- Exercise solving
- General knowledge Q&A

These aren’t always relevant. You don’t need a model that can ace trivia or solve math problems — you need one that **follows instructions consistently and respects project structure**.

#### A Note on "Thinking Models"

“Thinking models” are designed to reason deeply, solve complex problems, and generate structured content. But for typical product development, they often overcomplicate things.

Here’s a counterintuitive insight:  
> **You don’t always want a model that “thinks.”**

Based on Apple’s recent research and my own tests, models like **DeepSeek R1** and **Gemini Pro 2.5** tend to:
- Drift from instructions  
- Add unnecessary reasoning  
- Attempt to generate more than asked

For example, when prompted to create only a spec file, they’ll also try to generate implementation files — which breaks the workflow.

On the other hand, simpler models like **DeepSeek Chat V3** often outperform them for basic instruction-following tasks.

#### Recommendations:

1. **Use models optimized for following instructions.** Personally, I use **Claude Sonnet 4** for this.  
2. **Disable "thinking tokens" if possible** by setting the thinking budget to zero — this forces simpler, more deterministic behavior.  
3. **Smaller models may eventually outperform larger ones** for some tasks, though current evidence is still limited. Be cautious with their shorter context limits.

> "Their reasoning effort increases with problem complexity up to a point, then declines despite having an adequate token budget. By comparing LRMs with their standard LLM counterparts under equivalent inference compute, we identify three performance regimes: (1) low-complexity tasks where standard models surprisingly outperform LRMs, (2) medium-complexity tasks where additional thinking in LRMs demonstrates advantage, and (3) high-complexity tasks where both models experience complete collapse."  
- [Apple: *The Illusion of Thinking*](https://ml-site.cdn-apple.com/papers/the-illusion-of-thinking.pdf)

### Plan → Spec → Code: The Workflow That Keeps AI on Track

When working with AI coding agents (Aider, Claude Code, OpenCode, etc.), one of the most effective ways to ensure reliability is to separate the interaction into three distinct phases:

1. Plan – Use modes like /ask or “Plan” to define the intent of a feature.  
   At this stage, the agent doesn't write or edit files. Instead, it helps you structure the problem:  
   – Clarify the feature's goals and flow  
   – Confirm file/folder architecture and naming conventions  
   – Ask for missing details before moving forward  

   To guide the agent properly, load two files into the context:  
   – ai/rules/project-rules.md: defines the expected spec format and structure  
   – ai/rules/spec-prompt.md: acts as a specialized prompt to generate .spec.md files accurately

   Then prompt the agent like this:  
   **→ “Please create a spec called getTasks, in the group Tasks. It fetches a list of todos from the backend.”**

   **If the feature relies on an API or third-party service, copy and paste the relevant documentation at the end of your message.**

2. Spec – Once the plan is clarified, switch to /code (Aider), Build mode (OpenCode), or command mode (Claude Code) to generate the actual spec file.  
   The .spec.md file becomes a contract: it defines what to build, how to build it, and with what constraints.  
   It improves reproducibility, reduces token usage, and keeps long-term alignment across sessions and tools.

3. Code – After reviewing and finalizing the spec, stay in code mode to generate the feature implementation itself.  
   The agent now writes only what’s described in the spec — nothing more, nothing less.

This three-phase workflow helps scale AI-assisted development while maintaining control over code quality, structure, and project conventions.

→ Plan first. Spec precisely. Then code. That’s how you stay in control.

To go further : 
- [Agentic Claude Code: 3 Codebase Folders for TOP 1% AI Coding](https://www.youtube.com/watch?v=hGg3nWp7afg) ( A highly recommended resource for getting started with AI-assisted and agentic coding. While not all content should be taken at face value, it offers valuable concepts and practical insights worth exploring.)


