---
title: "Langchain: Retriever"
categories:
  - articles
tags:
  - LLM
  - Framework
excerpt: "Query and retrieve documents"
---

Retrieval systems are one of key components in LLM applications. They are used to retrieve relevant information from a large external data sources. The data sources can be database or unstructured data like documents. And the system usually have two parts: 

1. Query analysis: A process where the users query is analyzed and transformed into a structured query optimized for the following retrieval.
2. Information retrieval: The structured query is then used to retrieve the most relevant information from the data sources.


## Query Analysis

Query analysis is the bridge connecting the user query in natural language to the retrieval system. It has two main operations: 

1. Query Re-writing:   Queries can be rew-written or expanded for the followign retrieval
2. Query Construction:  Search indexes may require structured queries (e.g., SQL for databases).

![query_analysis](/assets/images/articles/query_analysis.png){: .align-center}

### Query re-writting

Query re-writing is a process where the user query is transformed into a more effective search queries. It is still natural language to natural language. As described in the [LangChain documentation](https://python.langchain.com/docs/concepts/retrieval/), it have the following approaches: 

| Name | When to use | Description |
|------|------------|-------------|
| [Multi-query](https://python.langchain.com/docs/how_to/MultiQueryRetriever/) | When you want to ensure high recall in retrieval by providing multiple phrasings of a question. | Rewrite the user question with multiple phrasings, retrieve documents for each rewritten question, return the unique documents for all queries. |
| [Decomposition](https://github.com/langchain-ai/rag-from-scratch/blob/main/rag_from_scratch_5_to_9.ipynb) | When a question can be broken down into smaller subproblems. | Decompose a question into a set of subproblems / questions, which can either be solved sequentially (use the answer from first + retrieval to answer the second) or in parallel (consolidate each answer into final answer). |
| [Step-back](https://python.langchain.com/docs/how_to/StepBackRetriever/) | When a higher-level conceptual understanding is required. | First prompt the LLM to ask a generic step-back question about higher-level concepts or principles, and retrieve relevant facts about them. Use this grounding to help answer the user question. Paper. |
| [HyDE](https://github.com/langchain-ai/rag-from-scratch/blob/main/rag_from_scratch_5_to_9.ipynb) | If you have challenges retrieving relevant documents using the raw user inputs. | Use an LLM to convert questions into hypothetical documents that answer the question. Use the embedded hypothetical documents to retrieve real documents with the premise that doc-doc similarity search can produce more relevant matches.|

Usually, this transformation is driven by an LLM. For example, to acheive decompositon, With prompting and a structured output, the LLM can generate a set of queries from a single raw query.

```python
from pydantic import BaseModel, Field
from langchain_openai import ChatOpenAI
from langchain_core.messages import SystemMessage, HumanMessage

# Define a pydantic model to enforce the output structure
class Questions(BaseModel):
    questions: List[str] = Field(
        description="A list of sub-questions related to the input query."
    )

# Create an instance of the model and enforce the output structure
model = ChatOpenAI(model="gpt-4o", temperature=0) 
structured_model = model.with_structured_output(Questions)

# Define the system prompt
system = """You are a helpful assistant that generates multiple sub-questions related to an input question. \n
The goal is to break down the input into a set of sub-problems / sub-questions that can be answers in isolation. \n"""

# Pass the question to the model
question = """What are the main components of an LLM-powered autonomous agent system?"""
questions = structured_model.invoke([SystemMessage(content=system)]+[HumanMessage(content=question)])
```

In the above example, the output is a list of sub-questions as:

```
Questions(questions=['What is an LLM and how does it function within an autonomous agent system?', 'What are the key components of an autonomous agent system?', 'How does an LLM integrate with other components in an autonomous agent system?', 'What role does natural language processing play in an LLM-powered autonomous agent system?', 'What are the hardware requirements for running an LLM-powered autonomous agent system?', 'How do LLMs handle decision-making processes in autonomous agent systems?', 'What are the challenges in developing an LLM-powered autonomous agent system?', 'How is data input and output managed in an LLM-powered autonomous agent system?', 'What are the security considerations for an LLM-powered autonomous agent system?']
```

### Query Construction

Different from the query analysis which is a natural language to natural language process, the query construction is a natural language to a more specialized query language or filters. It can have the following examples:

1. Structed data: [Text to SQL](https://python.langchain.com/docs/tutorials/sql_qa/) for Relational database or [Cypher](https://python.langchain.com/docs/tutorials/graph/) for Graph database
2. [Semi-structured data](https://python.langchain.com/docs/how_to/self_query/): convert natural language to metadata filters. Than, the metadata filters are used to combine with semantice search.

## Information retrieval

It depends on the query. Four major kinds of retrieval systems are:

1. Keyword search
2. Vector search
3. Graph database search
4. Relational database search



## References

{% include references.html keys="langchain2024" %}

