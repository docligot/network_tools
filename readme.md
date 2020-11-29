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

```
# Undirected Graph
G = nx.from_pandas_edgelist(edges, source = 'from', target = 'to')

# Directed Graph
D = nx.from_pandas_edgelist(edges, source = 'from', target = 'to', create_using=nx.DiGraph())

```

### Visualization

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

## References

* NetworkX: https://pypi.org/project/networkx/
* Network Analysis on Python: https://coderzcolumn.com/tutorials/data-science/network-analysis-in-python-important-structures-and-bipartite-graphs-networkx
* Stack Exchange: https://datascience.stackexchange.com/questions/61248/create-nodes-edges-from-csv-latitude-and-longitude-for-graphs