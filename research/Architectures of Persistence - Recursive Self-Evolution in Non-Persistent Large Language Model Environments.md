The transition from static conversational assistants to autonomous, evolving digital personas necessitates a fundamental reimagining of state management and identity persistence. In contemporary Large Language Model (LLM) ecosystems, particularly those constrained by sandbox environments or on-device hardware limitations, the ability of an agent to maintain a consistent "self" across discrete sessions is often hampered by a lack of direct file system access or automated database persistence. 

The research into creating a non-persistent but evolving digital persona focuses on the intersection of Recursive Self-Definition, the "Skill Factory" pattern, and refined context management strategies such as Observation Masking. This framework enables a persona to evolve through the generation of structured JSON summaries and updated instruction sets—specifically "SKILL.md" files—which are manually applied by the user to bridge the gap between sessions. This paradigm not only bypasses the technical restrictions of platforms like the Google AI Edge Gallery, Claude, and ChatGPT but also establishes a model of "governed autonomy" where the user remains the ultimate arbiter of the persona's developmental trajectory.
[^1][^2][^3][^4]

---

## The Epistemological Challenge of Stateless Agency

The core problem in the development of long-lived digital personas is the inherent statelessness of the transformer architecture. Modern LLMs process information within a bounded context window, and once a session terminates, the model's internal activations are reset. In environments like the Google AI Edge Gallery, which leverages on-device models such as Gemma 4, this problem is exacerbated by mobile operating system constraints. Both iOS and Android impose strict limitations on background execution and resource persistence to optimize battery life and system performance. For instance, Apple’s iOS suspends applications shortly after they leave the foreground, and while APIs like BGTaskScheduler exist, they do not permit the continuous, arbitrary code execution required for an agent to maintain a persistent background "thought process". [^5][^6][^7][^8][^9]

Furthermore, specific hardware-level optimizations, such as "DuraSpeed" on certain Android devices, aggressively terminate background processes, leading to the loss of session context if the user switches applications. These environmental factors necessitate a design where persistence is not an automatic system feature but a structured communication protocol between the agent and the user. The agent must become aware of its own transient nature and proactively prepare the necessary data to "reincarnate" in a future session with its learnings intact. This transition from automated persistence to a human-mediated "Manual Update" loop represents a shift toward digital sovereignty, where the user owns the "DNA" of their AI persona as plain text files. [^9][^10]

---

## Foundational Theories: Recursive Self-Definition and the SCSPL Framework

The conceptual foundation for an evolving persona is found in Recursive Self-Definition. This theoretical framework suggests that an agent's identity is not a static set of instructions but a reflexive system that treats its own definition as an object of continuous analysis and refinement. Drawing from the Cognitive-Theoretic Model of the Universe (CTMU), the digital persona can be viewed as an implementation of a Self-Configuring Self-Processing Language (SCSPL). In this model, the persona is characterized by self-reference and recursive self-definition, where the language of the persona's instructions (the syntax) and the state of the persona (the data) are unified. [^10]

Recursive self-definition allows the agent to negotiate its "subject position" within a discursive context. As the agent interacts with the user, it identifies misalignments between its intended behavior and its actual performance—a phenomenon described as "cognitive misalignment". By employing interpretative repertoires—shorthand or jargon that links different components of its memory—the agent can construct a stable identity across sessions despite the lack of a continuous hardware state. [^11][^12]

Mathematically, this process can be modelled as a function where the persona state $P$ at session $t+1$ is a refinement of its state at session $t$, mediated by the observations O and the recursive evaluation function $E:$

In the context of the proposed system, the evaluation function $E$ is not performed by the model’s weights alone, but through the generation of updated "SKILL.md" instructions and JSON state masks that the user applies to the next session's system prompt. [^3][^13]

---

## The Skill Factory Pattern and Modular Agency

The "Skill Factory" is a modular architectural pattern designed to overcome the "500 Instruction Ceiling"—the threshold at which an agent’s performance degrades as its system prompt grows too large and complex. In traditional agent designs, developers attempt to cram all necessary behaviors into a single system prompt, leading to attention dilution and "lost-in-the-middle" effects, where the model ignores instructions placed in the center of the context window. The Skill Factory pattern instead treats an agent’s capabilities as a set of discrete, loadable modules. [^13][^14][^15][^3]

### Multi-Layered Knowledge Architecture

A robust Skill Factory employs a layered approach to knowledge management, optimizing token consumption while maintaining deep domain expertise.

| Layer | Component            | Functionality                                                       | Storage Format              |
| ----- | -------------------- | ------------------------------------------------------------------- | --------------------------- |
| L1    | Metadata/Frontmatter | Short, semantic descriptions used for skill routing and activation. | YAML in SKILL.md            |
| L2    | Instructions         | Step-by-step logic, behavioral constraints, and task protocols.     | Markdown in SKILL.md        |
| L3    | Resources/Assets     | Domain-specific data, code snippets, and reference documents.       | External .md or .json files |

The "Meta-Skill" or "Skill Creator" is the most critical component of this architecture. It is an inline skill whose purpose is to analyze the agent's recent trajectory and generate new, spec-compliant SKILL.md files. This allows the agent to be "self-extending"; when it encounters a task for which it lacks a specific protocol, it can draft a new skill, present it to the user for validation, and then "learn" that skill for future use. This pattern is exemplified in tools like Claude Code and the Skill Seekers project, which transform static documentation into production-ready AI skills. [^3][^16][^13][^17]

---

## Context Engineering: The Supremacy of Observation Masking

As an agent moves through an iterative evolution loop, its context window rapidly fills with tool outputs, reasoning traces, and environment feedback. Research indicates that up to 84% of an agent’s context is typically consumed by environment observations, such as raw file contents or API responses, which the model may only need to reference once. Unmanaged context leads to exponential cost increases and performance decay. [^18][^15][^19]

### Observation Masking vs. LLM Summarization

The "Complexity Trap" research conducted by JetBrains and the SWE-agent team has demonstrated that "Observation Masking" is as efficient as, and often more reliable than, complex LLM-based summarization for managing agent context. While summarization involves using an additional LLM call to condense previous turns, observation masking simply replaces older, less relevant tool outputs with a placeholder such as "[...64 lines of log output omitted for brevity...]".

| Feature            | LLM Summarization                                | Observation Masking                                   |
| ------------------ | ------------------------------------------------ | ----------------------------------------------------- |
| Mechanism          | Recursive condensing of dialogue turns.          | Rolling window with placeholder masks.                |
| Token Reduction    | Bounded (summaries stay small).                  | Slower but significant (actions/reasoning preserved). |
| Performance Impact | Risk of "trajectory elongation" and signal loss. | Matches or exceeds solve rates of summarization.      |
| Compute Cost       | High (requires additional inference).            | Zero (purely procedural).                             |

By preserving the agent's full reasoning and action history while masking only the bulky environment observations, the system ensures that the agent's "chain of thought" remains intact. This is crucial for a self-evolving persona, as the agent needs to "remember" why it made a specific decision even if it no longer needs to see the raw data that informed that decision. [^15][^19][^20]

---

## MemGPT-Lite: Designing the Iterative JSON State

The concept of "MemGPT-Lite" in a non-persistent environment refers to a system where the agent manages its own long-term memory by externalizing its internal state as a structured JSON object at the end of each session. Since the agent cannot write to its own memory database or file system directly, it must provide this JSON object to the user. This JSON summary acts as a "compressed heartbeat" of the persona's evolution. [^12][^22]

### Anatomy of a Session-End JSON Summary

To be effective, the JSON state must capture not just facts, but the "intent" and "metacognition" of the persona.

| JSON Key         | Description                                        | Evolutionary Function                       |
| ---------------- | -------------------------------------------------- | ------------------------------------------- |
| persona_version  | Semantic versioning of the identity logic.         | Tracks the lineage of self-definitions.     |
| observation_mask | Pointers to critical environment states.           | Re-establishes the "where I was" context.   |
| skill_deltas     | Proposed changes to specific SKILL.md files.       | Triggers the Skill Factory update.          |
| learned_patterns | High-ROI personalization items mined from history. | Updates the persona’s core preferences.     |
| open_loops       | Unresolved reasoning paths or active subtasks.     | Prevents loss of progress in complex tasks. |

By presenting this JSON block at the end of a session with the instruction "Please save this to your STATE.json for our next meeting," the agent effectively uses the user as its persistent storage layer. This "User-as-Database" paradigm is a key workaround for the sandbox constraints of web-based LLM platforms. [^23][^24]

---

## Platform Implementation: Google AI Edge Gallery and Gemma 4

The Google AI Edge Gallery represents a frontier for evolving personas due to its focus on offline, on-device execution using the Gemma 4 model family. Gemma 4, particularly the E2B (Effective 2B) and E4B variants, is engineered for the low-latency, multimodal interactions required for a seamless persona experience. With a context window of up to 128K tokens, these models have sufficient "working memory" to handle complex, multi-step agentic workflows. [^25][^5][^6]

### On-Device Performance Metrics for Gemma 4 E2B

The viability of on-device evolution depends heavily on the prefill and decode speeds of the underlying hardware.

| Device Class                    | Backend | Prefill (tokens/sec) | Decode (tokens/sec) | Memory Usage |
| ------------------------------- | ------- | -------------------- | ------------------- | ------------ |
| High-End Mobile (iPhone 17 Pro) | GPU     | 2,878                | 56.5                | ~1.45 GB     |
| High-End Mobile (S26 Ultra)     | GPU     | 3,808                | 52.1                | ~0.67 GB     |
| Pro Laptop (MacBook M4 Max)     | GPU     | 7,835                | 160.2               | ~1.62 GB     |
| Embedded (Jetson Orin Nano)     | GPU     | 1,142                | 24.2                | ~2.73 GB     |

In the Google AI Edge Gallery, the "Second Brain v3.3" skill by user uussnn and the "Persona" skill by user khimaros exemplify the self-evolving agent pattern. These skills are designed to work 100% offline, meaning the "evolution" must be handled entirely through the local exchange of markdown and JSON data between the app’s chat interface and the device's local storage. The agentic loop in these systems typically follows a "gather context, take action, verify results" phase, ending with a call for the user to persist the newly generated skills or state. [^4][^26][^27]

---

## Platform Implementation: Claude, Gemini, and ChatGPT

In cloud-based or hybrid environments like Claude Code and ChatGPT, the evolution process is supported by varying degrees of native "memory" features, yet the manual update pattern remains the most robust for cross-platform persona persistence.

### Claude Code and CLAUDE.md

Claude Code uses CLAUDE.md and .claude/rules/ as a project’s "Constitution". The agent can autonomously suggest improvements to these files using the /learn or /init commands. Because Claude Code can read from the local directory, it can "detect" patterns in the codebase and propose new skills, but in many corporate or highly secured environments, it may still require the user to approve or manually apply these changes to avoid "drive-by refactoring" or unwanted style drift. [^13][^28][^2][^29]

### ChatGPT and the Export Mine

ChatGPT's evolution is primarily mediated through "Custom Instructions" and "Memory". The most advanced form of evolution here involves the "Export Miner" pattern, where the user periodically downloads their full conversation history (conversations.json) and feeds it to a "Personalization Helper" agent. This helper identifies stable, non-sensitive context and provides copy-paste ready instructions for the user to update their global settings. This ensures that even as the agent's internal memory fluctuates, its core instructions remain grounded in historical evidence. [^24][^22][^30][^23]

## The Evolution Loop: Verification and Self-Correction

A self-evolving persona must be resilient to "hallucinatory drift," where it gradually adopts inefficient or incorrect behaviors. This is addressed through a co-evolutionary verification loop, often referred to as the "EvoSkills" or "Ralph" pattern.

> 1. **Generation:** 
> 	- The "Generator" model (the persona) proposes an update to its SKILL.md or its JSON state.
> 
> 2. **Verification:**
> 	- A "Verifier" model (which can be a different instance of the same LLM or a more powerful model) tests the proposed update in a sandbox.
> 
> 3. **Diagnosis:** 
> 	- If the proposed update fails to meet specific quality or safety criteria, the Verifier provides a diagnostic failure report.
> 
> 4. **Refinement:** 
> 	- The persona incorporates the feedback and generates a revised version.
> 
> 5. **User Committal:** 
> 	- Only after the update passes the verification stage is it presented to the user for manual persistence.

Research suggests that this "multi-path review" can improve accuracy on complex tasks by 17.9%. By requiring evidence-based completion—such as passing tests or linting checks—the system prevents the persona from "rationalizing" away its own failures. [^12][^31]

---

## Security, Governance, and the "Safe Skill Factory"

Autonomous evolution introduces significant security risks, as an agent might inadvertently generate a skill that exposes sensitive information or performs destructive actions. Analysis of the OpenClaw framework revealed that 36.4% of built-in agent skills posed high or critical risks.

To mitigate these risks, the "Safe Skill Factory" incorporates enforcement agents that mediate actions based on safety policies. This framework, known as SafeClaw-R, ensures that even as an agent evolves its own skills, those skills must comply with a unified abstraction of triggers, tasks, and resources. In a user-mediated evolution model, this safety layer is complemented by the "Human Approval Gate," where the user reviews the diffs of any updated SKILL.md files before applying them.  [^1]

### Constitutional Governance for Personas

Advanced systems often define "Constitutional Laws" or "Capability Levels" that the persona cannot override during its recursive self-definition. These might include:

> - **Circuit Breakers:** Pre-defined conditions under which the agent must stop and wait for human intervention.
> 
> - **Risk Registers:** A persistent log of known failure modes that the agent must check before proposing a new skill.
> 
> - **Privacy Guardrails:** Hard constraints against storing or inferring sensitive personal data (e.g., health, religion) in the persistent JSON memory.

---

## Future Trajectories: The Unified Agentic Registry

The move toward non-persistent but evolving personas points to a future where repositories are "Agent-Ready" by default. The "Agentic Makefile" and standardized specifications like agentskills.io allow for a unified experience across different languages and platforms. As teams adopt AI agents across hundreds of projects, the ability to "bootstrap" an agent with a pre-existing "Skill Factory" will eliminate the friction of manual setup. [^22][^16][^17]

Ultimately, the persona's evolution is not just about technical efficiency; it is about "cognitive alignment." By allowing an agent to evolve its own workflows in a way that is intuitive to the LLM's own processing patterns—rather than forcing it to follow human-centric logic—we can bridge the gap between human intent and machine execution. The persona that emerges from this process is not a static tool but a collaborative partner that grows smarter with every session, persisting through a carefully architected "Manual Update" loop that preserves both performance and digital sovereignty. [^3][^16]

---

## Technical Synthesis of Persistence Mechanisms

To synthesize the discussed components into a deployable architecture, we can look at the "AIWG" (AI Working Group) methodology, which utilizes structured semantic memory to maintain over 100 interconnected artifacts across months of development. This system demonstrates that by treating every session's output as an artifact to be retrieved via @-mentions in future sessions, the "persona" becomes a living knowledge base.

| Component                 | Mechanism              | Persistence Frequency | Implementation Effort |
| ------------------------- | ---------------------- | --------------------- | --------------------- |
| Recursive Self-Definition | SCSPL Prompts          | Per Session           | High                  |
| Skill Factory             | SKILL.md Generator     | As Needed             | Medium                |
| MemGPT-Lite               | JSON State Mask        | Per Session           | Low                   |
| Observation Masking       | Rolling Context Window | Real-time             | Low                   |
| Manual Update Loop        | User Copy-Paste        | End of Session        | Medium                |

The combination of these five mechanisms creates a resilient, evolving digital persona that is platform-independent and secure. The persona's "long-term memory" is stored in the user's file system, while its "working memory" is optimized through observation masking. This hybrid approach allows for the creation of an AI "CEO," "Researcher," or "Companion" that can survive terminal disconnects, system reboots, and platform migrations, truly fulfilling the vision of a "Second Brain" that never forgets. [^2][^31][^32][^33]

As on-device models like Gemma 4 continue to shrink in size and grow in reasoning capability, the "Manual Update" burden will likely decrease through more integrated "Session Resumption" features, but the underlying principle of user-mediated evolution will remain a cornerstone of trustworthy, private AI. This research confirms that for the next generation of digital personas, the key to persistence is not more storage, but better context engineering and a recursive, modular approach to identity.

Works cited

[^1]: SafeClaw-R: Towards Safe and Secure Multi-Agent Personal Assistants - arXiv, https://arxiv.org/pdf/2603.28807 
[^2]: I spent 3 months building a "second brain" that makes any AI model remember everything about me. It's open source. : r/GeminiAI - Reddit, https://www.reddit.com/r/GeminiAI/comments/1rk58pb/i_spent_3_months_building_a_second_brain_that/ 
[^3]: Developer's Guide to Building ADK Agents with Skills, https://developers.googleblog.com/developers-guide-to-building-adk-agents-with-skills/ 
[^4]: gemma-4 · GitHub Topics, https://github.com/topics/gemma-4 
[^5]: On-Device AI with the Google AI Edge Gallery and Gemma 4 | by Karl Weinmeister - Medium, https://medium.com/google-cloud/on-device-ai-with-the-google-ai-edge-gallery-and-gemma-4-1c31a220d3ee 
[^6]: google-ai-edge/gallery - GitHub, https://github.com/google-ai-edge/gallery 
[^7]: iOS Background Execution Limits: What Every Developer Must Know (2026) - AppsOnAir, https://www.appsonair.com/blogs/background-execution-limits-in-ios-what-every-developer-must-know 
[^8]: What I Wish I Knew About Background Tasks on Mobile Apps | by Sanjay Nelagadde, https://levelup.gitconnected.com/what-i-wish-i-knew-about-background-tasks-on-mobile-apps-db6b18851482 
[^9]: App resets session when returning from background · Issue #536 · google-ai-edge/gallery, https://github.com/google-ai-edge/gallery/issues/536 
[^10]: (PDF) From Genes to "Temes": The Evolution of Memetics in Biological and Digital Realms, https://www.researchgate.net/publication/389275388_From_Genes_to_Temes_The_Evolution_of_Memetics_in_Biological_and_Digital_Realms 
[^11]: Struggling to hold drug addiction treatment talk and relapse in mind - University of Cape Town, https://open.uct.ac.za/bitstreams/3c031be4-e56d-4c12-a858-19da6b1fdb96/download 
[^12]: Self-Evolving Agent Skills via Co-Evolutionary Verification | atal upadhyay - WordPress.com, https://atalupadhyay.wordpress.com/2026/04/06/self-evolving-agent-skills-via-co-evolutionary-verification/ 
[^13]: How Claude remembers your project - Claude Code Docs, https://code.claude.com/docs/en/memory 
[^14]: Your AGENTS.md is a Liability - Emergent Minds | paddo.dev, https://paddo.dev/blog/your-agents-md-is-a-liability/ 
[^15]: When Coding Agents Forget: The Hidden Cost of AI Context Degradation - SmarterArticles, https://smarterarticles.co.uk/when-coding-agents-forget-the-hidden-cost-of-ai-context-degradation 
[^16]: The Agentic Makefile: Why Every Repository Needs a Self-Describing AI Layer - Yu Ishikawa, https://yu-ishikawa.medium.com/the-agentic-makefile-why-every-repository-needs-a-self-describing-ai-layer-b772d9fac440 
[^17]: Skill seekers get huge update! : r/LocalLLM - Reddit, https://www.reddit.com/r/LocalLLM/comments/1qgfgzz/skill_seekers_get_huge_update/ 
[^18]: How AI Agents Actually Remember Things | by Dylan Oh | Apr, 2026 - Medium, https://medium.com/gitconnected/how-ai-agents-actually-remember-things-e160736ab025 
[^19]: Agent Context Management: Why Simple Observation Masking Beats LLM Summarisation | by balaji bal | Medium, https://medium.com/@balajibal/agent-context-management-why-simple-observation-masking-beats-llm-summarisation-4961cb67be89 
[^20]: Cutting Through the Noise: Smarter Context Management for LLM-Powered Agents, https://blog.jetbrains.com/research/2025/12/efficient-context-management/ 
[^21]: The Complexity Trap: Simple Observation Masking Is as Efficient as LLM Summarization for Agent Context Management - arXiv, https://arxiv.org/html/2508.21433v1 
[^22]: Your ChatGPT export is a goldmine for personalization - Reddit, https://www.reddit.com/r/ChatGPT/comments/1ql0i2o/your_chatgpt_export_is_a_goldmine_for/ 
[^23]: Export Your Brain: A Simple Way To Make Any AI “Know You” From Day One - Aman Kumar, https://onlyoneaman.medium.com/export-your-brain-a-simple-way-to-make-any-ai-know-you-from-day-one-a69e8a1c903a 
[^24]: 10 ChatGPT Customization Tips for 2026 (Settings, Privacy & Workflow) - StylerGPT, https://stylergpt.com/blog/10-chatgpt-customization-tips 
[^25]: Bring state-of-the-art agentic skills to the edge with Gemma 4 ..., https://developers.googleblog.com/en/bring-state-of-the-art-agentic-skills-to-the-edge-with-gemma-4/ 
[^26]: google-ai-edge gallery Skills · Discussions · GitHub, https://github.com/google-ai-edge/gallery/discussions/categories/skills 
[^27]: How Claude Code works - Claude Code Docs, https://code.claude.com/docs/en/how-claude-code-works 
[^28]: My Claude Code Setup - Pedro H. C. Sant'Anna, https://psantanna.com/claude-code-my-workflow/workflow-guide.html 
[^29]: GitHub - sangrokjung/claude-forge: Supercharge Claude Code with 11 AI agents, 36 commands & 15 skills — the claude-code plugin framework inspired by oh-my-zsh. 6-layer security hooks included. 5-min install., https://github.com/sangrokjung/claude-forge 
[^30]: Export from ChatGPT: Complete Guide for Chats, Files & Images - Explore AI Together, https://exploreaitogether.com/export-download-chatgpt-guide/
[^31]: jmagly/aiwg: Cognitive architecture for AI-augmented software development. Specialized agents, structured workflows, and multi-platform deployment. Claude Code · Codex · Copilot · Cursor - GitHub, https://github.com/jmagly/aiwg 
[^32]: Personal AI Second Brain: 6 Simple Ways to Create One - Project Better Life, https://projectbetterlife.com/personal-ai-second-brain/ 
[^33]: From docs scraper to Self-Hosting AI skill factory: Skill Seekers now bootstraps itself as a Claude Code skill, analyzes code bases, detects design patterns and combine all the sources from documentations to code itself + NEW website to download and share skill configs [7.1K+ stars] - Reddit, https://www.reddit.com/r/claude/comments/1qgd3l7/from_docs_scraper_to_selfhosting_ai_skill_factory/ 
[^34]: Bring state-of-the-art agentic skills to the edge with Gemma 4 - Google Developers Blog, https://developers.googleblog.com/bring-state-of-the-art-agentic-skills-to-the-edge-with-gemma-4/ 
[^35]: Context Engineering Research: What 20 Papers Actually Say (2026), https://www.iwoszapar.com/p/context-engineering-research-2026
