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

### Community Detection

#### Using modularity to detect communities of nodes.

```
from networkx.algorithms.community import greedy_modularity_communities
communities = sorted(greedy_modularity_communities(G), key=len, reverse=True)
pd.DataFrame(communities)
```

![Alt text](https://github.com/docligot/network_tools/blob/main/community.png)

Detecting number of communities

```
len(communities)

# 4
```

#### Visualizing communities

Utility functions to segment nodes and edges. 

```
def set_node_community(G, communities):
        '''Add community to node attributes'''
        for c, v_c in enumerate(communities):
            for v in v_c:
                # Add 1 to save 0 for external edges
                G.nodes[v]['community'] = c + 1

def set_edge_community(G):
        '''Find internal edges and add their community to their attributes'''
        for v, w, in G.edges:
            if G.nodes[v]['community'] == G.nodes[w]['community']:
                # Internal edge, mark with community
                G.edges[v, w]['community'] = G.nodes[v]['community']
            else:
                # External edge, mark as 0
                G.edges[v, w]['community'] = 0

def get_color(i, r_off=1, g_off=1, b_off=1):
        '''Assign a color to a vertex.'''
        r0, g0, b0 = 0, 0, 0
        n = 16
        low, high = 0.1, 0.9
        span = high - low
        r = low + span * (((i + r_off) * 3) % n) / (n - 1)
        g = low + span * (((i + g_off) * 5) % n) / (n - 1)
        b = low + span * (((i + b_off) * 7) % n) / (n - 1)
        return (r, g, b)
    
# Set node and edge communities
set_node_community(G, communities)
set_edge_community(G)

node_color = [get_color(G.nodes[v]['community']) for v in G.nodes]

# Set community color for edges between members of the same community (internal) and intra-community edges (external)
external = [(v, w) for v, w in G.edges if G.edges[v, w]['community'] == 0]
internal = [(v, w) for v, w in G.edges if G.edges[v, w]['community'] > 0]
internal_color = ['black' for e in internal]
```

Visualizing the community graphs

```
pos = nx.spring_layout(G)

plt.figure(figsize=(15, 10))
nx.draw(G, pos, with_labels=True)
# Draw external edges
nx.draw_networkx(
    G,
    pos,
    node_size=0,
    edgelist=external,
    edge_color="silver")
# Draw nodes and internal edges
nx.draw_networkx(
    G,
    pos,
    node_color=node_color,
    edgelist=internal,
    edge_color=internal_color)
```

![Alt text](https://github.com/docligot/network_tools/blob/main/community_detection.png)

## References

* NetworkX: https://pypi.org/project/networkx/
* Network Analysis on Python: https://coderzcolumn.com/tutorials/data-science/network-analysis-in-python-important-structures-and-bipartite-graphs-networkx
* Stack Exchange: https://datascience.stackexchange.com/questions/61248/create-nodes-edges-from-csv-latitude-and-longitude-for-graphs
* Highlighting Shortest Path in Python https://stackoverflow.com/questions/24024411/highlighting-the-shortest-path-in-a-networkx-graph
* Community Detection on Python https://graphsandnetworks.com/community-detection-using-networkx/
