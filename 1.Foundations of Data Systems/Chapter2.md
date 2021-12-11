# Chapter 2: Data models and Querying languages
### Disclaimer Note
I will be focusing on the data models and not the languages used to access them, because the point
of this repo is to focus on system design rather then language semantics. To read up on that, refer to the book.

Data models are at the core of software problems, they determine how we think about the problem at hand

- We model the places, objects, sensors and locations into objects and models
- We map these things into JSON, XML, Databases
- Low level engineers decide how to represent these Json, xml in memory and bytes

## Relational Model Versus Document Model
### SQL
- The roots of relational databases lie in business data processing, transaction processing and batch processing.
- Relational model came into being in order to address the need to model business processing and transactions. SQL came into being in 1970

### NoSQL
- Came into being due to a need for higher scalability (write operations)
- Open source databases
- Less restrictive schemas
- Specialized query operations that  are not well supported by the relational model

### Object relational mismatch
Common criticism of SQL is the disparity between how we represent objects in OOP and the SQL

Thus there is a preference by some to store data as JSON due to it's locality. Two ways to do that:
 
1. Store data as JSON strings in relational databases, but it would not be possible to query
specific fields inside the JSON
2. Second way is to store data as JSON itself in document databases like MongoDB and Firestore

### Network model
The biggest debate in 1970 was between choosing the relational and network model. Network models as the name implies contain data in a tree like parent child relationship, for example the location "Great Seatle area" would have multiple parents(people).

We traverse a network model through access paths kind of like a linked list, this can become complex for large nested queries, like traversing an n dimensional space.

Documents databases are nested like network models, but joins are similar in document and relational DBS, joins for doc DBS have to be done in the application side. which isn't a problem as long as there isn't deep nesting. Relational model stores all data in tables in tuples with no nesting laying it all bear.

## Which data model leads to simpler application code? Which to use, relational or document DB
- If you have data that is in a tree like structure that you have to show in a single page, better to go with document db instead of shredding the data into tables and joining it.
- If your application is doing a lot of deep joins, better to go with relational model.
- For highly interconnected data, the document model is awkward, the relational model is acceptable,
and graph models are the most natural.

### Note
Document databases are not schema less, schema is imposed by the application code as it expects a certain kind of data, relational.

## Declarative(SQL) vs Imperative languages (IMS, CODASYL)
- Declarative languages are. concise and just specify the type of data we want; lower level software can decide which algorithm to use
- Imperative code also specifies the sequence and order of execution of the code, this makes it difficult to implement a new more efficient algorithm or parallel processing

## Graph like data models
Many to many relationships are a huge deciding factor for which data model to go for, if your data does not have a lot of
many to many relationships, document dbs are a good option, if they do have some, relational models are good as well.

But by far the best way to represent many to many relationships are graph dbs.

A graph consists of two kinds of objects: vertices (also known as nodes or entities) and
edges (also known as relationships or arcs). Many kinds of data can be modelled as a
graph. 

### Typical examples include
**Social graphs**
Vertices are people, and edges indicate which people know each other.
**The Web Graph**
Vertices are web pages, and edges indicate HTML links to other pages.
**Road or Rail networks**
Vertices are junctions, and edges represent the roads or railway lines between
them.

![Example of graph-structured data (boxes represent vertices, arrows represent
edges).](../resources/graph.png)

Example of graph-structured data (boxes represent vertices, arrows represent
edges).

## Property Graph Model ((implemented by Neo4j, Titan, and InfiniteGraph)
This model has two entities, vertices and edges, the vertices consists of a unique identifier, outgoing, incoming edges and a set of key value pairs. 
The edges consist of a unique identifier, a vertex where the edge starts and one it ends, a label to describe the relationship this edge represents and a set of key value pairs.

- A property graph can be used to create many sorts of relationships.
- It has a loose schema and is easily evolvable. It can be created in a relational model as well.

## Triple Store Model
Similar to the property graph model, it as object-verb-subject, approach to storing information.
Where the object is a vertex, a subject can be a vertex or a primitive value

- Triple stores are related to the semantic web but not co dependant on them.
- Semantic web is the idea of storing information about a website in machine readable form to create a database of everything on the 
web.

## Ending Note
1.	Document databases target use cases where data comes in self-contained documents
and relationships between one document and another are rare.
2. Graph databases go in the opposite direction, targeting use cases where anything
is potentially related to everything.





