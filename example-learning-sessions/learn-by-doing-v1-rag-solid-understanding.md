# Learning Session: Retrieval-Augmented Generation (RAG)

- **Skill:** `learn-by-doing`
- **Skill version:** `614697b` (Git revision; the skill does not declare a semantic version)
- **Topic:** Retrieval-Augmented Generation (RAG)
- **Chosen depth:** Solid understanding
- **Roles:** User and Teaching Model

---

**User:**

Use the learn-by-doing skill in the folder to teach me about RAG.

**Teaching Model:**

How deep would you like to go?

- **Quick overview** — the big idea in a few minutes
- **Solid understanding** — core concepts and how they fit together
- **Deep dive** — architecture, tradeoffs, edge cases, and reasoning

**User:**

Solid understanding

**Teaching Model:**

Imagine asking a librarian a question.

Without RAG, the librarian answers only from memory. Their knowledge may be incomplete or outdated.

With RAG, the librarian:

1. Searches the library for relevant passages.
2. Reads those passages.
3. Uses them to compose an answer.

That is **Retrieval-Augmented Generation**:

- **Retrieval** finds relevant information from an external knowledge source.
- **Augmented** means that information is added to the model’s prompt.
- **Generation** is the model producing an answer using both the retrieved information and its existing abilities.

In your own words, why might this be more reliable than asking the model to answer from memory alone?

**User:**

Because it allows the model to use knowledge outside of its training data.

**Teaching Model:**

Exactly. It can use private, specialized, or recently updated knowledge without retraining.

A RAG system has two phases:

**Preparation**

Documents are:

1. Collected
2. Split into smaller passages called **chunks**
3. Converted into searchable representations
4. Stored in an index

**Answering**

When a user asks a question:

1. Search the index for relevant chunks.
2. Place the best chunks into the model’s prompt.
3. Ask the model to answer using that context.

The key separation is: preparation makes knowledge searchable; answering retrieves and uses it.

Why do you think documents are split into chunks instead of storing each entire document as one searchable item?

**User:**

This increases search efficiency by allowing the model to be use only the relevant information. Also it might provide a more relevant answer.

**Teaching Model:**

Right. Smaller chunks improve retrieval precision and keep irrelevant text out of the model’s limited prompt space.

There is a tradeoff:

- **Chunks too large:** contain extra noise and consume more prompt space.
- **Chunks too small:** may lose the surrounding context needed to understand them.

Next comes **semantic search**. Each chunk is converted into an **embedding**: a list of numbers representing its meaning. The question is converted the same way. A retriever compares these representations and selects chunks whose meanings are closest.

This can match ideas even when the exact words differ. For the query **“How can I reset my password?”**, which passage should semantic search rank higher?

A. “Instructions for changing your account credentials”

B. “How we store passwords securely”

**User:**

A.

**Teaching Model:**

Correct. The wording differs, but the meaning is close.

The retriever usually returns the **top-k** chunks—the *k* most similar passages. For example, with `k = 4`, it supplies the four best matches.

Choosing `k` creates another tradeoff:

- Too few chunks: important evidence may be missed.
- Too many chunks: irrelevant or conflicting material can distract the model and increase cost.

Those chunks are then inserted into a prompt resembling:

```text
Use the following context to answer the question.
If the context does not contain the answer, say you do not know.

Context:
[retrieved chunks]

Question:
[user's question]
```

This grounding instruction matters because retrieval does not guarantee that the answer is present.

What should the model do if the retrieved chunks do not actually contain the answer?

**User:**

The model should admit it doesn't have the answer.

**Teaching Model:**

Exactly. A well-designed RAG system should abstain instead of inventing an unsupported answer. This reduces hallucinations, although RAG cannot eliminate them.

RAG can fail in two main places:

- **Retrieval failure:** the system fetches the wrong chunks or misses the right one.
- **Generation failure:** the correct evidence is retrieved, but the model ignores or misinterprets it.

Suppose the needed answer exists in the document collection, but none of the retrieved chunks contains it. Is that a retrieval failure or a generation failure?

**User:**

That is a retrieval failure.

**Teaching Model:**

Correct. The generator never received the evidence, so it had no fair chance to answer correctly.

Retrieval is commonly improved with:

- Better chunk sizes and boundaries
- **Hybrid search**, combining semantic similarity with exact keyword matching
- **Metadata filters**, such as product, date, or department
- A **reranker**, which carefully reorders an initial set of candidates

A typical system might retrieve 20 chunks quickly, rerank them more accurately, then send only the best 4 to the model. This balances speed, precision, and prompt size.

Why might retrieving 20 and reranking down to 4 work better than retrieving only 4 initially?

**User:**

Retrieving 20 will increase the body of information that is available increasing the likelihood the sought answer is present. Then narrowing down to the actual information that is used to answer is use to provide accurate, relevant and possibly more efficient answers

**Teaching Model:**

Exactly. The first retrieval stage prioritizes **recall**—making sure the useful evidence is somewhere in the candidate set. The reranker then prioritizes **precision**—keeping only the most relevant evidence.

This also shows why RAG must be evaluated in layers:

- **Retrieval quality:** Did the system find the necessary evidence?
- **Answer correctness:** Did it reach the right conclusion?
- **Faithfulness:** Is every claim supported by the retrieved evidence?
- **Answer relevance:** Did it directly address the question?
- **Performance:** Was the response fast and reasonably priced?

An answer can be factually correct but still unfaithful if it was not supported by the supplied chunks—the model may merely have remembered or guessed it.

If the correct chunk was retrieved, but the answer makes an additional unsupported claim, which evaluation dimension failed?

**User:**

I think faithfulness.

**Teaching Model:**

Yes—**faithfulness** failed because the extra claim was not grounded in the retrieved evidence.

Now assemble the pieces yourself: explain the complete RAG process, starting with source documents and ending with the answer. Include chunking, embeddings, retrieval, reranking, prompt augmentation, and what the model should do when the evidence is insufficient.

**User:**

Okay, just to let you know I won't reread the above conversation but will RAG out the answer.
RAG stands for Retrival Augmented Generation. Retrieval part stands for retrieving information relevant to the user query. This part is done from a documented body of information which is broken down in chunks ie. relevant information pieces. Then in the Augmentation part of the process from the preselected relevant chunks, a shortlist of most appropriate pieces are selected and interpreted by the (AI/LLM) model. Next the model evaluates how truthful the candidate for an answer is based on the query - whether it covers the topic in question, does it provide a full and exhaustive answer and whether it complies with cost and time constraints of the RAG configuration. Based on that the final answer of the model must be either to provide the answer it extracts from the document body or to admit that it cannot confidently answer based on the available information or the constraints.

**Teaching Model:**

Your overall model is strong. You correctly captured retrieval from an external document collection, selecting relevant evidence, and abstaining when evidence is insufficient.

Three refinements:

- It’s spelled **Retrieval-Augmented Generation**.
- Selecting and reranking chunks belongs to **retrieval**, not augmentation.
- **Augmentation** simply means inserting the selected chunks into the model’s prompt as context. The model then **generates** a grounded answer.

Evaluation measures such as faithfulness, relevance, cost, and latency are normally used to test and improve the RAG system; the model does not necessarily evaluate all of them while answering.

A compact pipeline is:

```text
Documents → chunks → embeddings/index
User query → retrieve candidates → rerank
Selected chunks → augment prompt → generate grounded answer or abstain
```

One final check: in this pipeline, what exactly is being augmented?

**User:**

The selected chunks of documented information are being inserted into model's prompt.

**Teaching Model:**

Exactly—the **model’s prompt** is augmented with the selected chunks. The model itself is not retrained or permanently updated.

You now have the core RAG mental model:

```text
Index knowledge → retrieve evidence → augment prompt → generate grounded answer
```

You also correctly understand its main limitation: good generation cannot compensate for missing or irrelevant retrieval.

**User:**

So basically the user prompt is augmented with the available information so that the answer is relevant to the desired outcome of the system and on topic based on the externally defined work the user isn't necessarily well versed with. The responsibility of that relevance is outsourced to the RAG system and its designer.

**Teaching Model:**

Yes—with one refinement: the **complete model prompt**, not merely the user’s original wording, is augmented with retrieved evidence.

Your “outsourced relevance” framing is useful. The RAG designer is largely responsible for:

- Choosing and maintaining authoritative sources
- Chunking and indexing them well
- Retrieving and reranking relevant evidence
- Instructing the model how to use or reject that evidence
- Evaluating retrieval quality and answer faithfulness

The user can therefore ask in ordinary language without knowing the underlying documents. However, responsibility is shared: ambiguous questions, poor source material, weak retrieval, or an unfaithful generator can still produce bad results.

A concise formulation would be:

> RAG delegates evidence selection to a designed retrieval system, then gives that evidence to an LLM so it can produce an answer grounded in the system’s chosen knowledge base.

**User:**

What's the difference between a "complete model prompt" and "the user's original wording"?

**Teaching Model:**

The **user’s original wording** is only the message you type:

> “How do I reset my company password?”

The **complete model prompt** is the full input assembled by the application and sent to the model. It might conceptually look like this:

```text
System instructions:
You are a company support assistant.
Answer only from the supplied documentation.

Retrieved information:
Employees can reset passwords at account.example.com/reset.
A security code will be sent to their registered phone.

Conversation history:
[earlier messages]

User question:
How do I reset my company password?

Output instructions:
Answer concisely and cite the supporting document.
```

So RAG normally leaves the user’s wording intact and adds retrieved chunks elsewhere in the model’s input.

```text
Complete model input
├── System/application instructions
├── Retrieved RAG context
├── Conversation history
├── User’s original wording
└── Formatting or tool instructions
```

In casual conversation, people often call the user’s message “the prompt.” Technically, however, the model may receive a much larger assembled prompt or **context**.
