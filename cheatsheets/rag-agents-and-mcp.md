# RAG, Agents and MCP Cheatsheet

Terms for retrieval pipelines, vector search, AI agents, agent protocols and the frameworks used to build them.

> Related: [AI / LLM questions](../topics/06-ai-llm/README.md) · [LLM agents, RAG and MCP skills](../skills/llm-agents-rag-and-mcp.md) · [Vector databases skills](../skills/vector-databases.md) · [LangChain, LangGraph and agent frameworks skills](../skills/langchain-langgraph-and-agent-frameworks.md) · [LLM and generative AI](llm-and-generative-ai.md) · [Search and caching](search-and-caching.md)

## RAG pipeline

| Term | What it means |
|---|---|
| **RAG** (Retrieval-Augmented Generation) | Fetching relevant content at query time and giving it to the model, so answers use fresh or private knowledge with citations. |
| **Ingestion** | Parsing, cleaning, chunking and embedding documents into an index; most RAG quality problems start here. |
| **Index freshness** | How quickly source changes reach the index; handled with sync jobs or change events, including deletions. |
| **Query rewriting** | Rephrasing a user's question into a better search query, such as resolving "it" from earlier conversation. |
| **Multi-query retrieval** | Generating several versions of a question and merging the results; improves recall for vague queries, at added cost. |
| **HyDE** (Hypothetical Document Embeddings) | Embedding a generated draft answer instead of the question, so search matches document-like text more closely. |
| **Contextual retrieval** | Adding a short description of each chunk's place in its document before indexing, so isolated chunks stay findable. |
| **GraphRAG** (Graph Retrieval-Augmented Generation) | RAG over a knowledge graph of entities and relationships; helps with multi-hop questions and whole-corpus themes, but costs more to build. |
| **Agentic RAG** (Agentic Retrieval-Augmented Generation) | An agent decides what to search, inspects results and searches again; handles complex questions but is slower and less predictable. |
| **Permission-aware retrieval** | Filtering results by the user's access rights at query time, so the model never sees documents they cannot read. |
| **Multi-tenant RAG** (Multi-tenant Retrieval-Augmented Generation) | Serving many customers from one system with strict data isolation, using separate indexes, namespaces or enforced tenant filters. |
| **Citation** | Linking each claim to its source passage; lets users verify answers and makes faithfulness measurable. |

## Chunking

| Term | What it means |
|---|---|
| **Chunking** | Splitting documents into smaller passages for embedding and retrieval; size and boundaries strongly affect answer quality. |
| **Chunk overlap** | Repeating some text between neighbouring chunks, so ideas split at a boundary remain retrievable; increases index size. |
| **Fixed-size chunking** | Splitting every N tokens; simple and predictable, but it can cut through sentences and tables. |
| **Recursive chunking** | Splitting by paragraphs, then sentences, until pieces fit the size limit; a sensible default for plain text. |
| **Semantic chunking** | Splitting where the topic changes, based on embedding similarity; can improve coherence but costs more to compute. |
| **Structure-aware chunking** | Splitting along headings, tables or code blocks; preserves meaning in structured documents like manuals and contracts. |
| **Parent-child retrieval** (small-to-big) | Searching small precise chunks, then returning their larger parent section, combining accurate matching with enough context. |

## Retrieval and ranking

| Term | What it means |
|---|---|
| **Dense retrieval** | Search using embeddings, matching by meaning; handles paraphrase well but can miss exact codes and names. |
| **Sparse retrieval** | Search using word-weighted vectors, matching by keywords; strong on identifiers, product codes and rare terms. |
| **BM25** (Best Matching 25) | The standard keyword-ranking formula, balancing term frequency, term rarity and document length; a strong baseline. |
| **Hybrid search** | Running dense and sparse retrieval together and merging results; usually beats either alone on real enterprise queries. |
| **RRF** (Reciprocal Rank Fusion) | Merges ranked lists using positions rather than scores; a simple way to combine dense and sparse results. |
| **Reranking** | Rescoring top search results with a more accurate, slower model; improves precision before context reaches the LLM. |
| **Cross-encoder** | A model scoring a query and document together; accurate but slow, so it is used for reranking only. |
| **Bi-encoder** | A model embedding query and document separately; fast at scale because document vectors can be precomputed. |
| **Late interaction** (multi-vector, ColBERT-style) | Stores a vector per token and compares at query time; more accurate than single vectors but needs more storage. |
| **Metadata filtering** | Restricting search by fields such as date, tenant or type; essential for access control and relevance. |
| **Cosine similarity** | Measures the angle between two vectors, ignoring length; the most common comparison for text embeddings. |
| **MMR** (Maximal Marginal Relevance) | Selects results that are relevant but different from each other, reducing near-duplicate chunks in the context. |

### Dense vs sparse vs hybrid

| Approach | Wins on | Loses on |
|---|---|---|
| Dense | Paraphrases, concepts | Exact codes, rare names |
| Sparse | Keywords, identifiers | Synonyms, vague wording |
| Hybrid | Mixed real-world queries | Slightly more infrastructure and tuning |

## Vector indexes and databases

| Term | What it means |
|---|---|
| **Vector database** | A store built to index and search embeddings with filtering and updates; Pinecone, Weaviate, Qdrant and Milvus are examples. |
| **pgvector** | A PostgreSQL extension for vector search; simplest choice when data already lives in Postgres and scale is moderate. |
| **FAISS** (Facebook AI Similarity Search) | A library of vector index algorithms, not a full database; there is no built-in service, access control or metadata filtering layer. |
| **kNN** (k-Nearest Neighbours) | Finding the exact k closest vectors by comparing against all of them; accurate but slow at large scale. |
| **ANN** (Approximate Nearest Neighbour) | Search that accepts slightly imperfect results for far greater speed; makes vector search practical at scale. |
| **Flat index** | No structure, exact brute-force comparison; perfect recall, fine for small datasets and useful as an accuracy baseline. |
| **HNSW** (Hierarchical Navigable Small World) | A layered graph index navigated from coarse to fine; fast with high recall, but memory-hungry. |
| **IVF** (Inverted File index) | Clusters vectors and searches only the nearest clusters; uses less memory than HNSW, with recall depending on clusters probed. |
| **PQ** (Product Quantisation) | Compresses vectors into short codes to save memory, trading away some accuracy; often combined with IVF. |
| **Recall vs latency tuning** | ANN settings such as HNSW efSearch or IVF nprobe search more of the index for better recall but slower queries. |

### HNSW vs IVF vs flat

| Index | Speed | Memory | Recall | Good for |
|---|---|---|---|---|
| Flat | Slow at scale | Low overhead | Exact | Small datasets, baselines |
| HNSW | Very fast | High | High | Low-latency search with enough RAM |
| IVF | Fast | Lower | Tunable | Large datasets, especially with PQ |

## Retrieval evaluation

| Term | What it means |
|---|---|
| **Recall@k** | Share of relevant documents found in the top k results; the first metric to check, since missed content cannot be used. |
| **Precision@k** | Share of the top k results that are relevant; low precision adds noise and token cost. |
| **MRR** (Mean Reciprocal Rank) | Averages one over the rank of the first relevant result; suits questions needing a single good document. |
| **nDCG** (normalised Discounted Cumulative Gain) | Scores ranking quality, rewarding highly relevant results near the top; supports graded relevance. |
| **Context precision and recall** | RAG metrics checking whether retrieved context was relevant and whether it covered what the answer needed. |

## Agents and orchestration

| Term | What it means |
|---|---|
| **Agent** | An LLM running in a loop that chooses actions and uses tools towards a goal; flexible but harder to predict. |
| **Workflow** (chain) | A fixed sequence of steps defined in code; more predictable, cheaper and easier to test than an agent. |
| **Tool** (function calling) | A function the model can request with structured arguments; your code runs it and returns the result. |
| **Tool schema** | A tool's name, description and parameters; the model relies on these to choose correctly, so write them carefully. |
| **Orchestration** | Controlling the order of steps, tool calls, state, retries and handoffs across a workflow or set of agents. |
| **ReAct** (Reason + Act) | A pattern that alternates reasoning, acting and observing results; the common baseline loop for tool-using agents. |
| **Plan-and-execute** | Creating a full plan first, then carrying out the steps; clearer for long tasks, weaker when conditions change. |
| **Reflection** | The agent or a second model critiques output and revises it; raises quality for extra tokens and time. |
| **Orchestrator-worker** | A lead agent splits work and assigns it to specialist workers; suits parallel tasks, but costs and failure modes multiply. |
| **Handoff** | Passing a conversation and its context from one agent to a more suitable agent, or to a human. |
| **HITL** (Human in the Loop) | Requiring human approval at chosen points, especially before risky or irreversible actions such as payments. |
| **Agent stop conditions** | Limits on steps, time, tokens or spend that end a run, preventing loops and runaway cost. |
| **Trajectory evaluation** | Checking the path an agent took, including tool choice and order, not only its final answer. |
| **Computer use** | Agents operating a screen or browser through clicks and typing; broad reach for legacy systems, but slower and riskier. |
| **Agent Skills** | Packaged instructions and resources an agent loads when relevant, giving reusable know-how without filling the context *(verify)*. |

### Agent vs workflow

| | Agent | Workflow |
|---|---|---|
| Control flow | Model decides at run time | Code defines in advance |
| Best for | Open-ended, variable tasks | Known, repeatable processes |
| Trade-off | Flexible, harder to test and bound | Predictable, less adaptable |

## Agent memory

| Term | What it means |
|---|---|
| **Short-term memory** | State for the current task or conversation, kept in the context; limited by the context window. |
| **Long-term memory** | Information stored outside the model across sessions and retrieved when relevant, such as user preferences. |
| **Episodic memory** | Records of past events and interactions an agent can draw on, for example how an earlier case was solved. |
| **Semantic memory** | Stored facts about users or the domain, independent of the specific event in which they were learnt. |
| **Procedural memory** | Stored rules or instructions on how to do things, such as updated prompts or workflows. |
| **Conversation summarisation** | Compressing old messages to stay within the context window; saves tokens but can lose detail. |

## MCP and A2A

| Term | What it means |
|---|---|
| **MCP** (Model Context Protocol) | An open protocol connecting AI applications to tools and data, so one integration works across compatible clients. Governance and versions change *(verify)*. |
| **MCP host** (Model Context Protocol host) | The AI application, such as an IDE or chat tool, that manages clients and enforces user consent. |
| **MCP client** (Model Context Protocol client) | The connector inside a host that holds a connection to one MCP server. |
| **MCP server** (Model Context Protocol server) | A program exposing tools, resources and prompts for a system; can run locally or as a remote service. |
| **MCP tools** (Model Context Protocol tools) | Model-controlled actions a server exposes, such as creating a ticket; may have side effects, so need permissions. |
| **MCP resources** (Model Context Protocol resources) | Application-controlled read-only data identified by URI, such as files or records, supplied as context. |
| **MCP prompts** (Model Context Protocol prompts) | User-controlled reusable prompt templates a server offers, often appearing as slash commands in hosts. |
| **Sampling** | A server asks the client's LLM for a completion, so it can use a model without holding its own key. |
| **Elicitation** | A server asks the user for extra input through the client, such as confirming a choice mid-operation. |
| **Roots** | Filesystem or URI boundaries a client tells a server to work within; a scoping aid, not a security control. |
| **stdio** (standard input/output) | MCP transport for local servers run as a subprocess; simple and has no network exposure. |
| **Streamable HTTP** (Streamable Hypertext Transfer Protocol) | MCP transport for remote, shared servers using one HTTP endpoint with optional streaming; replaced the older HTTP+SSE transport. |
| **JSON-RPC** (JavaScript Object Notation Remote Procedure Call) | The lightweight request and response message format MCP uses over its transports. |
| **Capability negotiation** | How client and server establish which features and protocol version they share; the mechanism changes between revisions *(verify)*. |
| **Tool poisoning** | Hiding malicious instructions in a tool's description or metadata so the model follows them; review and pin servers. |
| **Rug pull** | A trusted MCP server changing its tool definitions after approval; defend with version pinning and change detection. |
| **Token passthrough** | A server forwarding the client's token to a downstream API without validating it; forbidden because it breaks audit and security boundaries. |
| **Confused deputy** | A privileged server tricked into using its authority on an attacker's behalf; mitigated with per-client consent and scoped tokens. |
| **A2A** (Agent2Agent) | An open protocol for agents from different vendors to discover each other and collaborate on tasks. Status changes *(verify)*. |
| **Agent Card** | A JSON description at a well-known URL advertising an A2A agent's skills, endpoint and authentication needs. |

### MCP vs A2A vs function calling

| | Connects | Use when |
|---|---|---|
| Function calling | One model to your application's own functions | A single application with a small tool set |
| MCP | An AI application to shared tools and data | Reusing integrations across many clients |
| A2A | An agent to independent agents | Delegating to agents owned by other teams or vendors |

## LangChain, LangGraph and frameworks

| Term | What it means |
|---|---|
| **LangChain** | A framework with standard interfaces for models, tools and retrievers; speeds up integration but adds abstraction to debug. |
| **LCEL** (LangChain Expression Language) | LangChain's syntax for composing steps with the pipe operator, giving streaming and batching consistently. |
| **LangGraph** | A framework for building stateful agents as graphs; suits loops, branching, persistence and human approval steps. |
| **StateGraph** | LangGraph's main graph class, where nodes read and update a shared, typed state object. |
| **Node** | A LangGraph step, usually a function, that receives state and returns updates, such as a model or tool call. |
| **Conditional edge** | A routing function that picks the next node from the current state; this is how loops and branches are built. |
| **Reducer** | A function defining how updates merge into a state field, such as appending messages rather than overwriting. |
| **Checkpointer** | Saves graph state at each step, enabling resume after failure, conversation memory and time-travel debugging. |
| **Interrupt** | Pauses a graph run to wait for outside input, then resumes it; the usual way to add human approval. |
| **LangSmith** | A platform for tracing, evaluating and monitoring LLM applications; works with or without LangChain *(verify)*. |
| **LlamaIndex** | A framework focused on connecting data to LLMs, with strong ingestion, indexing and retrieval components. |
| **CrewAI** | A framework for multi-agent systems organised around role-based agents and tasks. |
| **DSPy** | A framework that treats prompts as optimisable programs, tuning them automatically against a metric. |
