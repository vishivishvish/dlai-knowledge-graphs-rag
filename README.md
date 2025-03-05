# **Knowledge Graphs for RAG**

**[Deeplearning.ai | Neo4j](https://www.deeplearning.ai/short-courses/knowledge-graphs-rag/)**

**Andrew Ng - CEO, Deeplearning.ai**

**Andreas Kollegger - Developer Evangelist & GenAI Lead, Neo4j**

***Notes by Vishnu Subramanian - AI Researcher & Instructor, BloomTech***

***Wednesday, 03/05/2025 - Monday, 03/10/2025***

## ***1 - Introduction***

- Knowledge Graphs are one of the more powerful, but underrated tools in AI.
- Knowledge Graphs provide a way to store and organize data that emphasize the relationships between entities, in contrast to traditional relational databases that only organize data into tables with rows and columns.
- Knowledge Graphs use a graph-based structure with nodes that represent entities like people and companies, and edges that represent the relationships between these entities, such as an employer-employee relationship.
- Each of these nodes / edges in the graph can also store additional information, about the entity (in the case of the node) and about the relationship (in the case of the edge).
- Ex: a person node could store information about the person’s name, email and other details. A company node could store details like the number of employees, annual revenue, etc.
- The employer-employee relationship would be represented by that kind of edge between the above two nodes, and that edge could also store extra information about the nature of that relationship, such as job title, start date, salary etc.
- The graph structure of nodes and edges is very flexible, and lets you more conveniently model some parts of the world than the traditional relational database structure.
- Knowledge Graphs make it easier to represent and search for deep relationships, since the relationship itself is a component of the database, and not just a key shared between two tables.
- This enables much faster execution of queries, getting you to the data you need more efficiently.
- This is why web search engines and e-commerce sites that offer product search capability have found Knowledge Graphs a key technology for delivering relevant results.
- In fact, if you search for a celebrity on Google or Bing, the results you get in the cards to the side have often been retrieved using a Knowledge Graph.
- When you combine a Knowledge Graph with an Embedding model, you have a powerful tool for carrying out Retrieval-Augmented Generation with an LLM.
- That’s because you can take advantage of the relationships and the metadata stored in the graph, to improve the relevance of the text you retrieve and pass to the language model.
- In a basic RAG system, the documents you may want to query or chat with may be first split into smaller sections or chunks, which are then transformed into vectors using an embedding model.
- Once they are in this vectorized form, you can use a similarity function such as Cosine Similarity to search through these chunks of text and find the ones relevant to your prompt.
- But it turns out that storing these text chunks in a Knowledge Graph, opens up new ways to retrieve relevant data from your documents.
- Rather than just similarity search using text embeddings, you can retrieve one chunk, then traverse the Knowledge Graph from that chunk to find other relevant chunks of text, which can potentially give your LLM more complete context.
- We’ll see in the course that this method has the potential to discover connections between text sources that similarity-based retrieval can miss.
- In this course, we will build a Knowledge Graph to represent a collection of financial forms that companies are required to file with the SEC, a US Government Agency that oversees and regulates financial markets, and protects investors.
- We’ll start with an introduction to Knowledge Graphs and see how we can use Neo4j’s query language Cypher to explore and modify a fun graph related to movie data.
- Then we will use Neo4j in combination with a text embedding model, to create vector representations of text fields within the knowledge graph.
- Next, we’ll build a Knowledge Graph to represent one set of SEC forms, and use LangChain to carry out RAG by retrieving text from this graph.
- Lastly we’ll go through the graph creation process one more time, for a second set of SEC forms, connect the two graphs using some linking data, and see how we can use more complex graph queries to carry out retrieval across multiple sets of documents. 
- All this together allows you to ask some very interesting questions of the SEC dataset.

***WIP - More Notes Coming!***
