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

#### Directed Graph

```
nx.draw(D, with_labels=True)
plt.show()
```

#### Circular Layout

```
nx.draw_circular(G, with_labels=True)
plt.show()
```

#### Spectral Layout

```
nx.draw_spectral(G, with_labels=True)
plt.show()
```

#### Planar Layout

```
nx.draw_planar(G, with_labels=True)
plt.show()
```

#### Spring Layout

```
nx.draw_spring(G, with_labels=True)
plt.show()
```

#### Shell Layout

```
nx.draw_shell(G, with_labels=True)
plt.show()
```

#### Random Layout

```
nx.draw_random(G, with_labels=True)
plt.show()
```

### Network Statistics

#### Nodes and Edges

```
pd.DataFrame(G.edges)
```

```
pd.DataFrame(G.nodes)
```

#### Degrees

```
degrees = [val for (node, val) in G.degree()]
degrees
```

```
in_degrees = [val for (node, val) in D.in_degree()]
in_degrees
```

```
out_degrees = [val for (node, val) in D.out_degree()]
out_degrees
```

```
out_degrees = [val for (node, val) in D.out_degree()]
out_degrees
```

#### Centrality

```
closeness_centrality = nx.closeness_centrality(G)
pd.DataFrame(list(closeness_centrality.items()))
```

```
betweeness_centrality = nx.betweenness_centrality(G, normalized = True, endpoints = False)
pd.DataFrame(list(betweeness_centrality.items()))
```

```
page_rank = nx.pagerank(G, alpha=0.8)
pd.DataFrame(list(page_rank.items()))
```

## References

