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

## ***2 - Knowledge Graph Fundamentals***

- We’ve been introduced to Knowledge Graphs as a data structure that stores information in nodes and in the edges that highlight the relationships between these nodes.
- Let’s look at these concepts in some more detail.
- Nodes are data records. We’re going to explore what that means by drawing what a graph might look like, and seeing how the data patterns within the graph emerge.

<img src="https://drive.google.com/uc?export=view&id=1xrjfJnI9HYVhc10LzCh94nfRCBh9wohL">

- Above, let’s assume we have a simple, one-node graph (a Person node called “Andreas”)
- Let’s say we can represent this graph in notation as just (Person)
- Let’s say we add another person called “Andrew” in this graph.

<img src="https://drive.google.com/uc?export=view&id=1I5FMxrj15lI0MYMYMmophoN1upIxFb7_">

- The data pattern (notation) for this graph would be (Person) , (Person)
- But in case we have a relationship between these two people, where we want to encode that Andreas knows Andrew since 2024 - relationships are also data records in a Knowledge Graph.

<img src="https://drive.google.com/uc?export=view&id=1x5ceIsWRrxK1DUMXfTZO3ljtsSIDvA0-">

- The data pattern for this graph would be (Person) -[KNOWS]-> (Person)
- The nodes and relationships in a Knowledge Graph are also geometrically called vertices and edges.
- You can think of a “relationship” between nodes as involving a pair of nodes and then information about that pair.

<img src="https://drive.google.com/uc?export=view&id=1_M7A8pg2mIjfwYgfEtw9l_9tv8HQdkRi">

- In this sense, a relationship actually “contains” two nodes.
- Let’s imagine a slightly more complex graph, with three nodes, one of which is also of a different type (Course) compared to the other two (Persons).

<img src="https://drive.google.com/uc?export=view&id=1oC630LTJNd7NKJhpPumpu0kGRV3oRkmS">

- The data pattern here is (Person)-[TEACHES]->(Course)<-[INTRODUCES]-(Person)
- The good thing about these labels on the relationships is that, obviously, thanks to Embeddings, now we don’t need to add any other semantically similar information such as saying “Person 1 is a Teacher of the course”, because applying the embeddings to the “Teaches” nature of the relationship will already give us that meaning.
- Here’s another small complex graph like this, with 3 nodes and 3 relationships. 

<img src="https://drive.google.com/uc?export=view&id=1QLVs9k-8ylQFWCEcvsT84OAI9dgw75aE">

- Now formally, nodes have labels. Such as the “Person” node or the “Course” node in the above graph. Labels are a way of grouping multiple nodes together.
- Nodes, because they’re data records, also have values of course. So you can consider the label-value pairing as the dictionary-style key-value pairing that defines a node, such as Person: “Andreas”, Person: “Andrew”, Course: “Knowledge Graphs for RAG”

<img src="https://drive.google.com/uc?export=view&id=1_6oll7DFiK2PlJnRX8CNBJWQT6ktLx21">

- Relationships are also data records, so they have a direction, a type and properties.

<img src="https://drive.google.com/uc?export=view&id=1tla53lUtTyTz_Xrj6u44u3h5U9__-8nE">

- To summarize, a Knowledge Graph is a database that stores information in nodes and relationships.
- Both nodes and relationships can have properties (key-value pairs).
- Nodes can be given labels to help group them together.
- Relationships always have a type and a direction.

## ***3 - Querying Knowledge Graphs***

- Now that we’ve explored the fundamental ideas behind Knowledge Graphs, let’s look at how they’re put into practice.
- We’ll use the Cypher query language to interact with a fun Knowledge Graph that contains data about movies and actors.

<img src="https://drive.google.com/uc?export=view&id=1-_Va0BUOz-8xLc2Q8SWOuiQ-JA_edG8B">

- For both these types of nodes in this Knowledge Graph, Persons and Movies, they have unique types of properties. The “Person” node has a “name” property (string) and year “born” property (integer), while the “Movie” node has properties “title”, “tagline” (both strings) and year “released” (integer).

<img src="https://drive.google.com/uc?export=view&id=1YbwEk2GONk2sx0elcTUQbywT-yhM8bUW">

- And of course, there are many kinds of relationships that could exist between nodes belonging to the “Person” and “Movie” classes.

<img src="https://drive.google.com/uc?export=view&id=1ncUIqEfIK2kJiO2JsvFDYLs68hOmA_UK">

- To write Cypher queries in Python, we will use a multi-line string, delineated by the triple quotation mark (“””) symbol.
- As an example:

`“””MATCH (n)`

`RETURN count(n)”””`

- In the above query, the MATCH keyword is usually looking for a pattern-matching condition. In the absence of one, it just selects all the nodes in the graph. We assign the results of that query to a variable “n”, and when we RETURN the count(n), that should give us the number of nodes in the entire graph.
- Now, when we run:
  
`result = kg.query(cypher);`

`result`

- We get the output as:
  
`[{‘count(n)’: 171}]`

- So we see that the output of the Knowledge Graph query is a list with each row having a dictionary. In this case, due to only 1 row, there’s only one dictionary. The key of the dictionary is based on what we’ve returned in the RETURN clause - which in this case is ‘count(n)’.
- We have the option of using an alias to rename this key. So:
  
`cypher = “””MATCH (n)`

`RETURN count(n) AS numberOfNodes”””;`

`result = kg.query(cypher);`

`result`

- Now the output we would get would be:
  
`[{‘numberOfNodes’: 171}]`

- Now, if we only want to pattern-match a specific kind of node with the MATCH keyword, we can add a colon (:) symbol and mention the kind of node, like so:
  
`cypher = “””MATCH (n:Movie)`

`RETURN count(n) AS numberOfMovies”””;`

- We would get the output as:
  
`[{‘numberOfMovies’: 38}]`

- This of course shows that out of 171 nodes, we have 38 movie nodes in this graph.
- The variable ‘n’ is of course, also just a placeholder. We can change that to ‘m’ to improve readability.
  
`cypher = “””MATCH (m:Movie)`

`RETURN count(m) AS numberOfMovies”””;`

- Now, to get the number of persons:
  
`cypher = “””MATCH (p:Person)`

`RETURN count(p) AS numberOfPeople”””;`

- What we get is:

`[{‘numberOfPeople’: 133}]`

- If we want to return a specific person, let’s say Tom Hanks, we have to write the cypher query like so:
  
`cypher = “””MATCH (tom:Person {name: ‘Tom Hanks’})`

`RETURN tom”””;`

`print(kg.query(cypher));`

- What we get is:
  
`[{‘tom’: {‘born’: 1956, ‘name’: ‘Tom Hanks’}}]`

- The key of the main dictionary stays the same as the variable name we used in the Cypher query.
- We get a list with only one dictionary row, meaning only one node has matched our search condition.
  
- Let’s say we’re searching for the node pertaining to the movie Cloud Atlas:
  
`cypher = ””MATCH (cloudAtlas:Movie {title: ‘Cloud Atlas’})`

`RETURN cloudAtlas”””;`

`print(kg.query(cypher));`

- We get:

`[{‘cloudAtlas’: {‘tagline’: ‘Everything is connected’, ‘title’: ‘Cloud Atlas’, ‘released’: 2012}}]`

- Let’s say we’re only interested in the value of the ‘released’ property of this node, and not the entire node.
- We can modify the Cypher query like so:

`cypher = ””MATCH (cloudAtlas:Movie {title: ‘Cloud Atlas’})`

`RETURN cloudAtlas.released”””;`

- We will get:

`[{‘cloudAtlas.released’: 2012}]`

- We can also return two properties from the node if we want, like so:

`cypher = ””MATCH (cloudAtlas:Movie {title: ‘Cloud Atlas’})`

`RETURN cloudAtlas.released, cloudAtlas.tagline”””;`

- We would get:

`[{‘cloudAtlas.released’: 2012, ‘cloudAtlas.tagline’: ‘Everything is connected’}]`

- Let’s say we’re looking for the titles of movies in this database that were released in the nineties (1990 <= release year < 2000).
- Similar to SQL, this would be a filter in our Cypher query like so:

`cypher = ””MATCH (nineties:Movie)`

`WHERE nineties.released >= 1990`

`AND nineties.released < 2000`

`RETURN nineties.title“””;`

- Our return value:

<img src="https://drive.google.com/uc?export=view&id=1N_BgoWm40y8JgyDgetgROakVf7wSBZAg">


***WIP - More Notes Incoming!***

