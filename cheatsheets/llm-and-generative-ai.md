# LLM and Generative AI Cheatsheet

Core terms for how large language models work, how to control and customise them, and how to run, secure and evaluate them in production.

> Related: [AI / LLM questions](../topics/06-ai-llm/README.md) · [LLM agents, RAG and MCP skills](../skills/llm-agents-rag-and-mcp.md) · [LLM evaluation skills](../skills/llm-evaluation.md) · [RAG, agents and MCP](rag-agents-and-mcp.md) · [ML fundamentals](ml-fundamentals.md) · [Security and auth](security-and-auth.md)

## Model basics

| Term | What it means |
|---|---|
| **LLM** (Large Language Model) | A neural network trained on huge amounts of text to predict the next token; the engine behind chat, extraction and summarisation features. |
| **Transformer** | The neural architecture behind modern LLMs; attention lets it process whole sequences in parallel and capture long-range context. |
| **Attention** | A mechanism that weights which other tokens matter most for the current one, so context like pronouns resolves correctly. |
| **Self-attention** | Attention where tokens in one sequence look at each other; it is how a model builds context-aware token representations. |
| **Multi-head attention** | Several attention operations run in parallel, each able to pick up different relationships, then combined for a richer representation. |
| **Positional encoding** | Extra information added to tokens to mark their order, because attention alone does not know word position. |
| **Token** | The unit a model reads and writes, often a word piece; pricing, limits and context size are all counted in tokens. |
| **Tokenisation** | Splitting text into tokens; it affects cost and quality, and non-English text often uses more tokens per word. |
| **BPE** (Byte Pair Encoding) | A common tokeniser method that repeatedly merges frequent character pairs into a fixed vocabulary, handling unseen words as smaller pieces. |
| **Embedding** | A list of numbers representing meaning, so similar texts sit close together; the basis of semantic search and clustering. |
| **Logits** | Raw scores a model gives every possible next token before they are converted to probabilities; sampling settings act on them. |
| **KV cache** (Key-Value cache) | Stored attention keys and values for earlier tokens, so generation does not recompute them; a major latency saver. |
| **Encoder vs decoder model** | Encoders turn input into representations (embeddings); decoders generate text; encoder-decoders map input to output, as in translation. |
| **Base model vs chat model** | A base model only continues text; a chat or instruct model is further tuned to follow requests and hold conversations. |
| **Reasoning model** | A model that spends extra tokens working through a problem before answering; better on hard tasks but slower and costlier. |
| **Multimodal model** | A model that accepts or produces more than text, such as images or audio, usually by converting them into token-like representations. |
| **MoE** (Mixture of Experts) | An architecture that routes each token to a few specialised sub-networks, giving large capacity at lower compute per token. |
| **Open-weight model** | A model whose weights can be downloaded and self-hosted; gives control and data isolation but adds operational burden. Licences vary *(verify)*. |
| **Frontier model** | A model at the current leading edge of capability; usually the most capable and most expensive option available. |
| **SLM** (Small Language Model) | A compact model for cheap, fast or on-device tasks; often good enough for routing, classification and extraction. |
| **Knowledge cutoff** | The date a model's training data ends; it does not know later events unless given them through retrieval or tools. |
| **Scaling laws** | Observed relationships showing model performance improves predictably with more data, compute and parameters; used to plan training runs. |

## Context and prompting

| Term | What it means |
|---|---|
| **Context window** | The maximum tokens a model can consider in one request, including input and output; going over causes errors or truncation. |
| **Max output tokens** | A cap on how much the model may generate; set too low, responses cut off mid-answer, which breaks structured output. |
| **Lost in the middle** | The tendency to attend less to content in the middle of long contexts; place key material near the start or end. |
| **Prompt caching** | Reusing the processed form of a repeated prompt prefix, cutting cost and latency for long stable instructions. Details vary by provider *(verify)*. |
| **Context engineering** | Designing everything the model sees, including instructions, retrieved data, tools and history; broader than wording a single prompt. |
| **System prompt** | Standing instructions that set role, rules and format for a conversation; the main place to control behaviour. |
| **Zero-shot prompting** | Asking for a task with no examples; simplest and cheapest, and a good baseline before adding complexity. |
| **Few-shot prompting** | Including a handful of worked examples in the prompt; a quick way to fix format and improve consistency. |
| **CoT** (Chain of Thought) | Asking the model to reason step by step before answering; improves multi-step problems at the cost of more tokens. |
| **Prompt chaining** | Splitting a task into several sequential prompts; easier to test and debug than one big prompt, but adds latency. |
| **Structured output** | Getting responses in a fixed format such as JSON, ideally enforced by schema, so downstream code can parse them reliably. |
| **Delimiters** | Tags or markers separating instructions from data in a prompt; reduce confusion, though they do not stop prompt injection. |

## Sampling and decoding

| Term | What it means |
|---|---|
| **Temperature** | Scales randomness when picking the next token; low is focused and repeatable, high is varied. Zero is not guaranteed deterministic. |
| **Top-p** (nucleus sampling) | Samples only from the smallest token set whose probabilities add up to p; limits unlikely tokens while adapting to confidence. |
| **Top-k** | Samples only from the k most likely next tokens; a fixed cut-off that is simpler but less adaptive than top-p. |
| **Greedy decoding** | Always picking the most likely next token; repeatable and simple, but can produce dull or repetitive text. |
| **Beam search** | Keeps several possible sequences at once and picks the best overall; useful in translation, rarely used for chat. |
| **Constrained decoding** | Blocks tokens that would break a grammar or schema during generation; how strict structured outputs are enforced. |
| **Logprobs** (log probabilities) | The model's log-scale confidence for each generated token; useful for classification thresholds and flagging uncertain outputs. |
| **Stop sequence** | A string that ends generation when produced; stops the model running past the part you need. |

### Temperature vs top-p vs top-k

| Setting | Controls | Typical use |
|---|---|---|
| Temperature | How sharply probabilities are weighted | Low for extraction, higher for creative work |
| Top-p | Cut-off by cumulative probability | Usually tune this or temperature, not both |
| Top-k | Cut-off by fixed count | Hard limit on possible next tokens |

## Training and customisation

| Term | What it means |
|---|---|
| **Pre-training** | Initial large-scale training on general text, teaching language and facts; by far the most expensive stage. |
| **Fine-tuning** | Further training on your examples to change style, format or task skill; poor for facts that change often. |
| **SFT** (Supervised Fine-Tuning) | Fine-tuning on input and desired-output pairs; the standard approach for teaching a task or format. |
| **Instruction tuning** | Fine-tuning a base model on instruction-and-response examples so it follows requests rather than just continuing text. |
| **RLHF** (Reinforcement Learning from Human Feedback) | Training with human preference rankings so a model favours responses people judge better; a key step for assistants. |
| **RLAIF** (Reinforcement Learning from AI Feedback) | Like RLHF, but another model supplies the preference labels; scales feedback at lower human cost. |
| **Constitutional AI** (Constitutional Artificial Intelligence) | A training method from Anthropic where a model critiques and revises outputs against written principles, reducing reliance on human harm labels. |
| **DPO** (Direct Preference Optimisation) | A simpler alternative to RLHF that learns directly from preferred and rejected response pairs, without a separate reward model. |
| **LoRA** (Low-Rank Adaptation) | Fine-tuning that trains small adapter matrices instead of all weights; far cheaper in memory and easy to swap. |
| **QLoRA** (Quantised Low-Rank Adaptation) | LoRA run on a low-precision base model, letting large models be fine-tuned on modest GPUs. |
| **PEFT** (Parameter-Efficient Fine-Tuning) | The family of methods that train only a small share of parameters, including LoRA; also a popular library name. |
| **Reinforcement fine-tuning** | Fine-tuning that rewards verifiably correct outcomes rather than copying examples; suits tasks with checkable answers. |
| **GRPO** (Group Relative Policy Optimisation) | A reinforcement method that scores a group of sampled answers against each other; commonly used for reinforcement fine-tuning. |
| **Catastrophic forgetting** | When new training degrades skills a model already had; check by re-running broader evals after fine-tuning. |
| **Synthetic data** | Artificially generated training or test examples; fills gaps quickly but can miss real-world messiness and inherit errors. |
| **Quantisation** | Storing weights at lower numeric precision, such as 8-bit, to cut memory and speed up inference, with some possible quality loss. |
| **Distillation** | Training a smaller student model to imitate a larger teacher; yields cheaper inference with much of the capability. |

### Prompting vs RAG vs fine-tuning

| Approach | Best for | Weakness |
|---|---|---|
| Prompting | Instructions, format, quick iteration | Limited by context and consistency |
| RAG | Fresh or private knowledge, citations | Quality depends on retrieval |
| Fine-tuning | Style, format, repeated task skill | Slow to update, poor for changing facts |

### Quantisation vs distillation

| | Quantisation | Distillation |
|---|---|---|
| Changes | Precision of the same weights | Trains a new, smaller model |
| Effort | Low | High, needs training data and evals |
| Result | Same model, less memory | Different model, faster and cheaper |

## Serving, latency and cost

| Term | What it means |
|---|---|
| **TTFT** (Time to First Token) | Delay from sending a request to the first generated token; the latency users feel most in streaming interfaces. |
| **ITL** (Inter-Token Latency) | Average gap between generated tokens after the first; determines how smoothly a stream reads. |
| **End-to-end latency** | Total time until the full response arrives, roughly TTFT plus ITL times output tokens; matters for non-streaming workflows. |
| **TPS** (Tokens Per Second) | Generation throughput, per request or across a server; used for capacity planning and comparing serving options. |
| **Streaming** | Sending tokens as they are generated; improves perceived speed but needs handling for partial failures and validation. |
| **Rate limit** | A provider cap on requests or tokens per minute; exceeding it returns HTTP 429, so use backoff, queues and fallbacks. |
| **Batch inference** | Processing many requests asynchronously rather than live; cheaper and higher throughput, suited to work that can wait. |
| **Continuous batching** | Serving technique that adds and removes requests from a running GPU batch per token, raising throughput for self-hosted models. |
| **Speculative decoding** | A small draft model proposes tokens and the main model verifies them together, speeding generation without changing quality. |
| **Model routing** | Sending each request to the cheapest model that can handle it; often the biggest cost lever available. |
| **Fallback** | Switching to another model or provider when one fails or is overloaded; prompts must be tested on both. |
| **Self-hosting** | Running a model on your own GPUs using servers such as vLLM or TGI; gives control but adds operational load. |
| **ZDR** (Zero Data Retention) | A provider arrangement where prompts and outputs are not stored after processing; common enterprise requirement. Terms vary *(verify)*. |
| **Data residency** | A requirement that data is stored or processed in a specific region; affects which providers and endpoints are usable. |
| **Model deprecation** | A provider retiring a model version; plan for pinned versions, regression evals and migration windows. |

## Safety and security

| Term | What it means |
|---|---|
| **Hallucination** | Confident output that is false or unsupported; reduced by grounding, citations and abstention, but it cannot be fully eliminated. |
| **Guardrails** | Checks around a model on inputs, outputs and actions, such as policy filters and schema validation; they enforce rules prompts cannot guarantee. |
| **Prompt injection** | Malicious text that makes a model follow an attacker's instructions rather than the application's; the top LLM security risk. |
| **Direct prompt injection** | The user themselves types the attack into the application, for example asking it to ignore its rules. |
| **Indirect prompt injection** | The attack hides in content the model reads, such as a web page or email; dangerous when tools are connected. |
| **Jailbreak** | A prompt crafted to get around a model's safety training; related to injection but targets the model's own rules. |
| **Red teaming** | Deliberately attacking your own system to find safety and security failures before real users or attackers do. |
| **PII** (Personally Identifiable Information) | Data that identifies a person; must be controlled in prompts, logs and training data for privacy and compliance. |
| **OWASP Top 10 for LLM Applications** (Open Worldwide Application Security Project; Large Language Model) | A widely used list of leading LLM risks, such as injection and excessive agency; contents change between editions *(verify)*. |
| **Excessive agency** | Giving an LLM more tools or permissions than it needs, so a mistake or injection does real damage. |

### Direct vs indirect prompt injection

| | Direct | Indirect |
|---|---|---|
| Source | The user's own input | Retrieved documents, emails, web pages, tool results |
| Main risk | Rule bypass, prompt leakage | Data theft or unwanted actions for an innocent user |
| Key defence | Input and output checks | Treat content as untrusted, least privilege, human approval |

## Evaluation

| Term | What it means |
|---|---|
| **Eval** (evaluation) | A repeatable test measuring LLM output quality against criteria; the way to know a change helped rather than hurt. |
| **Golden dataset** | A curated set of inputs with expected outputs or criteria, used for regression testing; must be maintained as usage changes. |
| **Offline eval** | Testing on a fixed dataset before release; fast and safe, but only as representative as the data. |
| **Online eval** | Measuring real production traffic through sampling and user signals; catches what test sets miss. |
| **A/B test** | Comparing two variants on live users to measure real business impact; the strongest evidence, but slower. |
| **LLM-as-a-judge** (Large Language Model as a judge) | Using a model to score outputs against a rubric; scales evaluation, but the judge itself must be validated against human labels. |
| **Rubric** | Written scoring criteria for a judge or human reviewer; specific rubrics make scores consistent and defensible. |
| **Faithfulness** (groundedness) | Whether every claim in an answer is supported by the provided context; the standard measure for hallucination in RAG. |
| **Answer relevance** | Whether the response actually addresses the question; a faithful answer can still miss the point. |
| **Correct abstention** | Declining to answer when the context lacks the information; measured with deliberately unanswerable test questions. |
| **Regression eval** | Re-running evals after a prompt or model change, to catch quality drops before they ship. |
| **Inter-annotator agreement** | How often human labellers agree; low agreement means the criteria are unclear, and limits achievable model accuracy. |
| **Trace** | A recorded view of one request, including prompts, retrievals, tool calls, tokens and timing; the basis for debugging LLM applications. |
| **ROUGE** (Recall-Oriented Understudy for Gisting Evaluation) | A word-overlap metric for summaries; cheap but a weak proxy for quality, so do not rely on it alone. |
