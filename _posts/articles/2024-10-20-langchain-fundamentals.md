---
title: "Langchain: Fundamentals"
categories:
  - articles
tags:
  - LLM
  - Framework
excerpt: "A fundamental view of LangChain"
---


LangChain is a OSS framework for building applications with language models. In this blog, I will provide a fundamental view of LangChain and its components. Firstly, we will look at the benefits of LangChain. Then, we will go through the conceptual design of LangChain. Than, we will check the basic components of LangChain. It would be a series of blogs. Here, we will only cover runnables, chatmodel, prompt, message. In the next blogs, we will cover the tool, retriever, outputparser, and finally, we will check the LangGraph.

### What is LangChain?

LangChain is designed to simplify the process of building applications with language models. Now, they are expanding their scope to cover the whole lifecycle of LLM applications by providing a set of building blocks. It has multiple modules with different focuses as follows:

![LangChain Framework](/assets/images/articles/lc_framework.png){: .align-center}

1. **langchain-core**: Basic abstractions and LangChain Expression Language (LCEL) for building workflow or chains.

2. **langchain-community**: Community-contributed modules for building applications with language models. such as langchain-openai, langchain-ollama

3. **langgraph**: A framework for building agentic applications with language models. It is modeling steps as edges and nodes in a graph.

4. **langsmith**: Platform to debug, test, evaluate, and monitor LLM applications.

5. **langchain**: contains chains and retrieval strategies that are generic across all integrations.

The benefits of LangChain are:

* Easy switch between LLM providers: LangChain provides a unified interface for different LLM providers. Langchain exposes a standard interface for key components such as models, prompts, tool calling, output parsers,and chains.Therefore, it is easy for developers to switch between providers. It is mainly supported by langchain-core and langchain-community.

* Easy to build applications with language models: combine multiple components and models into more complex applications, there's a growing need to efficiently connect these elements into control flows that can accomplish diverse tasks. Orchestration is crucial for building such applications. It is mainly supported by langgraph.

* Easy to track and debug: LangChain provides a platform for observability and evaluations. It is mainly supported by langsmith.

The following code is an example of how to switch between LLM providers. We are using a simple prompt to ask the LLM to answer the question about the capital of Canada. And one API LLM provider from OpenAI (API) and two local LLM providers from Ollama and HuggingFace are used.


  ```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("human", "{input}")
])
llm = ChatOpenAI(model='gpt-4o-mini')
chain = prompt | llm
result = chain.invoke({"input": "Hello, what is the capital of Canada?"})
print(result.content)

from langchain_ollama import ChatOllama
llm_qwen = ChatOllama(model="qwen2.5:3b")
chain_qwen = prompt | llm_qwen
result_qwen = chain_qwen.invoke({"input": "Hello, what is the capital of Canada?"})
print(result_qwen.content)

from langchain_huggingface import ChatHuggingFace, HuggingFacePipeline
llm_hf = HuggingFacePipeline.from_model_id(
    model_id="TinyLlama/TinyLlama-1.1B-Chat-v1.0",
    task="text-generation",
    pipeline_kwargs=dict(max_new_tokens=256, 
                         do_sample=True, 
                         temperature=0.7, 
                         top_k=50, 
                         top_p=0.95,
    ),
)
chat_model = ChatHuggingFace(llm=llm_hf)
chain_hf = prompt | chat_model.bind(skip_prompt=True)
result_hf = chain_hf.invoke({"input": "Hello, what is the capital of Canada?"})
print(result_hf.content)

  ```

There is a detailed documentation on LangChain available on their official site [here](https://python.langchain.com/docs/tutorials/) . In this blog, I will only cover high-level and important components and concepts which can help you to understand the LangChain framework and start building applications using LangChain quickly. Here, langchain-core would be covered mainly. in the next blog, I will focus more on LangGraph.


### Basic Components

#### Runnables:
LangChain's core abstraction is the Runnable interface, which provides a standard way to compose and execute language model chains. Via LCEL, runnables can be chained together using operators like  (pipe) and + (combine), allowing you to build complex workflows from simple components. They support batch processing, streaming, async execution and other advanced features. 

The Runnable interface provides five key capabilities:

| Interface | Description |
| --- | --- |
| Invoked | A single input is transformed into an output |
| Batched | Multiple inputs are efficiently transformed into outputs |
| Streamed | Outputs are streamed as they are produced |
| Inspected | Schematic information about Runnable's input, output, and configuration can be accessed |
| Composed | Multiple Runnables can be composed to work together using the LangChain Expression Language (LCEL) to create complex pipelines |

The major types of predefined runnables are as follows:

| Component | Input Type | Output Type |
| --- | --- | --- |
| Prompt | dictionary | PromptValue |
| ChatModel | a string, list of chat messages or a PromptValue | ChatMessage |
| LLM | a string, list of chat messages or a PromptValue | String |
| OutputParser | the output of an LLM or ChatModel | Depends on the parser |
| Retriever | a string | List of Documents |
| Tool | a string or dictionary, depending on the tool | Depends on the tool |

If you see in the above code snippes, we are using the prompt and chatmodel runnables. And the pipe operator is used to chain them together. In the following, we will check those componets in more details, which is also the basic building blocks of LangChain.

#### ChatModel & LLM:

ChatModel is a wrapper around an LLM which provide a standardized way to interact with modern LLMs through a message-based interface. The legacy LLM is also supported in LangChain to support the simple text-in-text-out interactions (via string).  Instead of simple text-in-text-out interactions, they work with structured messages that have specific roles (like "system", "human", or "assistant") and can contain various types of content. It is more and more common to use ChatModel for development. And even in OpenAI's API, chat model is now the chosen way to interact with modern models. 

  ```python
from langchain_openai import OpenAI
llm = OpenAI()
response = llm.invoke("What is after Monday?")
# Returns: '\n\nAfter Monday comes Tuesday.'
ChatOpenAI Example (Message-based)
from langchain_openai import ChatOpenAI
chat = ChatOpenAI()
response = chat.invoke("What is after Monday?")
# Returns: AIMessage(content='Tuesday')

  ```

As you can see in the above example, LangChain also has implementations of older LLMs that do not follow the chat model interface and instead use an interface that takes a string as input and returns a string as output. OpenAI vs ChatOpenAI refer to the difference between the legacy LLM and the chat model. Similarly, LangChain has ChatOllama and ChatAnthropic to support chatmodels.

The key features of ChatModel are summarized as below: 

1. **Multiple Provider Support**
   - Works with many providers (OpenAI, Anthropic, Ollama, Azure, etc.)
   - Consistent interface across different providers
   - Available through `langchain-<provider>` packages

2. **Standard Capabilities**
   ```python
   from langchain_openai import ChatOpenAI
   
   # Basic initialization
   chat = ChatOpenAI(
       temperature=0.7,
       max_tokens=500
   )
   ```
  with the common configuration options such as

     | Parameter | Description |
     | --- | --- |
     | `model` | Specific model identifier |
     | `temperature` | Controls response randomness (0.0-1.0) |
     | `max_tokens` | Limits response length |
     | `timeout` | Maximum wait time for responses |
     | `max_retries` | Number of retry attempts |
     | `rate_limiter` | For managing request rates |

3. **Core Functionalities**
   - `invoke`: Main method for model interaction
   - `stream`: For streaming responses
   - `batch`: Efficient processing of multiple requests
   - `bind_tools`: Tool integration
   - `with_structured_output`: Structured response formatting

4. **Advanced Features**
   - Tool calling for external service integration
   - Structured output formatting
   - Multimodal capabilities (images, audio, video)
   - Built-in monitoring and debugging through LangSmith
   - ChatModel with its supported features could be found [here](https://python.langchain.com/docs/integrations/chat/)

#### Prompt & Message:

As mentioned in the previous section, messages are the fundamental units of communication in chat models. They represent individual pieces of a conversation and have specific roles and content. While prompts are templates that help structure how we format inputs before sending them to language models. They act as a higher-level abstraction above messages.

For messages, it has two key components: role and content. The supported role types are as follows:
   - `SystemMessage`: Sets behavior/context for the AI
   - `HumanMessage`: User inputs
   - `AIMessage`: Model responses
   - `ToolMessage`: Results from tool calls

By adding the content, the message structure would be:
   ```python
   from langchain_core.messages import HumanMessage, SystemMessage
   
   messages = [
       SystemMessage(content="You are a helpful assistant"),
       HumanMessage(content="What is LangChain?")
   ]
   ```
For prompts, we can use it as below:

   ```python
   # String Template
   from langchain_core.prompts import PromptTemplate
   template = PromptTemplate.from_template("Tell me a joke about {topic}")
   
   # Chat Template
   from langchain_core.prompts import ChatPromptTemplate
   chat_template = ChatPromptTemplate([
       ("system", "You are a helpful assistant"),
       ("user", "Tell me about {topic}")
   ])
   ```
Since Prompt is one of runnables, we can use the `invoke` method to fill in our values.

![Prompt](/assets/images/articles/prompt.png){: .align-center}
You can see the difference in the format.

And from the above code, you can see that the prompt is a higher-level abstraction above messages. And the relationship between them is as follows:
   - Prompts → Messages → LLM
   - Prompts help structure and format the input
   - Messages carry the actual content to the model

Messages can help us to directly interact with ChatModels with the fine-grained control over conversion and specific conversion roles. And prompts can be beneficial for reusable templates, standardization inputs with variable contents.

In the above `ChatPromptTemplate`, we can see string is used to format the message. And we can also pass a list of messages to format the prompt. The example could be found as below: 

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.messages import HumanMessage
prompt_template = ChatPromptTemplate([
    ("system", "You are a helpful assistant"),
    MessagesPlaceholder("msgs")
])
prompt_template.invoke({"msgs": [HumanMessage(content="hi!")]})
```

The following code is an example of how to use the prompt and message in LangChain to create the few-shot prompts.

```python
from langchain_core.prompts import (
    FewShotChatMessagePromptTemplate,
    ChatPromptTemplate
)
examples = [
    {"input": "what is 2~2", "output": "4"},
    {"input": "what is 2~3", "output": "6"},
    {"input": "what is 4~9", "output": "36"},
    {"input": "what is 25~2", "output": "50"},
]
example_prompt = ChatPromptTemplate(
    [('human', '{input}'), ('ai', '{output}')]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
    examples=examples,
    # This is a prompt template used to format each individual example.
    example_prompt=example_prompt,
)
final_prompt = ChatPromptTemplate(
    [
        ('system', 'You are a helpful AI Assistant and you should use the examples to answer the question'),
        few_shot_prompt,
        ('human', '{input}'),
    ]
)
chain = final_prompt | llm
chain.invoke({"input": "what is 4~4?"})
```

#### OutputParser:

https://medium.com/@juanc.olamendy/parsing-llm-structured-outputs-in-langchain-a-comprehensive-guide-f05ffa88261f


#### Tool:

#### Retriever:

## Discussions

## References

{% include references.html keys="langchain2024" %}

