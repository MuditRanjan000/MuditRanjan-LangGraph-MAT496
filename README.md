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

### Lesson 4: Trim and Filter Messages
* **What I Learned:** I learned how to manage long conversation histories by trimming the messages passed to a node. This is important for preventing context window overflow. I used the `add_messages` reducer to manage the message state and applied a transformation in the `.invoke()` call's `config` to send only a slice of the full message history to the LLM.
* **My Code Tweak:** I created a graph that accumulates messages. In the final `.invoke()` call, I passed a configuration that used a lambda function (`lambda x: x[-2:]`) to trim the history, ensuring only the last two messages were sent to the LLM node.
* **Source File:** [lesson_4.ipynb](my_learnings/module_2/lesson_4.ipynb)

### Lesson 5: Chatbot with Summarization and Memory
* **What I Learned:** I learned the concept of building a stateful chatbot that manages a long conversation history by summarizing it. This involved using a checkpointer for memory and a conditional router that triggers a summarization node when the conversation exceeds a certain length.
* **My Code Tweak & Issue:** I attempted to build the summarization chatbot. However, running the code locally resulted in a persistent "Kernel Died" error. This confirms the local environment is unstable and unable to run the course notebooks for this module.
* **Source File:** [lesson_5.ipynb](my_learnings/module_2/lesson_5.ipynb)

### Lesson 6: Chatbot with Summarization and External Memory
* **What I Learned:** I learned how to build a chatbot that manages its conversation history in an external store, separate from the graph's state. I also learned how to combine this with a summarization step, where a router checks the length of the external memory and triggers a node to summarize the history and update the external store.
* **My Code Tweak:** I implemented a graph where the state only holds the current input and a session ID. The nodes are responsible for fetching from, and saving to, an external (simulated) memory store. A router function checks the length of this external history and, when it exceeds a threshold, calls a summarization node that replaces the long history with a concise summary.
* **Source File:** [lesson_6.ipynb](my_learnings/module_2/lesson_6.ipynb)

---------------------------------------------------------------------------------------------------------------------------------------

### MODULE 3

### Lesson 1: Streaming
* **What I Learned:** I learned how to stream intermediate outputs from a LangGraph graph as they are generated. This involves using the `.stream()` method on the compiled graph and processing the events yielded by the stream. I also saw how using `.stream()` within a node itself (like on an LLM call) allows for token-by-token streaming.
* **My Code Tweak:** I created a simple graph with one node that calls `llm.stream()`. I then used `app.stream(..., stream_mode="updates")` to iterate through the output chunks generated by the LLM node and print them to the console in real-time, simulating a streaming response.
* **Source File:** [lesson_1.ipynb](my_learnings/module_3/lesson_1.ipynb)

### Lesson 2: Breakpoints
* **What I Learned:** I learned how to use breakpoints to pause and resume graph execution. This is done by specifying nodes where interrupts can occur when compiling the graph (`interrupt_before` or `interrupt_after`). The graph's state is saved at the breakpoint using the checkpointer, and execution can be resumed later by invoking the graph again with the same configuration (thread ID).
* **My Code Tweak:** I created a simple graph with an LLM node followed by a node that pauses for a few seconds. I compiled the graph with `interrupt_before=["pause"]`. I invoked the graph once, which ran the LLM and paused. I then invoked it again with `None` as input but the same config, which successfully resumed execution from the breakpoint and completed the graph.
* **Source File:** [lesson_2.ipynb](my_learnings/module_3/lesson_2.ipynb)

### Lesson 3: Editing State and Human Feedback
* **What I Learned:** I learned how to modify the state of a graph while it is paused at a breakpoint. This involves using `app.get_state(config)` to retrieve the current state, making changes to the state dictionary, and then using `app.update_state(config, new_state)` to save the modifications back to the checkpointer before resuming execution.
* **My Code Tweak:** I created a graph that generates a poem and then pauses for approval. After the pause, I simulated human feedback by getting the current state, adding a new `HumanMessage` with a correction to the `messages` list, and updating the state using `app.update_state()`. Resuming the graph finished the execution with the modified state, demonstrating how human-in-the-loop corrections can be incorporated.
* **Source File:** [lesson_3.ipynb](my_learnings/module_3/lesson_3.ipynb)

### Lesson 4: Dynamic Breakpoints
* **What I Learned:** I learned how to implement dynamic breakpoints, where the graph only pauses if a specific condition is met during execution. This involves adding a flag or condition to the graph's state, using a conditional edge (router) to check that condition, and only routing to a node marked for interruption if the condition is true.
* **My Code Tweak:** I created a graph where an LLM call sets a `needs_approval` flag in the state based on the response content. A router node checks this flag. If `True`, it routes to a `human_approval` node which is configured to interrupt *after* execution. If `False`, the router routes directly to `END`. I tested both paths, confirming the graph paused only when the flag was set. 
* **Source File:** [lesson_4.ipynb](my_learnings/module_3/lesson_4.ipynb)

### Lesson 5: Chatbot with Summarization and Memory
* **What I Learned:** I learned the concept of building a stateful chatbot that manages a long conversation history by summarizing it. This involved using a checkpointer for memory and a conditional router that triggers a summarization node when the conversation exceeds a certain length.
* **My Code Tweak & Issue:** I attempted to build the summarization chatbot. However, running the code locally resulted in a persistent `AttributeError: 'GeneratorContextManager' object has no attribute 'get_next_version'`. This error indicates that the installed version of `langgraph` is incompatible with the checkpointer features used in the lesson.
* **Source File:** [lesson_5.ipynb](my_learnings/module_2/lesson_5.ipynb)

----------------------------------------------------------------------------------------------------------------------------------

### MODULE 4

### Lesson 1: Parallelization
* **What I Learned:** I learned how to run multiple nodes in a LangGraph graph concurrently (in parallel). This is achieved by adding multiple edges originating from the same starting point (like `START` or another node). LangGraph automatically waits for all parallel branches to complete before proceeding. Wrapping the node functions in `RunnableLambda` is helpful for this pattern.
* **My Code Tweak:** I built a graph where two different "expert" LLM nodes answered the same question simultaneously, one concisely and one with more detail. The graph successfully executed both branches in parallel and returned the results from both experts.
* **Source File:** [lesson_1.ipynb](my_learnings/module_4/lesson_1.ipynb)

