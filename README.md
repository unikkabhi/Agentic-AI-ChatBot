# 🤖 Basic Agentic Chatbot using LangGraph + Groq

This project demonstrates how to build a **tool-enabled agentic chatbot** using **LangGraph**, **LangChain**, and the **Groq LLM API**.  

It starts with a simple conversational chatbot and extends it into a **tool-calling autonomous agent** using conditional graph routing.

---

# 📌 Overview

This notebook walks through:

1. Building a basic LLM chatbot with LangGraph
2. Managing conversation state
3. Streaming responses
4. Integrating external tools (Search API + custom function)
5. Enabling conditional tool-calling logic
6. Creating a structured agent workflow

The project is ideal for understanding **agent orchestration** and how modern LLM systems move beyond simple text generation.

---

# 🏗️ Architecture

## Basic Chat Flow


START → Chatbot Node → END


## Tool-Enabled Agent Flow


START → LLM Node
↓
(Conditional Routing)
↳ Tool Node → END
↳ Direct Response → END


The agent decides dynamically whether:
- It can answer directly  
- It needs to call a tool  

---

# 🛠️ Tech Stack

- Python 3.10+
- LangGraph
- LangChain
- Groq API (Llama 3.1 model)
- Tavily Search API
- python-dotenv

---

# ⚙️ Installation

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/your-username/basic-agentic-chatbot.git
cd basic-agentic-chatbot
2️⃣ Create Virtual Environment
python -m venv .venv
source .venv/bin/activate   # Mac/Linux
.venv\Scripts\activate      # Windows
3️⃣ Install Dependencies
pip install -r requirements.txt

If requirements.txt is not included:

pip install langchain langgraph langchain-groq langchain-tavily python-dotenv
🔑 Environment Variables

Create a .env file in the root directory:

GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
🧠 Step-by-Step Implementation
1️⃣ Basic Chatbot Setup
from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langchain_groq import ChatGroq
from dotenv import load_dotenv
import os

load_dotenv()

class State(TypedDict):
    messages: Annotated[list, add_messages]

llm = ChatGroq(model="llama-3.1-8b-instant")

def chatbot(state: State):
    return {"messages": [llm.invoke(state["messages"])]}

builder = StateGraph(State)
builder.add_node("chatbot", chatbot)
builder.add_edge(START, "chatbot")
builder.add_edge("chatbot", END)

graph = builder.compile()

response = graph.invoke({"messages": ["Hi"]})
print(response)
2️⃣ Streaming Responses
for event in graph.stream({"messages": ["Hi, how are you?"]}):
    print(event)
3️⃣ Adding Tools
Tavily Search Tool
from langchain_tavily import TavilySearch

search_tool = TavilySearch(max_results=3)
Custom Multiply Tool
def multiply(a: int, b: int) -> int:
    return a * b
4️⃣ Bind Tools to LLM
tools = [search_tool, multiply]
llm_with_tools = llm.bind_tools(tools)
5️⃣ Tool-Calling Agent Node
from langgraph.prebuilt import ToolNode, tools_condition

def tool_calling_llm(state: State):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

builder = StateGraph(State)

builder.add_node("tool_calling_llm", tool_calling_llm)
builder.add_node("tools", ToolNode(tools))

builder.add_edge(START, "tool_calling_llm")

builder.add_conditional_edges(
    "tool_calling_llm",
    tools_condition
)

builder.add_edge("tools", END)

graph = builder.compile()
🚀 How It Works

User sends a message.

LLM processes the message.

If the LLM decides a tool is required:

The request is routed to the ToolNode.

Tool executes.

Response is returned.

This pattern enables agent-like behavior.

🎯 Use Cases

Building autonomous AI agents

Tool-integrated chatbots

CRM automation agents

Search-augmented generation

Learning agent orchestration

🔮 Future Improvements

Add memory persistence

Add multi-agent collaboration

Integrate database tools

Add frontend (Streamlit / FastAPI)

Deploy via Docker

📂 Project Structure
basicchatbot.ipynb
.env
requirements.txt
README.md
