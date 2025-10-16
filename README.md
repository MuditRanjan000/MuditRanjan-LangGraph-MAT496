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