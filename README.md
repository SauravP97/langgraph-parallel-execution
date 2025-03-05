# Parallel execution of nodes in LangGraph
## Enhancing the performance of the Graph workflows

This repo is a POC of the parallel execution of nodes in **LangChain's** LangGraph framework. We run two flavours of our Graph workflow, one **serial** and one **parallel** and compare their run-time performance.

Parallel execution of nodes is essential to speed up overall graph operation. **LangGraph** offers native support for parallel execution of nodes, which can significantly enhance the performance of graph-based workflows. This parallelization is achieved through **fan-out** and **fan-in** mechanisms, utilizing both standard edges and conditional_edges.

![Intro pic](/media/into-pic.png)

## Setting up the Graph workflow

We have 4 nodes in our workflow.

1. **create_model**: Creates an LLM Model shared by rest of the nodes.
2. **get_dsa_topics**: Returns top 5 Data Structures crucial for cracking top Product Based companies.
2. **get_system_design_topics**: Returns top 5 System Design topics crucial for cracking top Product Based companies. 
3. **get_project_ideas**: Returns top 5 Project ideas crucial for cracking top Product Based companies.

![Node info](/media/node-info.png)

## Serial Execution of Nodes

We start with the serial execution of the nodes in our graph workflow. We can arrange our nodes in the following way.

```python
workflow.add_edge(START, "create_model")
workflow.add_edge("create_model", "get_dsa_topics")
workflow.add_edge("get_dsa_topics", "get_system_design_topics")
workflow.add_edge("get_system_design_topics", "get_project_ideas")
workflow.add_edge("get_project_ideas", END)
```

![serial execution](/media/serial-draw.png)

Below is the output of the execution:
```
Execution completed in 4.972717046737671 seconds
=============================================

DSA Topics: [
    'Arrays', 
    'Linked Lists', 
    'Trees', 
    'Hash Tables', 
    'Graphs', 
]

System Design Topics: [
    'Scalability and Load Balancing', 
    'Database Sharding and Partitioning', 
    'Caching Strategies', 
    'API Design and Rate Limiting', 
    'Consistent Hashing and Data Replication'
]

Project Ideas: [
    'Data Structures and Algorithms Visualizer', 
    'Real-time Code Collaboration Platform', 
    'Customizable Code Editor IDE', 
    'Machine Learning Model Evaluator', 
    'Scalable URL Shortener Service.'
]
```

## Parallel Execution of Nodes

Now, we setup a parallel execution of the nodes in our graph workflow. We can arrange our nodes in the following way.

```python
workflow.add_edge(START, "create_model")
workflow.add_edge("create_model", "get_dsa_topics")
workflow.add_edge("create_model", "get_system_design_topics")
workflow.add_edge("create_model", "get_project_ideas")
workflow.add_edge("get_dsa_topics", END)
workflow.add_edge("get_system_design_topics", END)
workflow.add_edge("get_project_ideas", END)
```

![parallel execution](/media/parallel-draw.png)

Below is the output of the execution:
```
Execution completed in 2.2904281616210938 seconds
=============================================

DSA Topics: ['Arrays', ' Linked Lists', ' Stacks', ' Queues', ' Hash Tables']

System Design Topics: [
    'Distributed Systems', 
    'High Scalability and Low Latency Design', 
    'Consistent Hashing', 
    'Load Balancing', 
    'Data Partitioning'
]

Project Ideas: [
    '1. Online Code Editor with Real-time Collaboration', 
    '2. E-commerce Platform with Secure Payment Integration', 
    '3. Social Media Feed with Infinite Scrolling and Real-time Notifications', 
    '4. Ride-sharing Application with Route Optimization Algorithm', 
    '5. Smart Chatbot with Natural Language Processing and Sentiment Analysis'
]
```

## Results

We reduced our graph based workflow runtime from **4.97 seconds** (serial execution) to **2.29 seconds** (parallel execution). This is about **53.9%** reduction in the workflow response time.

![Result](/media/result-mod.png)

## Configuration

Please install the following packages beforehand.

```python
pip install langchain-openai
pip install langgraph
pip install dotenv
```