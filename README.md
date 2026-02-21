# Agentic-AI-ChatBot
Basic Agentic Chatbot using LangGraph + Groq  This project demonstrates a simple yet extensible agentic chatbot built using LangGraph, LangChain, and the Groq LLM API. It showcases how to move from a basic conversational LLM setup to a tool-enabled autonomous agent with conditional routing.
🔹 Features

LLM-Powered Chatbot
Uses ChatGroq with the llama-3.1-8b-instant model for fast and efficient conversational responses.

State Management with LangGraph
Implements a StateGraph architecture where messages are stored and updated using annotated state definitions.

Streaming Support
Supports real-time streaming of responses for interactive chat experiences.

Tool Integration (Agentic Behavior)
Demonstrates how to bind tools to the LLM and enable dynamic tool calling:

Tavily Search API for web search

Custom-defined Python function (e.g., multiply tool)

Conditional Routing
Uses add_conditional_edges() to intelligently decide whether the LLM should:

Respond directly

Call a tool

Route execution through a tool node

This makes the chatbot capable of performing actions beyond simple text generation.

🏗️ Architecture

The workflow follows:

START → LLM Node → (Conditional)
                    ↳ Tool Node → END
                    ↳ Direct Response → END

This modular structure allows easy extension into more complex multi-agent or multi-tool systems.

🛠️ Tech Stack

Python

LangGraph

LangChain

Groq API

Tavily Search API

dotenv for environment management

🚀 Use Cases

Building autonomous AI agents

Tool-enabled conversational assistants

Experimenting with agent orchestration

Learning LangGraph workflows

This project serves as a foundational template for building scalable, tool-augmented AI agents that can be extended into production-grade systems.
