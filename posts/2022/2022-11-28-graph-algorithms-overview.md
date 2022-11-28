# Graph Algorithms <!-- omit in toc -->

## 1. Algorithm Types

- **Centrality**
  - Indicators of centrality assign numbers or rankings to nodes within a graph corresponding to their network position
- **Classification**
  - Classify the graph into sets
- **Community**
  - A network is said to have community structure if the nodes of the network can be easily grouped into sets of nodes such that each set of nodes is densely connected internally
- **GraphML/Embeddings**
  - A graph embedding is an approach converting neighborhood topology into a fixed size vector of decimal values
- **Path**
  - Identify path between vertexes and path analytics
- **Similarity**
  - Computes similarity between pairs of items
- **Topological Link Prediction**
  - Link prediction is the problem of predicting the existence of a link between two entities in a network
- **Frequent Pattern Mining**
  - An analytical process that finds frequent patterns

## 2. Algorithms

### 2.1. Centrality

<table>
	<thead>
        <tr>
            <th>Name</th>
            <th>Mechanism</th>
            <th>Use Case</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Eigenvector</td>
            <td>A vertex's influence is based on its indegree and the influence of the vertices which refer to it.</td>
            <td rowspan=3>
				<ul>
					<li>Finding influencer from a social network</li>
					<li>Ranking web pages for searching engine</li>
					<li>Identify the marketing targets within a doctor referral network</li>
					<li>Personalized version can be used for user user behavior prediction and recommender systems</li>
				</ul>
			</td>
        </tr>
		<tr>
			<td>PageRank</td>
			<td>Variant of Eigenvector. Adding additional jump probability</td>
		</tr>
		<tr>
			<td>Article Rank</td>
			<td>Variant of PageRank. Lowers the influence of low-degree nodes</td>
		</tr>
		<tr>
			<td>Closeness</td>
			<td>Find vertexes that have shorter distance to all other nodes</td>
			<td>
				<ul>
					<li>Recommend locations for a grocery store</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Harmonic</td>
			<td>Variant of Closeness Centrality. Reverses the sum and reciprocal operations in the definition of closeness centrality</td>
			<td>
				<ul>
					<li>Same as Closeness, but works better when run on disconnected graph</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Degree</td>
			<td>Variant of Closeness Centrality. Reverses the sum and reciprocal operations in the definition of closeness centrality</td>
			<td>
				<ul>
					<li>Same as Closeness, but works better when run on disconnected graph</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Betweenness</td>
			<td>Variant of Closeness Centrality. Reverses the sum and reciprocal operations in the definition of closeness centrality</td>
			<td>
				<ul>
					<li>Same as Closeness, but works better when run on disconnected graph</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Influence Maximization</td>
			<td>Variant of Closeness Centrality. Reverses the sum and reciprocal operations in the definition of closeness centrality</td>
			<td>
				<ul>
					<li>Same as Closeness, but works better when run on disconnected graph</li>
				</ul>
			</td>
		</tr>
    </tbody>
</table>

#### 2.1.1. PageRank

#### 2.1.2. Article Rank

#### 2.1.3. Betweenness

#### 2.1.4. Closeness

#### 2.1.5. Degree

#### 2.1.6. Eigenvector

#### 2.1.7. Harmonic

#### 2.1.8. Influence Maximization

### 2.2. Community

<div>
<table>
	<thead>
        <tr>
            <th>Name</th>
            <th>Mechanism</th>
            <th>Use Case</th>
        </tr>
    </thead>
	<tbody>
		<tr>
			<td>Weakly Connected Components (WCC)</td>
			<td>Every vertex is reachable from every other vertex through an undirected path</td>
			<td>
				<ul>
					<li>Entity Resolution, obtain a holistic picture of an given user entity</li>
					<li>Fraud ring detection</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Strongly Connected Components</td>
			<td>Every vertex is reachable from every other vertex through an directed path</td>
			<td>
				<ul>
					<li>AML, money laundering ring detection</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>K Core</td>
			<td>Maximal connected subgraph in which every vertex is connected to at least k vertices in the subgraph</td>
			<td>
				<ul>
					<li>Network analytical characterization of hierarchical layers (Hierarchical result based on different k values)</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>K-means</td>
			<td>Partition vertexes into k clusters in which each vertex belongs to the cluster with the nearest mean</td>
			<td>
				<ul>
					<li>Grouping transportation orders into clusters based on longitude and latitude</li>
					<li>Grouping product into categories based on their embeddings</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Label Propagation</td>
			<td>If the plurality of your neighbors all bear the label X, then you should label yourself as also a member of X</td>
			<td rowspan=3>
				<ul>
					<li>Anti-fraud, detect fraud rings that is a group of applications/accounts that share common PII info</li>
					<li>Detect user communities in a social network, at later stage, one could extract features of each user communities for marketing purposes</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Speaker-Listener Label Propagation</td>
			<td>Variant of the Label Propagation algorithm that is able to detect overlapping communities</td>
		</tr>
		<tr>
			<td>Louvain</td>
			<td>Partitions the vertices in a graph by approximately maximizing the graph's modularity score</td>
		</tr>
		<tr>
			<td>Local Cluster Coefficient (LCC)</td>
			<td>local clustering coefficient = Number_trangles/((n-1)n/2)</td>
			<td>
				<ul>
					<li>Anti phone call fraud, how many pairs of your callees called each other</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Triangle Counting</td>
			<td>Count the number of triangles in the graph</td>
			<td>
				<ul>
					<li>Evaluate the cohesiveness of communities</li>
				</ul>
			</td>
		</tr>
	</tbody>
</table>
</div>

---

#### 2.2.1. Connected Components

#### 2.2.2. K Core

#### 2.2.3. K Means

#### 2.2.4. Label Propagation

#### 2.2.5. Local Cluster Coefficient

#### 2.2.6. Louvain

#### 2.2.7. Speaker-Listener Label

#### 2.2.8. Propagation

#### 2.2.9. Triangle Counting

### 2.3. GraphML/Embeddings

<div>
<table>
	<thead>
		<tr>
			<th>Name</th>
			<th>Mechanism</th>
			<th>Use Case</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>FastRP</td>
			<td>It generates node embeddings of low dimensionality through random projections from the graph’s adjacency matrix to a low-dimensional matrix
			<td rowspan=2>
				<ul>
					<li>Use the embeddings as machine Learning features</li>
					<li>Use the embeddings for similarity algorithms</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>Node2Vec</td>
			<td>Uses random walks in the graph to create a vector representation of a node</td>
		</tr>
	</tbody>
</table>
</div>

#### 2.3.1. FastRP

#### 2.3.2. Node2Vec

### 2.4. Path

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Mechanism</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Astar Shortest Path</td>
      <td>A shortest path algorithm guided by a heuristic function that estimates the cost of the cheapest path</td>
      <td rowspan=2>
        <ul>
          <li>Route planning. Planning the best route for delivering an order based on train schedules</li>
          <li>Flight search. Planning a series of flights that take least amount of travelling time</li>
          <li>Anti-fraud. Analyse relationship between a new credit card application and a blacklisted application</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Shortest Path</td>
      <td>Return either the shortest or top k shortest unweighted/weighted paths connecting a set of source vertices to a set of target vertices</td>
    </tr>
    <tr>
      <td>Cycle Detection</td>
      <td>Find all the cycles (loops) in a graph</td>
      <td>
        <ul>
          <li>AML, find the transaction loops</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Estimated Diameter</td>
      <td>The worst-case length of a shortest path between any pair of vertices in a graph</td>
      <td>
        <ul>
          <li>Understand the interconnectedness of the graph to support following analytics, e.g. to choose small world version of WCC or standard version</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Maxflow</td>
      <td>Finding a feasible flow through a flow network that obtains the maximum possible flow rate</td>
      <td>
        <ul>
          <li>AML, find the maximum money flow between two accounts</li>
          <li>Supply chain, find the maximum supply flow from one site to another</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Minimum Spanning Tree</td>
      <td>A minimum spanning tree is a set of edges that can connect all the vertices in the graph with the minimal sum of edge weights</td>
      <td rowspan=2>
        <ul>
          <li>Travelling salesman problem</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Minimum Spanning Forest</td>
      <td>A set of minimum spanning trees, one for each WCC</td>
    </tr>
  </tbody>
</table>

#### 2.4.1. Astar_shortest_path

#### 2.4.2. BFS

#### 2.4.3. Cycle_detection

#### 2.4.4. Estimated_diameter

#### 2.4.5. Maxflow

#### 2.4.6. Minimum_spanning_forest

#### 2.4.7. Minimum_spanning_tree

#### 2.4.8. Shortest Path

### 2.5. Classification

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Mechanism</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Greedy Graph Coloring</td>
      <td>Assigns a unique integer value known as its color to the vertices of a graph such that no neighboring vertices share the same color</td>
      <td rowspan=2>
        <ul>
          <li>Find the maximum tasks that can be performed together without conflicting with each other</li>
          <li>Find the maximum of vertexes that can execute some logic in parallel without conflicting with each other
        </ul>
      </td>
    </tr>
    <tr>
      <td>Maximal Independent Set</td>
      <td>An independent set of vertices does not contain any pair of vertices that are neighbors</td>
    </tr>
  </tbody>
</table>

#### 2.5.1. Greedy Graph Coloring

#### 2.5.2. Maximal Independent Set

### 2.6. Similarity

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Mechanism</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Cosine Similarity</td>
      <td>Calculates cosine similarity based on topological common attributes</td>
      <td rowspan=3>
        <ul>
          <li>Find similar users, patients</li>
          <li>User purchased item A also purchased product B</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Jaccard Similarity</td>
      <td>Calculates jaccard similarity based on topological common attributes</td>
    </tr>
    <tr>
      <td>Approximate Nearest Neighbors</td>
      <td>Calculate the approximate nearest neighbors with similarity score based on non-topological attribute values</td>
    </tr>
    <tr>
      <td>K Nearest Neighbors</td>
      <td>Based on similarity scores makes a prediction for every vertex whose label is not known</td>
      <td>
        <ul>
          <li>Anti-fraud, predict fraud labels</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

#### 2.6.1. Cosine

#### 2.6.2. Jaccard

#### 2.6.3. K Nearest Neighbors

#### 2.6.4. Approximate Nearest Neighbors

### 2.7. Topological Link Prediction

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Mechanism</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Adamic Adar</td>
      <td>Number of shared links between two nodes</td>
      <td rowspan=6>
        <ul>
          <li>Collaborative filtering for recommender systems</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Common Neighbors</td>
      <td>Number of common neighbors between two vertices</td>
    </tr>
    <tr>
      <td>Preferential Attachment</td>
      <td>Product of the number of neighbors of the two vertices</td>
    </tr>
    <tr>
      <td>Resource Allocation</td>
      <td>Reciprocal of number of neighbors of common neighbors</td>
    </tr>
    <tr>
      <td>Same Community</td>
      <td>Returns 1 if they two vertices are in the same community, and returns 0 if they are not in the same community</td>
    </tr>
    <tr>
      <td>Total Neighbors</td>
      <td>Total number of neighbors of two vertices</td>
    </tr>
  </tbody>
</table>

#### 2.7.1. Adamic Adar

#### 2.7.2. Common Neighbors

#### 2.7.3. Preferential Attachment

#### 2.7.4. Resource Allocation

#### 2.7.5. Same Community

#### 2.7.6. Total Neighbors

### 2.8. Frequent Pattern Mining

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Mechanism</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Apriori</td>
      <td>Returns top k most frequent sequential patterns</td>
      <td>
        <ul>
          <li>Application UX improvements</li>
          <li>Customer churn prediction</li>
          <li>Bad rating cause analysis</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

#### 2.8.1. Aprior
