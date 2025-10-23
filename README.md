### MODULE 1

### Lesson 1: Motivation
* **What I Learned:** This was an introductory lesson covering the core motivation for LangGraph. It explained why managing state and creating cycles (loops) is crucial for building complex agents, and how LangGraph is designed to solve these challenges where standard chains might fall short.
* **My Code Tweak:** No coding was done in this lesson as it was purely conceptual.
* **Source File:** All my work will be saved in the `my_learnings` directory, following the module structure.

### Lesson 2: Simple Graph
* **What I Learned:** I learned how to build a basic graph with a single step. This involved defining a `StateGraph`, creating a state dictionary with `TypedDict`, adding a Python function as a node to perform an action (calling an LLM), and using `START` and `END` to define the graph's flow.
* **My Code Tweak:** I created a `lesson_2.ipynb` notebook and implemented a simple graph that takes a user's question, calls GPT-4o to get an answer, and stores the result in the graph's state. I successfully compiled and ran the graph to get a response.
* **Source File:** [lesson_2.ipynb](my_learnings/module_1/lesson_2.ipynb)

### Lesson 3: LangSmith Studio
* **What I Learned:** I learned how to use LangSmith Studio to visualize and debug my local LangGraph applications. This involved starting the `langgraph dev` server and connecting to it through the Studio UI in the browser, which provides a real-time, visual trace of the graph's execution.
* **My Code Tweak:** This was a UI-based lesson. I started the local development server and used the Studio to run and inspect the simple graph I built in the previous lesson.
* **Source File:** [lesson_2.ipynb](my_learnings/module_1/lesson_2.ipynb)

### Lesson 4: Chain
* **What I Learned:** I learned how to create a multi-step chain in LangGraph by adding multiple nodes and connecting them in a sequence. This involved updating the `GraphState` to hold intermediate values and using `workflow.add_edge()` to define the data flow from one node to the next.
* **My Code Tweak:** I built a two-step graph. The first node ("rewriter") rephrases the user's question for clarity, and the second node ("llm") answers the rewritten question. This demonstrates a simple sequential chain where the output of one step is used as the input for the next.
* **Source File:** [lesson_4.ipynb](my_learnings/module_1/lesson_4.ipynb)

### Lesson 5: Router
* **What I Learned:** I learned how to add conditional logic to a graph using a router. This involved creating a conditional edge with `add_conditional_edges()`, which calls a function to inspect the graph's state and decide which node to execute next. This is the key to building agents that can use tools and create cycles.
* **My Code Tweak:** I built a simple agent that can use a `multiply` tool. My graph has three main parts: an LLM node that can either respond or use a tool, a tool execution node, and a router function that checks if a tool was called. I also learned to wrap tool outputs in a `ToolMessage` to correctly pass the results back into the graph's state.
* **Source File:** [lesson_5.ipynb](my_learnings/module_1/lesson_5.ipynb)

### Lesson 6: Agent
* **What I Learned:** I learned how to combine multiple tools, nodes, and conditional routing to build a complete agent. The agent can now decide whether to call a tool (and which one to call) or respond to the user directly, looping back to the LLM after a tool is used to generate a final, user-facing answer.
* **My Code Tweak:** I built an agent with two tools: a `multiply` tool for math and a `search_tavily` tool for web searches. The graph's router checks for tool calls in the LLM's response and directs the flow accordingly. I tested the agent with two different inputs to confirm that it correctly used a tool for one question and answered directly for the other.
* **Source File:** [lesson_6.ipynb](my_learnings/module_1/lesson_6.ipynb)

### Lesson 7: Agent with Memory
* **What I Learned:** I learned the concept of adding memory to a LangGraph agent by using a checkpointer, which allows the graph's state to be persisted. The goal was to use a `SqliteSaver` and a `thread_id` to enable a multi-turn conversation where the agent remembers previous interactions.
* **My Code Tweak & Issue:** I attempted to implement an agent with memory using `SqliteSaver`. However, the code failed with a `ModuleNotFoundError: No module named 'langgraph.checkpoint.sqlite'`, which indicates that the installed version of the `langgraph` library is outdated and incompatible with the course material.
* **Source File:** [lesson_7.ipynb](my_learnings/module_1/lesson_7.ipynb)

---------------------------------------------------------------------------------------------------------------------------------------

### MODULE 2

### Lesson 1: State Schema
* **What I Learned:** I learned that the `StateGraph` requires a clearly defined state schema, which I created using Python's `TypedDict`. This schema is the central data structure that gets passed between all the nodes in the graph.
* **My Code Tweak:** I created a `GraphState` with `question`, `documents`, and `answer` fields. I then built a two-step RAG chain where the first node (`retriever`) filled the `documents` field, and the second node (`generator`) used that data to fill the `answer` field. This showed how the state is updated as it moves through the graph.
* **Source File:** [lesson_1.ipynb](my_learnings/module_2/lesson_1.ipynb)

### Lesson 2: State Reducers
* **What I Learned:** I learned how to use state reducers to manage how the graph's state is updated. By using `Annotated` with an operator like `operator.add`, I can make the state accumulate values (like appending messages to a list) rather than just overwriting them.
* **My Code Tweak:** I defined the `messages` field in my `GraphState` as a reducer. I then built a simple graph that I invoked twice in a row, passing the output state of the first run as the input to the second. This successfully demonstrated how the message list grew with each run, proving the reducer was working.
* **Source File:** [lesson_2.ipynb](my_learnings/module_2/lesson_2.ipynb)

### Lesson 3: Multiple Schemas
* **What I Learned:** I learned how to create a graph that can handle different types of tasks by using multiple state schemas and a router. By defining a `Union` of different `TypedDict` schemas, the graph's state can adapt based on the input.
* **My Code Tweak:** I built a graph that can either solve simple math problems or answer general questions. I created two states, `MathState` and `GeneralState`, and a router function that checks the user's question for math operators. The graph successfully routed to the correct node based on the input, demonstrating how to handle multiple schemas.
* **Source File:** [lesson_3.ipynb](my_learnings/module_2/lesson_3.ipynb)