---
title: "Langchain: Tools"
categories:
  - articles
tags:
  - LLM
  - Framework
excerpt: "How to enable tools use in LangChain"
---

Tool calling is a way to enable LLM to use external tools. In OpenAI, it is called "function calling". Here, these two terms are used interchangeably. Via function calling, LLM can fetch data or take actions like sending emails. With those capabilities, LLM can be used to build rich workflow and interaction with Application UIs.

The lifecycle of a tool call is as follows:

![Prompt](/assets/images/articles/tool_call.png){: .align-center}

1. Tool Creation: Create a tool using @tool decorator to link a function with its schema. 
2. Tool Binding: Connect the tool to a compatible model. 
3. Tool Calling: Model invokes the tool with appropriate inputs. 
4. Tool Execution: Tool runs with the model-provided arguments.

The dummy code is as follows: 
```python
# Tool creation
tools = [my_tool]
# Tool binding
model_with_tools = model.bind_tools(tools)
# Tool calling 
response = model_with_tools.invoke(user_input)
```
### 1. Tool 

Tools are a way to wrap a function and its schema in a way that can be passed to a ChatModel. The interface of a tool is as follows:

The schema should have the following fields:
* `name`: The name of the tool.
* `description`: A brief description of what the tool does.
* `args`: Property that returns the JSON schema for the tool's arguments.

The key methods to execute a tool are as follows:
* `invoke`: Execute the tool with the provided arguments.

LangChain has a large number of 3rd party tools that can be used directly. The full list can be found [here](https://python.langchain.com/docs/integrations/tools/). For example, we can use the `TavilySearch` tool to search the web.


![Search Tool](/assets/images/articles/search_tool.png){: .align-center}


Langchain has a decorator `@tool` to create a tool. Via this decorator, we can simplifies the process of creating a tool.

```python
from langchain_core.tools import tool

@tool
def multiply(a: int, b: int) -> int:
    """Multiply a and b.

    Args:
        a: first int
        b: second int
    """
   return a * b
```
Some best practices to design a tool are as follows:
* Tools that are well-named, correctly-documented and properly type-hinted are easier for models to use.
* Design simple and narrowly scoped tools, as they are easier for models to use correctly.
* Use chat models that support tool-calling APIs
* Asking the model to select from a large list of tools poses challenges for the model.

### 2. Tool Binding

LangChain has a method `bind_tools` to bind a tool to a model. The method is as follows: 

```python
model_with_tools = model.bind_tools([multiply])
```

### 3. Tool Calling

In the tool calling process, the model will take two kidns of inputs: 
1. Users' query 
2. Function schema 

The model would decide which tool and when to use that tool based on the input's relevance. Than, the output would be the payload needed for the tool, i.e., the arguments to execute the tool. And the model decides when to use a tool based on the input's relevance. The model doesn't always need to call a tool. For example, given an unrelated input, the model would not call the tool. 

```python
@tool
def multiply(a: int, b: int) -> int:
    """Multiply a and b.

    Args:
        a: first int
        b: second int
    """
    return a * b

llm_with_tools = llm.bind_tools([multiply])
result = llm_with_tools.invoke("Hello world!")
print(result)
result = llm_with_tools.invoke("What is 3 times by 2?")
print(result)
print(result.tool_calls)
```
The above results are both `AIMessage` objects. The first result is the natural language response with the content. The second result is the tool call response: the content is empty but with an additional `tool_calls` field. The tool call is the payload needed for the tool, i.e., the arguments to execute the tool as:

```
[{'name': 'multiply', 'args': {'a': 3, 'b': 2}, 'id': 'call_UDe4BsVA4wnhKE2jvKmrNtMw', 'type': 'tool_call'}]
```
It has the following fields:
* `name`: The name of the tool.
* `args`: The arguments to execute the tool.
* `id`: The ID of the tool call.
* `type`: tool_call

### 4. Tool Execution

Tools could be exeucted via the `invoke` method by taking the tool call as the input. 

```python
multiply.invoke(result.tool_calls[0])
```

And we can also use the LCEL to build the chain.

```python
chain = llm_with_tools | (lambda x: x.tool_calls[0]["args"]) | multiply
chain.invoke("What is two times by 43")
```
### Pass tool outputs to the model

Now, we have the last step of passing the tool outputs to the model so that the model can use the tool outputs to generate the final response to the query.

```python
from langchain_core.messages import HumanMessage
query = "What is 3 times by five?"
messages = [HumanMessage(query)]
ai_msg = llm_with_tools.invoke(messages)
print(ai_msg.tool_calls)
messages.append(ai_msg)
for tool_call in ai_msg.tool_calls:
    selected_tool = {"multiply": multiply}[tool_call["name"].lower()]
    tool_msg = selected_tool.invoke(tool_call)
    messages.append(tool_msg)
print(messages)
llm_with_tools.invoke(messages)
```
The below is the `messages` after the tool execution:
```
[HumanMessage(content='What is 3 times by five?', additional_kwargs={}, response_metadata={}),
 AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_YqdoEgVMQ2u3LZKeH8EzEf36', 'function': {'arguments': '{"a":3,"b":5}', 'name': 'multiply'}, 'type': 'function'}], 'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 18, 'prompt_tokens': 66, 'total_tokens': 84, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-2024-08-06', 'system_fingerprint': 'fp_d28bcae782', 'finish_reason': 'tool_calls', 'logprobs': None}, id='run-b1ab307b-a5a3-4df0-b7ff-b0cc49838713-0', tool_calls=[{'name': 'multiply', 'args': {'a': 3, 'b': 5}, 'id': 'call_YqdoEgVMQ2u3LZKeH8EzEf36', 'type': 'tool_call'}], usage_metadata={'input_tokens': 66, 'output_tokens': 18, 'total_tokens': 84, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}}),
 ToolMessage(content='15', name='multiply', tool_call_id='call_YqdoEgVMQ2u3LZKeH8EzEf36')]
```

As you can see, tool_call generate a `AIMessage` object with the tool_calls field. Than, the tool_calls field is passed to the tool to generate the `ToolMessage` object. And the `id` here matched to each other. This helps the model match tool responses with tool calls.  Than, the final out would be `AIMessage` object with the content as below: 
```
AIMessage(content='3 times 5 is 15.', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 10, 'prompt_tokens': 91, 'total_tokens': 101, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-2024-08-06', 'system_fingerprint': 'fp_d28bcae782', 'finish_reason': 'stop', 'logprobs': None}, id='run-94ef30bc-c935-47f8-a495-c70cb2fed2d3-0', usage_metadata={'input_tokens': 91, 'output_tokens': 10, 'total_tokens': 101, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}})
```

The above basic flow is actually the built in those tool calling agents defined in LangChain and LangGraph.


### 5. Building a tool calling agent


## References

{% include references.html keys="langchain2024" %}