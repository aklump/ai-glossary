# AI Glossary

At the [BADCamp AI Summit](https://www.badcamp.org/event/sessions/ai-summit) (Oakland, November 13, 2026), the room brainstormed AI terms people weren't clear on. I photographed the whiteboard and asked Claude to read the terms, research how each is currently used in AI and developer communities, and write this glossary. Terms are alphabetized; where a term has more than one current meaning, each is listed. Older or unrelated senses are left out. Sources are at the end, as of November 2026.

---

## ACP

**Meaning 1 (Agent Client Protocol, the usual dev meaning):** An open JSON-RPC standard, created by the Zed editor team and launched in August 2025 with Google's Gemini CLI as the first integration, that lets any AI coding agent run inside any compatible editor or IDE. JetBrains adopted it in October 2025, and Codex and Claude Code can run over it. It does for agents what the Language Server Protocol did for language tooling: no more one-off editor integrations per agent.

**Meaning 2 (Agentic Commerce Protocol):** A protocol from OpenAI and Stripe that lets AI agents complete purchases on a user's behalf. Unrelated to Meaning 1.

Related: MCP connects an agent to tools and data; ACP (Meaning 1) connects an agent to an editor. The two are designed to work together.

---

## Agent

**Meaning 1 (most common in dev tooling): an LLM running in a loop with tools.** The model receives a goal, decides on an action, calls a tool (read a file, run a command, search the web), observes the result, and repeats until the task is done or it stops. The loop is what makes it an agent rather than a single chat reply.

**Meaning 2 (looser, marketing usage):** Any AI product that performs actions on a user's behalf, even if it is only a fixed workflow with an LLM step. Many things called agents are really scripted pipelines.

---

## AGENTS.md

See **CLAUDE.md / AGENTS.md**.

---

## AI Best Practices

A loose, evolving set of habits for working with AI tools. Commonly cited ones: give clear and specific context, break big tasks into smaller steps, review all generated code, verify claims against sources, keep secrets out of prompts, use version control so changes are easy to revert, and write down project conventions in a file the AI reads (see CLAUDE.md / AGENTS.md). There is no formal standard; practices differ by team and tool.

---

## AI Slop

Low-quality, mass-produced AI output published or shipped as if it were finished work. Merriam-Webster made "slop" its 2025 Word of the Year and defines it as low-quality digital content produced in quantity by AI. A January 2026 academic paper gives three marks: superficial competence, asymmetric effort (cheap to make, costly to check), and mass producibility. In code, it means plausible-looking but bloated or wrong changes submitted without review; a common 2026 example is the flood of fake AI-generated bug-bounty reports. The term is pejorative, and there is no agreed line between slop and legitimate AI-assisted work.

---

## CLAUDE.md / AGENTS.md

A plain markdown file in a repository (and sometimes a user's home directory) that an AI coding agent loads into context at the start of every session. It holds standing instructions: build and test commands, code style, architecture notes, and rules of behavior. It is a context file, not a config file: no schema, no required fields. **AGENTS.md** is the tool-neutral open standard, contributed by OpenAI to the Linux Foundation's Agentic AI Foundation in December 2025 and read by Codex, Cursor, Copilot, Gemini CLI, Zed, Claude Code, and others. **CLAUDE.md** is Claude Code's own file name for the same idea, with Claude Code's memory features layered on top. Common advice: keep AGENTS.md as the source of truth and have tool-specific files point to it. Nested files in subfolders scope instructions to parts of a monorepo.

---

## Context (Window)

**Meaning 1: the context window.** The maximum amount of text, measured in tokens, a model can consider at once. It includes your prompt, the conversation so far, tool results, loaded files, and the model's own reply. Anything outside the window is invisible to the model.

**Meaning 2: context (informal).** Whatever information you have given the model for the current task: files, instructions, examples, history. "Give it more context" means this.

Related: *context engineering* is the practice of deciding what goes into the window and when.

---

## Context Bloat

When the context window fills with material the current task doesn't need: long tool outputs, whole files read for one line, stale conversation history, oversized CLAUDE.md or AGENTS.md files, or too many loaded tools and MCP servers. Bloat costs tokens on every turn, slows responses, and tends to make results worse, because the model has more irrelevant text to sort through and can lose track of instructions buried in it. Common fixes: start a fresh session for a new task, compact or clear the conversation, hand searches to subagents so only their summary comes back, keep standing instruction files short, and load skills and tools on demand rather than all at once. Related: *context engineering* (see **Context (Window)**).

---

## Dev Tooling

In AI conversation, the AI-powered tools used in software development: agentic CLIs (Claude Code, Codex CLI, Gemini CLI), AI-native editors (Cursor, Zed), IDE extensions, code review bots, and the plumbing around them (MCP servers, rules files, hooks, worktree managers). The phrase also covers the practice of configuring and extending those tools for a team.

---

## GEO / AEO

Two related acronyms for optimizing content so AI systems surface and cite it, the AI-era counterpart to SEO.

**GEO, Generative Engine Optimization:** Shaping content so generative AI products (chatbots, AI search summaries) mention or cite your brand and pages.

**AEO, Answer Engine Optimization:** Structuring content to be chosen as the direct answer by answer-style systems, including voice assistants, featured snippets, and AI answers.

The two are often used interchangeably, and GEO has become the more common label. Both sit on top of ordinary SEO rather than replacing it; Google has said there is no separate AI optimization track. Success is measured in brand mentions, citations, and AI referral traffic rather than rank position.

---

## Guardrails

**Meaning 1 (safety and content):** Controls that keep a model's inputs and outputs within acceptable bounds, such as filters and classifiers that block harmful, off-topic, or sensitive content, plus refusal rules in the system prompt.

**Meaning 2 (agent and developer controls):** Limits on what an agent is allowed to do, such as permission prompts, sandboxing, allow and deny lists, hooks that block specific commands, step or spend caps, and required human approval for risky actions.

Instructions in a prompt are a soft guardrail because a model can ignore or be talked out of them. Guardrails enforced in code or permissions are harder to bypass.

---

## Hallucination

When a model produces output that is fluent and confident but false or unsupported: invented facts, fake citations, nonexistent functions or packages, or misreported results. It happens because models predict likely text rather than look up verified truth. Mitigations include grounding with retrieval (RAG), giving tools for verification, and human review.

---

## Harness

**Meaning 1 (agent harness):** The software wrapped around a model that turns it into a working agent. Anthropic defines it as the system that processes inputs, orchestrates tool calls, and returns results. It owns the loop, tool execution, permissions, context assembly, state, and error recovery; the model only decides what to do next. Claude Code, Codex, Cursor's agent, and the Claude Agent SDK are harnesses. The common shorthand: "Agent = Model + Harness. If you're not the model, you're the harness." *Harness engineering* is the 2026 term for designing this layer, and it is now treated as a bigger performance lever than model choice; Anthropic's 2026 report says harness setup alone can move benchmarks by several points.

**Meaning 2 (evaluation):** An *eval harness* or *test harness* is code that runs a model or program against a set of tests and scores the results.

---

## In/Out (Tokens In / Tokens Out)

See **Token**.

---

## LLM (Large Language Model)

A neural network trained on very large amounts of text to predict the next token, which lets it generate, summarize, translate, reason about, and write code. Examples include Claude, GPT, Gemini, and Llama. "LLM" describes the type of model; people often use it loosely as a synonym for "AI."

---

## Loops

**Meaning 1 (agent loop):** The core cycle of an agent: think, act with a tool, observe the result, repeat until done.

**Meaning 2 (iteration workflows):** Deliberately repeating a task until a condition is met, for example re-running an agent until the tests pass, or having one model draft and another critique.

**Meaning 3 (failure mode):** A *doom loop*, where an agent repeats the same failing action without progress.

---

## MCP (Model Context Protocol)

An open standard for connecting AI applications to external tools and data sources, introduced by Anthropic in November 2024 and donated to the Linux Foundation's new Agentic AI Foundation in December 2025, where it is now governed. An **MCP server** exposes capabilities (tools, resources, prompt templates) over JSON-RPC, and an **MCP client** inside the AI app calls them, so one integration works across Claude, ChatGPT, Copilot, Gemini, and most coding agents. More than 10,000 MCP servers have been published. Often described as "USB-C for AI tools."

---

## ML (Machine Learning)

The broader field of building systems that learn patterns from data rather than following hand-written rules. Deep learning is a subset of ML, and LLMs are a subset of deep learning. In a developer setting, ML usually means training and deploying models, as opposed to simply calling one through an API.

---

## Mods

**Meaning 1 (Claude Code mods):** Small JavaScript or TypeScript functions that change how Claude Code behaves and looks, introduced by Anthropic on October 1, 2026 and on by default from version 2.1.287. A mod hooks into events inside Claude Code: it can rewrite a prompt before it reaches the model, block or alter a tool call, approve or deny a permission request, redact secrets from tool output, draw its own panels and buttons, or replace parts of Claude Code's interface. Mods ship inside plugins alongside skills and MCP servers. Some of Claude Code's own features, such as /diff and AGENTS.md support, are mods. They are not sandboxed, so they carry the same trust question as any installed code.

**Meaning 2 (informal):** Any customization of an AI tool or setup: plugins, hooks, rules files, custom system prompts, themes. Borrowed from game modding.

---

## Model

The trained neural network itself: the weights plus architecture that turn input into output. Examples: Claude Sonnet, GPT-5, Gemini, Llama. Models differ in capability, speed, cost, and context window size, and most harnesses let you switch between them. The model is the engine; everything around it (prompts, tools, loop, permissions) is the harness.

---

## Natural Language

Human language such as English or Spanish, as opposed to formal languages like code or SQL. In AI, "natural language" is the interface: you describe what you want in ordinary words instead of exact syntax. *Natural language processing (NLP)* is the field of getting computers to handle it.

---

## One-Shot

**Meaning 1 (prompting):** Giving the model exactly one example of the desired input and output within the prompt. Compare *zero-shot* (no examples) and *few-shot* (several examples).

**Meaning 2 (agentic coding, the common dev usage):** Getting a complete, working result from a single initial prompt with no human input after it. "The agent one-shotted the feature." It does not mean a single model call; the agent may run for hours. Critics note that one-shot demos hide how much work the prompt's wording and context did, and that most one-shot failures are context failures rather than model failures.

The two meanings are unrelated, so check which one is intended.

---

## Orchestration

**Meaning 1 (multi-agent):** Coordinating multiple agents or model calls: deciding who does what, passing results between them, handling failures, and combining outputs. Often a lead agent delegates to subagents.

**Meaning 2 (workflow engineering):** Sequencing and managing LLM calls, tools, retrieval, and control flow in an application, as frameworks like LangGraph do.

---

## Parallelability

How well a task can be split into independent pieces that several agents run at the same time: subagents researching different areas, or parallel sessions each implementing a separate feature. The usual isolation primitive is a git worktree per agent, so no two agents edit the same checkout; Claude Code, Codex, and Cursor all support worktree-based parallel work, and Claude Code's experimental "agent teams" let parallel agents talk to each other. Work where each step depends on the previous result cannot be parallelized. Costs include merge conflicts, the effort of combining results, multiplied token spend, and rate limits.

This is an informal term; "parallel agents" and "parallelizability" are the more common spellings of the same idea.

---

## Persona

A role, identity, and voice assigned to a model or agent, usually through the system prompt or a dedicated file, such as "senior security reviewer" or "friendly support agent." It shapes tone, focus, and how the agent handles ambiguity. In multi-agent setups each subagent often gets its own persona. Related: *Soul*, which is the current file-based way of writing one down.

---

## Prompt

**Meaning 1:** The input text sent to a model to get a response, including instructions, examples, and data.

**Meaning 2 (system prompt):** Standing instructions set by the developer or application that shape the model's role and behavior across the whole conversation, separate from the user's messages.

**Meaning 3 (user prompt):** The individual message a user types in a given turn.

*Prompt engineering* is the craft of writing prompts that reliably produce good results.

---

## Providers

**Meaning 1:** Companies that train and serve models through an API, such as Anthropic, OpenAI, and Google.

**Meaning 2:** Hosting and routing platforms that serve models from several labs, including cloud platforms (AWS Bedrock, Google Vertex AI, Azure) and aggregators such as OpenRouter.

Most harnesses let you swap providers, which matters for cost, availability, data residency, and model choice.

---

## RAG (Retrieval-Augmented Generation)

A technique where the system first retrieves relevant documents from an external source, typically via embeddings and a vector database or via search, and inserts them into the prompt before the model answers. This lets a model use private or up-to-date information and reduces hallucination without retraining. A closely related pattern is agentic search, where the model uses search tools directly instead of a fixed retrieval pipeline.

---

## Second Brain (2nd Brain)

A personal knowledge base of notes kept outside your head, from Tiago Forte's *Building a Second Brain*. In 2026 developer usage it almost always means a folder of plain markdown notes (usually an Obsidian vault) that an agent such as Claude Code reads, searches, and writes back to, giving the agent persistent memory across sessions. A CLAUDE.md or AGENTS.md at the vault root tells the agent how the notes are organized. The appeal is that markdown is portable: if the app or the agent changes, the memory still works.

---

## Session

A single continuous working run with an AI tool, with its own accumulated context. When it ends or is cleared, the model does not remember it unless something was saved to a file or memory system. Agentic tools save session transcripts to disk and let you resume, fork (branch a new session from an old one), compact (summarize to free context), or name them.

Sessions can often be resumed, forked, or compacted in agentic tools.

---

## Skills

**Meaning 1 (Agent Skills):** Folders of instructions, scripts, and resources that an agent loads on demand for a specific kind of task, such as creating spreadsheets or following a team's review process. Each skill is a directory with a SKILL.md file holding a name, a description, and instructions. Only the name and description sit in context until the skill is triggered, then the full file loads: Anthropic calls this progressive disclosure. Introduced by Anthropic in October 2025 and since published as an open standard (agentskills.io) adopted by other agent products. Where AGENTS.md sets always-on behavior, a skill is a triggerable procedure.

**Meaning 2 (informal):** Any capability of a model or agent, as in "it's good at coding."

---

## Soul

**Meaning 1 (SOUL.md):** A markdown file that defines an agent's identity, values, voice, and behavioral boundaries, separate from its tools and project instructions. The convention came out of the OpenClaw ecosystem and is now supported by several agent tools; there is also a community Soul.md specification and at least one open "Soul Spec" package format. A SOUL.md is loaded into every session as standing instructions, typically 30 to 80 lines, and sits beside AGENTS.md or CLAUDE.md rather than replacing them. A variant use is a soul file written about *you*, so an agent can write and think in your voice.

**Meaning 2 (figurative):** The quality that makes AI output feel human or distinctive, as opposed to generic slop.

---

## Spawning

Starting a new agent or subagent from within a running one to handle a task, usually with its own fresh context window. The parent describes the task, the child works independently, and a summary comes back. The word is borrowed from operating systems, where a process spawns child processes.

---

## Subagents

Separate agent instances that a main agent spawns to handle a delegated subtask. Each runs in its own fresh context window, often with a custom system prompt, a restricted tool set, and its own permissions, then returns a condensed result to the parent; the parent never sees the raw tool output. Benefits: parallel work, focus, and keeping the parent's context clean. Subagents report only to the parent and do not talk to each other, which is what separates them from Claude Code's experimental agent teams. For parallel code changes they are usually paired with git worktrees so they do not collide on files.

---

## Thread

A sequence of messages that forms one ongoing conversation with a model or agent, saved so it can be resumed. Codex uses it as the formal name for a saved session: every Codex session gets a thread ID, and the CLI and app expose `thread/start`, `thread/resume`, and fork commands. Many APIs and agent frameworks use the same word for a persisted message history. In this sense a thread and a *Session* are the same thing seen from the storage side.

---

## Token

The unit of text a model reads and writes: a word, part of a word, or punctuation. In English a token is roughly three-quarters of a word. Models are limited and billed by token counts, and the context window is measured in them.

**Tokens in / tokens out:** *Input tokens* are what you send (prompt, history, files); *output tokens* are what the model generates. They are priced differently, with output usually costing more.

---

## Tools

Functions an AI model can request to be run, such as reading a file, running a shell command, searching the web, or calling an API. The model emits a structured call, the harness executes it, and the result goes back to the model. Tools are how a model affects the world beyond producing text, and the tool loop is what makes an agent an agent. Also called tool use or function calling.

Related: MCP is a standard way to supply tools to a model.

---

## Sources

- [Agent Client Protocol (Zed)](https://zed.dev/acp) and [Bring Your Own Agent to Zed](https://zed.dev/blog/bring-your-own-agent-to-zed)
- [AGENTS.md overview (Atlan docs)](https://docs.atlan.com/agents/concepts/agents-md)
- [Linux Foundation announces the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-Agentic-ai-foundation)
- [MCP joins the Agentic AI Foundation](https://modelcontextprotocol.info/blog/joins-agentic-ai-foundation/)
- [Equipping agents for the real world with Agent Skills (Anthropic)](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) and [Agent Skills spec](https://cdn.jsdelivr.net/gh/agentskills/agentskills@main/README.md)
- [Customize Claude Code with mods (Anthropic)](https://claude.com/blog/claude-code-mods) and [Mods overview (Claude Code docs)](https://code.claude.com/docs/en/plugins/mods/overview)
- [How and when to use subagents in Claude Code (Anthropic)](https://claude.com/blog/how-and-when-to-use-subagents-in-claude-code)
- [Agent Harness Engineering (O'Reilly Radar)](https://www.oreilly.com/radar/agent-harness-engineering/) and [What Is an Agent Harness?](https://sidbharath.com/blog/agent-harness-guide/)
- [Context Engineering (getunblocked)](https://getunblocked.com/blog/context-engineering/)
- [SOUL.md (BrowserOS docs)](https://docs.browseros.com/features/soul.md) and [Soul.md specification](https://github.com/rokoss21/soul.md)
- [Obsidian second brain with Claude Code](https://okhlopkov.com/second-brain-obsidian-claude-code/)
- [Codex CLI session management](https://codex.danielvaughan.com/2026/04/13/codex-cli-session-management-resume-fork-transcripts)
- [Git worktrees for parallel AI agents (Augment)](https://augmentcode.com/guides/git-worktrees-parallel-ai-agent-execution)
- [What are agent guardrails? (Speakeasy)](https://www.speakeasy.com/resources/agent-guardrails)
- [One-shotting a static site generator](https://acq-uy.pages.dev/posts/one-shotting-a-static-site-generator/) and [Who One-Shot Ya?](https://prismatic.io/blog/who-one-shot-ya/)
- [GEO vs AEO (Jasper)](https://www.jasper.ai/blog/geo-aeo) and [GEO + AEO Guide (RightBlogger)](https://rightblogger.com/blog/generative-engine-optimization)
- [What Is AI Slop (eWeek)](https://www.eweek.com/news/what-is-ai-slop/) and [AI slop definitions (Mint Research)](https://mintresearch.org/reports/ai-slop/)
