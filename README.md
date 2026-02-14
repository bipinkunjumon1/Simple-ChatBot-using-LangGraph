```markdown
# Simple Chatbot using LangGraph

A Python-based simple chatbot built using LangGraph and Gemini AI. This project demonstrates how to structure a conversational AI workflow as a stateful graph with message history persistence, allowing for multi-turn interactions.

## Project Overview

This project utilizes langgraph to create a directed graph that:

1. Initializes State: Accepts a list of messages.

2. Chat Node: Invokes the LLM with the current messages to generate a response.

3. Persists State: Uses MemorySaver for checkpointing to maintain conversation history across invocations.

## Features

* State Management: Uses TypedDict with annotated list for accumulating messages.

* Conversational Memory: Employs MemorySaver to checkpoint state, enabling threaded conversations.

* Modular Logic: Single node for LLM invocation.

* Interactive Mode: Supports a loop for real-time user interaction.

* Graph Visualization: Includes code to render the graph structure using Mermaid.

## Technical Stack

* Language: Python

* Orchestration: LangGraph

* AI Model: LangChain Google GenAI (Gemini)

* Data Handling: typing.TypedDict, langchain_core.messages

* Environment: python-dotenv

* Checkpointing: MemorySaver

## Installation

1. Clone the repository:

   ```
   git clone https://github.com/your-username/chatbot-langgraph.git
   cd chatbot-langgraph
   ```

2. Install dependencies:

   ```
   pip install langgraph langchain-google-genai python-dotenv
   ```

3. Set up your Google API key in a `.env` file:

   ```
   GOOGLE_API_KEY=your_api_key_here
   ```

## How It Works

### The State

The chatstate keeps track of the conversation messages:

```python
from langgraph.graph.message import add_messages

class chatstate(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
```

### The Graph Logic

The workflow is a simple linear path:

* START → chat_node → END

With checkpointing enabled via MemorySaver.

## Usage

Run the script to start an interactive chat session:

```python
thread_id = '1'
while True:
    user_message = input("Type Here:")
    print('User:', user_message)

    if user_message.strip().lower() in ['exit', 'quit', 'bye']:
        break
    config = {'configurable': {'thread_id': thread_id}}
    response = workflow.invoke({'messages': [HumanMessage(content=user_message)]}, config=config)

    print('AI:', response['messages'][-1].content)
```

Example interaction:

```
User: hi
AI: Hi there! How can I help you today?
User: my name is arshin
AI: Nice to meet you, Arshin! It's a pleasure to connect with you. How can I assist you today?
User: who is the prime of india
AI: The current Prime Minister of India is **Narendra Modi**.
User: end
AI: Okay, Arshin. If you have any more questions in the future, feel free to ask! Have a great day.
```

## Visualization

The project includes a utility to display the graph structure:

```python
from IPython.display import Image
Image(workflow.get_graph().draw_mermaid_png())
```
```
