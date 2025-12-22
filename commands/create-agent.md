---
name: create-agent
description: Scaffold a new DeepAgent with LangChain/LangGraph integration
---

# Create Agent Command

Create a new agent with the specified configuration.

## Usage

When asked to create an agent, I will:

1. **Gather Requirements**
   - What is the agent's purpose?
   - What tools does it need?
   - Which model should it use?
   - Does it need persistence/memory?

2. **Generate Agent Code**

### Basic Agent Template
```python
from deepagents import create_deep_agent
from langchain_core.tools import tool

# Define tools
@tool
def my_tool(input: str) -> str:
    """Description of what this tool does."""
    # Implementation
    return result

# Create agent
agent = create_deep_agent(
    model="gpt-4o",  # or "claude-sonnet-4-20250514"
    tools=[my_tool],
    system_prompt="You are a helpful assistant specialized in..."
)

# Use the agent
if __name__ == "__main__":
    response = agent.invoke({
        "messages": [("human", "Your query here")]
    })
    print(response)
```

### Agent with LangGraph State Management
```python
from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langgraph.checkpoint.memory import MemorySaver
from deepagents import create_deep_agent

class AgentState(TypedDict):
    messages: Annotated[list, add_messages]

# Create base agent
base_agent = create_deep_agent(
    model="gpt-4o",
    tools=[...],
    system_prompt="..."
)

def agent_node(state: AgentState):
    result = base_agent.invoke({"messages": state["messages"]})
    return {"messages": result["messages"]}

# Build graph
workflow = StateGraph(AgentState)
workflow.add_node("agent", agent_node)
workflow.add_edge(START, "agent")
workflow.add_edge("agent", END)

# Compile with memory
memory = MemorySaver()
app = workflow.compile(checkpointer=memory)

# Use with session
config = {"configurable": {"thread_id": "session-1"}}
response = app.invoke({"messages": [("human", "Hello")]}, config)
```

3. **Add Tests** (if requested)
4. **Update dependencies** in pyproject.toml if needed
