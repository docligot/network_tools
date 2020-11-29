# Network Tools

Tools for analyzing networks. 

## NetworkX

### Dependencies


```
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt
import numpy as np
```

### Load data

```
edges = pd.read_csv('edges_sample.csv', header=0, encoding = "ISO-8859-1")
edges
```
![Alt text](https://github.com/docligot/network_tools/blob/main/edge_data.png)

### Create Graphs

Undirected graph only considers connections between nodes, directed graphs also consider the direction (source to target) of the connections between nodes.

```
# Undirected Graph
G = nx.from_pandas_edgelist(edges, source = 'from', target = 'to')

# Directed Graph
D = nx.from_pandas_edgelist(edges, source = 'from', target = 'to', create_using=nx.DiGraph())

```

### Visualization

Various layouts are availble to visualize a graph for exploration and interpretation. 

#### Undirected Graph

```
nx.draw(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/undirected_graph.png)

#### Directed Graph

```
nx.draw(D, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/directed_graph.png)

#### Circular Layout

```
nx.draw_circular(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/circular_graph.png)

#### Spectral Layout

```
nx.draw_spectral(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/spectral_graph.png)

#### Planar Layout

```
nx.draw_planar(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/planar_graph.png)

#### Spring Layout

```
nx.draw_spring(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/spring_graph.png)

#### Shell Layout

```
nx.draw_shell(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/shell_graph.png)

#### Random Layout

```
nx.draw_random(G, with_labels=True)
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/random_graph.png)

### Network Statistics

#### Nodes and Edges

```
pd.DataFrame(G.edges)
```
![Alt text](https://github.com/docligot/network_tools/blob/main/edges.png)

```
pd.DataFrame(G.nodes)
```
![Alt text](https://github.com/docligot/network_tools/blob/main/nodes.png)

#### Degrees

Degree is a measure of the number of connections of a node. In directed graphs, in-degree is inward connection, and out-degree is outward direction. 

```
degrees = [val for (node, val) in G.degree()]
degrees

# [2, 3, 3, 5, 4, 2, 4, 2, 1, 2, 5, 3, 2, 3, 2, 1]

```

```
in_degrees = [val for (node, val) in D.in_degree()]
in_degrees


# [0, 3, 0, 3, 1, 2, 0, 2, 1, 2, 3, 2, 0, 3, 0, 1]

```

```
out_degrees = [val for (node, val) in D.out_degree()]
out_degrees

# [2, 0, 3, 2, 4, 0, 4, 0, 0, 0, 3, 1, 2, 0, 2, 0]

```

#### Centrality

Centrality measures (closeness, betweenness, page rank) identify highly influential nodes in a network. Nodes with high centrality are often traversed by paths from one end of the network to another. 

```
closeness_centrality = nx.closeness_centrality(G)
pd.DataFrame(list(closeness_centrality.items()))
```
![Alt text](https://github.com/docligot/network_tools/blob/main/closeness_centrality.png)


```
betweeness_centrality = nx.betweenness_centrality(G, normalized = True, endpoints = False)
pd.DataFrame(list(betweeness_centrality.items()))
```
![Alt text](https://github.com/docligot/network_tools/blob/main/betweeness_centrality.png)

```
page_rank = nx.pagerank(G, alpha=0.8)
pd.DataFrame(list(page_rank.items()))
```
![Alt text](https://github.com/docligot/network_tools/blob/main/page_rank.png)

### Shortest Paths

#### Calculate Shortest Path

```
shortest_path = nx.shortest_path(G, '?????? Hongkongese revolution channel', 'Psychedelics Today')
shortest_path

# ['?????? Hongkongese revolution channel',
 'HEAL California',
 'Patel Prasad',
 'Healthy Earth - Healthy Home',
 'Blind Joe',
 "Doug's Compounding Pharmacy",
 'Psychedelics Today']
```

#### Highlight Shortest Path

Undirected Path

```
pos = nx.spring_layout(G)
plt.figure(figsize=(10,10)) 
shortest_graph = G.subgraph(shortest_path)
nx.draw(G, pos, with_labels=True)
shortest_edges = list(zip(shortest_path, shortest_path[1:]))
nx.draw_networkx_nodes(G, pos, nodelist=shortest_path, node_color="r", label=G.nodes)
nx.draw_networkx_edges(G, pos, edgelist=shortest_edges, edge_color="r")
plt.axis('equal')
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/shortest_graph.png)

Directed Path
```
pos = nx.shell_layout(D)
plt.figure(figsize=(10,10)) 
shortest_graph = D.subgraph(shortest_path)
nx.draw(D, pos, with_labels=True)
shortest_edges = list(zip(shortest_path, shortest_path[1:]))
nx.draw_networkx_nodes(D, pos, nodelist=shortest_path, node_color="r", label=G.nodes)
nx.draw_networkx_edges(D, pos, edgelist=shortest_edges, edge_color="r")
plt.axis('equal')
plt.show()
```
![Alt text](https://github.com/docligot/network_tools/blob/main/shortest_directed_graph.png)


## References

* NetworkX: https://pypi.org/project/networkx/
* Network Analysis on Python: https://coderzcolumn.com/tutorials/data-science/network-analysis-in-python-important-structures-and-bipartite-graphs-networkx
* Stack Exchange: https://datascience.stackexchange.com/questions/61248/create-nodes-edges-from-csv-latitude-and-longitude-for-graphs
* Highlighting Shortest Path in Python https://stackoverflow.com/questions/24024411/highlighting-the-shortest-path-in-a-networkx-graph