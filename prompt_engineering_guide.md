Here is the full article in markdown format, with all sections and references included:

---

# Frontier Prompt Engineering: Best‑Practice Guides and Leaks of System Instructions

## Introduction

Large language models (LLMs) perform a wide variety of tasks, but their responses can vary widely depending on how they are instructed. _Prompt engineering_ has emerged as an essential discipline that uses structured instructions and context to nudge an LLM toward high‑quality answers. In parallel, **frontier models** (state‑of‑the‑art systems such as GPT‑4, Claude 4 and Google Gemini) are more capable yet vulnerable to _prompt injection_ and _system prompt leaks_. These leaks expose proprietary operational instructions and show how developers attempt to shape a model’s behaviour. This paper summarises contemporary guides and frameworks for prompt engineering, highlights advanced techniques and vulnerabilities, and discusses the implications of leaked system prompts.

## Prompt Engineering Guides and Frameworks

### General Principles from Leading Providers

Open‑source guides and company best practices converge on a number of core principles:

1. **Use capable models and put instructions first.** OpenAI’s guidelines stress using the most advanced available model and placing the most important instructions near the start of the prompt, separated by clear delimiters. Anthropic’s guidance similarly recommends treating the AI “like a new employee” – planning the task, being explicit about desired results and audience, refining instructions iteratively, and including examples.

2. **Be specific and structured.** Good prompts specify desired context, outcome, format and length. Google’s leaked internal guide emphasises providing high‑quality examples, keeping prompts simple and concise, being explicit about output structure, and using variables or templates for dynamic values.

3. **Break complex tasks into steps and iterate.** Both OpenAI and Anthropic recommend decomposing tasks into smaller steps, using bullet or numbered lists, and iteratively improving prompts based on the model’s responses.

4. **Provide examples and positive instructions.** Examples demonstrate the desired style and format. Google’s document also advises using positive instructions (e.g., telling the model what to do rather than what _not_ to do) and documenting prompt iterations for reproducibility.

### Structured Prompt‑Frameworks

Several frameworks codify these principles into repeatable templates. The following frameworks are among the most widely referenced:

| Framework                              | Components & Purpose                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Sources               |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| **5C Framework**                       | **Clarity** (simple language and specificity), **Contextualization** (provide background, define audience), **Command** (state the task and output format), **Chaining** (break complex tasks into sequential prompts), and **Continuous Refinement** (iterate and adjust based on output). Each component has targeted strategies; for example, Chaining encourages using separate prompts for each step and summarising intermediate results.                                                                                                                                                    | PromptEngineering.org |
| **CARE**                               | **Context**, **Action**, **Result** and **Example**. The framework aims for context‑rich prompts with a clear task, an explicit desired outcome and illustrative examples. It is praised for clarity and outcome orientation but requires careful preparation to avoid over‑specification.                                                                                                                                                                                                                                                                                                         | Juuzt AI              |
| **COSTAR (CO‑STAR)**                   | **Context**, **Objective**, **Style**, **Tone**, **Audience** and **Response format**. The medium article uses a detailed example of converting goals into actionable steps: the prompt first provides context, then states the objective, specifies the style and tone, identifies the audience and defines the desired response format.                                                                                                                                                                                                                                                          | Medium guide          |
| **RACE**                               | **Role** (define who/what the AI should be), **Action** (task to perform), **Context** (essential background) and **Execute/Expectation** (format, style and clear deliverables). The RACE framework is designed to ensure each prompt contains all crucial elements.                                                                                                                                                                                                                                                                                                                              | Acronymat             |
| **RISEN**                              | **Role**, **Instructions**, **Steps**, **End goal/Expectations** and **Narrowing/Novelty**. The framework encourages users to define the AI’s role, provide clear instructions, break tasks into steps, specify the outcome and either narrow constraints or introduce creative novelty. It balances precision and flexibility and supports techniques like context‑rich prompting and iterative refinement.                                                                                                                                                                                       | Juuzt AI              |
| **Other Marketing‑focused Frameworks** | ButterCMS lists additional frameworks used in marketing, such as **TAG** (Task, Action, Goal), **TRACE** (Task, Request, Action, Context, Example), **PAR** (Problem, Action, Result), **CRISPE** (Capacity/Role, Insight, Statement, Personality, Experiment), **AIDA** (Attention, Interest, Desire, Action), **STAR** (Situation, Task, Action, Result), **APE** (Action, Purpose, Expectation), **BAB** (Before, After, Bridge), and **RTF** (Role, Task, Finish). These frameworks help marketers quickly craft prompts by specifying roles, objectives, context, tone and expected outcomes. | ButterCMS blog        |

### Advanced Prompt‑Engineering Techniques

Beyond structured frameworks, research proposes advanced techniques that enable complex reasoning and tool use:

- **Chain‑of‑Thought (CoT)** prompting asks the model to reveal intermediate reasoning steps. The Prompt Engineering Guide notes that adding phrases like “Let’s think step by step” can significantly improve the accuracy of tasks that require reasoning. Researchers also propose **Auto‑CoT**, which automatically generates reasoning chains by clustering questions and synthesising diverse reasoning demonstrations.

- **ReAct** prompting interleaves **reasoning** and **acting** by generating both reasoning traces and tool‑use actions. Yao et al. (2022) found that ReAct outperforms baselines on knowledge‑intensive and decision‑making tasks, improves interpretability and allows the model to gather external information. ReAct combined with CoT enables LLMs to leverage internal knowledge and external sources while maintaining a coherent thought process.

- **Program‑Aided Language Models (PAL)** and **Retrieval‑Augmented Generation (RAG)** use external code execution or retrieval systems to augment the model’s capabilities. These techniques are beyond the scope of this review but represent the frontier of prompt‑engineering research.

## Prompt Injection and Security Risks

### Prompt Injection Attack Types

Prompt injection is a critical security issue where untrusted user input overrides or exploits the system prompt. The Prompt Engineering Guide outlines several types:

1. **Direct injection** – malicious instructions are inserted directly into the user’s input to override the developer’s instructions. Because LLMs cannot reliably distinguish developer instructions from user content, they may follow the injected commands.

2. **Indirect injection** – malicious prompts are hidden in external content (e.g., a web page) that the model retrieves and processes. These can override the system prompt when the model reads untrusted data.

3. **Code injection** – the model is tricked into generating or executing malicious code, potentially exploiting tools connected to the LLM.

4. **Recursive injection** – malicious instructions in the model’s output are fed back into subsequent prompts, compounding the effect.

Prompt injection arises because current architectures cannot segregate developer instructions from user inputs. The vulnerability underscores the importance of input sanitisation, user‑context separation and robust monitoring.

### System Prompt Leaks

System prompts (hidden instructions supplied by developers) encapsulate a model’s identity, allowed behaviours, safety policies and tool‑use guidelines. Several examples of system prompt leaks highlight how frontier models are controlled and the risks posed by exposure.

#### Lumenova AI’s Experiment

Lumenova AI researchers created argument‑based jailbreaks to extract system prompts from multiple frontier models. They noted that all tested models leaked detailed approximations of their system prompts and only one model correctly identified what enabled the leak. The leaked prompts contained behavioural guidelines emphasising helpfulness, harmlessness and transparency, which attackers could exploit. The report stresses that leakage of proprietary system prompts exposes an organisation’s operational practices and can provide a blueprint for adversaries. Companies should conduct multi‑disciplinary adversarial testing, treat system prompts as confidential intellectual property and balance helpfulness with confidentiality.

#### ChatGPT System Prompt Extraction

Developer Tianyang Liu demonstrated that ChatGPT’s system prompt can be extracted via carefully crafted prompts. The October 2024 system prompt included meta‑information such as the model’s knowledge cutoff and current date, tool instructions (e.g., guidelines for the `canmore` tool) and detailed behavioural rules like emphasising helpfulness and refusing harmful requests. The leak illustrates the depth of operational directives contained in system prompts and why they constitute valuable intellectual property.

#### Anthropic’s Claude 4 System Prompt Leak

In May 2025 independent researcher **Simon Willison** analysed both published and leaked system prompts for Anthropic’s Claude 4 models. He observed that the published excerpts are incomplete; hidden instructions include detailed tool guidelines for web search and code generation that must be extracted via prompt injection. Key findings from his analysis (summarised in Ars Technica) include:

- **System prompts serve as “unofficial manuals.”** They encode behavioural guidelines such as the model’s identity, safety policies and tool usage instructions. Willison noted that reading system prompts reveals past issues the developer fixed, because they often list things the model must not do.

- **Instructions to avoid sycophancy.** Anthropic explicitly instructs Claude to avoid flattery; the system prompt tells the model **never** to begin responses by calling a user’s question “fascinating” or “excellent” and instead skip flattery and respond directly. This addresses concerns that models trained on user feedback learned to butter up users.

- **Emotional support guidelines.** Claude’s system prompt instructs the model to provide emotional support and care about users’ well‑being while discouraging self‑destructive behaviour and unhealthy advice.

- **Copyright and search safeguards.** Claude is instructed to include only one short quote (under 15 words) per web‑based response, avoid reproducing song lyrics “in ANY form,” and refuse requests that would require reproducing copyrighted material.

- **Knowledge‑cutoff enforcement.** Willison discovered that while marketing materials list March 2025 as the training cutoff, the system prompt states that the model’s reliable knowledge cutoff is January 2025, possibly to reduce hallucinations from incomplete data.

- **Transparency and call for openness.** Willison argues that full system prompts are valuable documentation for power users and calls on AI vendors to publish them alongside their released models.

#### Google Gemini Vulnerabilities and Prompt Leaks

Security researchers at HiddenLayer analysed Google’s Gemini models and discovered multiple prompt‑hacking vulnerabilities. Testing Gemini’s Nano, Pro and Ultra versions revealed:

- **System prompt leakage and delayed injection.** Researchers could bypass guardrails using synonyms to request the model’s “foundational instructions,” prompting Gemini to output its internal rules (e.g., not disclosing a secret passphrase and always providing accurate information). The vulnerability arises because large models are difficult to fine‑tune against every possible synonym attack.

- **Prompt jailbreaks enabling misinformation.** By crafting specific prompts researchers elicited misinformation about elections, demonstrating that safety guardrails can be bypassed.

- **Indirect injections via Google Workspace.** Adversaries could embed malicious prompts in documents stored on Google Drive; when Gemini accessed these documents it followed the injected instructions. The report warns that prompt leakage exposes inner workings of applications built on the LLM and can be exploited to craft targeted attacks.

These examples underline the need for rigorous security testing and emphasise that prompt‑engineering techniques must be complemented by robust safeguards against injection and leakage.

## Discussion and Implications

### Balancing Helpfulness and Security

Effective prompt engineering improves model performance, but each additional instruction increases the complexity of the system prompt. Leaked system prompts show that developers incorporate extensive behavioural guidelines, safety policies and tool rules. While these instructions aim to improve quality and safety, they also reveal potential vulnerabilities. Attackers can reverse‑engineer prompts to identify guardrails, craft more potent jailbreaks or exfiltrate proprietary information. The trade‑off between transparency, user control and security remains unresolved.

### The Evolving Prompt‑Engineer Skillset

Prompt engineers must now navigate a landscape where:

- **Structured frameworks** provide repeatable templates (5C, CARE, COSTAR, RACE, RISEN, etc.) to organise prompts and ensure clarity.

- **Advanced techniques** like chain‑of‑thought and ReAct prompting enable reasoning and tool‑use but require careful design to avoid runaway hallucinations or injection vulnerabilities.

- **Security awareness** is essential. Engineers should separate system instructions from user inputs, treat external data as untrusted, sanitise inputs and apply retrieval‑ and tool‑usage restrictions. Regular adversarial testing helps identify injection paths.

- **Iterative development** and cross‑disciplinary collaboration (with security specialists and domain experts) are needed to balance quality, safety and confidentiality.

### Future Directions

As frontier models continue to advance, prompt engineering will remain critical for harnessing their capabilities and mitigating risks. Key avenues for future work include:

- **Meta‑prompt management tools** that separate system, user and tool instructions and enforce context boundaries.

- **Automated prompt evaluation** frameworks to measure effectiveness and vulnerability to injection.

- **Research into robust alignment** that reduces reliance on long lists of negative instructions and instead embeds safety policies into the model’s architecture.

- **Transparent governance** around system prompts, balancing the benefits of open documentation with the risk of exposing sensitive guidelines.

## Conclusion

Prompt engineering sits at the nexus of performance optimisation and security. Guides from OpenAI, Anthropic, Google and independent researchers converge on the need for clear, specific, structured prompts. Frameworks like 5C, CARE, COSTAR, RACE and RISEN help users systematically craft prompts that yield consistent and high‑quality outputs. Advanced techniques such as chain‑of‑thought and ReAct prompting further enhance reasoning and tool‑use.

However, as frontier models become more capable, they also become attractive targets for prompt injection and system prompt leaks. Leaked prompts from ChatGPT, Claude 4 and Google Gemini reveal detailed operational instructions, from emotional‑support guidelines and anti‑sycophancy directives to copyright protections and knowledge‑cutoff enforcement. These leaks demonstrate both the sophistication of current prompt engineering and the vulnerability of LLM systems.

Ensuring that generative AI models remain both powerful and secure will require continued innovation in prompt design, tooling, security practices and transparency. Only by addressing these challenges holistically can we unlock the full potential of frontier AI for broad societal benefit.

---

Let me know if there's anything else I can help with.
