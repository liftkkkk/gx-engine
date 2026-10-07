# GX Graph Engine

**English** | [简体中文](README.zh-CN.md)

> A self-developed, pure-Python in-memory graph engine: graph storage, search, partitioning, community detection and semantic retrieval (GraphRAG) - zero database dependency.

GX is a **pure-Python in-memory graph engine** built from the ground up, providing the graph substrate for agent runtimes such as [GNA - Graph-Native Agent](https://github.com/liftkkkk/gna) and any other application that needs fast, dependency-light graph operations.

## Features

- **Graph data structures**: `Node` / `Edge` / `Graph`, directed & undirected, adjacency lists + name index + O(1) edge lookup
- **Graph search**: cached Dijkstra, BFS/DFS, A*
- **Graph metrics**: degree distribution, centrality, clustering coefficient
- **Graph partitioning**: spectral clustering, Louvain community detection, min-cut, balanced partitioning
- **Visualization**: interactive PyVis HTML / static Matplotlib
- **Serialization**: JSON / pickle, batch operations
- **Semantic retrieval (GraphRAG)**: `SemanticGraph` with node embeddings, similar-node search and multi-hop context assembly (pluggable embeddings: mock / sentence-transformers / OpenAI)

## Install

```bash
pip install gx-engine          # from PyPI (once published)
# or from source
git clone https://github.com/liftkkkk/gx-engine.git
cd gx-engine && pip install -e .
```

## Quick start

```python
from graph_engine import Graph, Node, Edge

g = Graph(directed=True)
n1 = Node(id="1", name="Alice", class_="person")
n2 = Node(id="2", name="Bob", class_="person")
g.add_node(n1)
g.add_node(n2)
g.add_edge_from_st(n1, n2, weight=1.0, label="colleague")

for e in g.get_neighbors(n1):
    print(e.target.name, e.label)
```

### GraphRAG semantic retrieval

```python
from graph_rag import SemanticGraph

sg = SemanticGraph()
sg.add_node_with_text("doc1", "AI applications in healthcare")
sg.add_node_with_text("doc2", "Latest advances in deep learning")
print(sg.retrieve_context("AI healthcare", hops=1))
```

More capabilities (metrics, partitioning, visualization) in `examples/` and the module docs.

## As the primary graph engine of GNA

[GNA - Graph-Native Agent](https://github.com/liftkkkk/gna) uses GX as its **primary backend** (highest priority): point `GX_PATH` at this repository - or simply `pip install gx-engine` - and GNA auto-detects it at startup. Without GX, GNA falls back to networkx automatically and stays fully functional.

## Dependencies

Required: `networkx`, `numpy`, `scikit-learn`. Optional: `pyvis` / `matplotlib` (visualization), `torch` / `transformers` (real semantic embeddings).

## License

MIT

---

## Buy me a coffee

GX Graph Engine is an open-source project I develop and maintain in my spare time, free forever.

If it helped you power graph storage, retrieval, or your agent's graph substrate, consider buying me a
coffee (9.9 CNY is enough). Every bit of support goes straight into new features and bug fixes.

<p align="center">
  <img src="icon.jpg" alt="Buy me a coffee" width="280">
</p>

<p align="center"><i>Leave a note with your donation about the feature you want most - I prioritize those ;)</i></p>

**Can't donate?** Starring the repo, opening an Issue, or sharing it with someone who needs it helps just as much!
