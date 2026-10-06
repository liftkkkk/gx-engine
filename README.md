# GX 图引擎

> 国产自研内存图引擎：图存储、图搜索、图分区、社区发现与语义检索（GraphRAG），零数据库依赖。

GX 是一个**纯 Python 内存图引擎**，从底层实现图数据结构与图算法，为智能体运行时（如 [GNA 图原生智能体](https://github.com/zzzlift/gna)）等上层应用提供高效图基座。

## 功能

- **图数据结构**：`Node` / `Edge` / `Graph`，有向/无向，邻接表 + 名称索引 + O(1) 边查找
- **图搜索**：缓存化 Dijkstra、BFS/DFS、A*、最短路
- **图指标**：度分布、中心性、聚集系数
- **图分区**：谱聚类、Louvain 社区发现、最小割、平衡分区
- **图可视化**：PyVis 交互式 HTML / Matplotlib 静态图
- **序列化**：JSON / pickle，批量增删
- **语义检索（GraphRAG）**：`SemanticGraph` 支持节点向量化、相似节点搜索与多跳上下文组装（可插拔 Embedding：mock / sentence-transformers / OpenAI）

## 安装

```bash
pip install gx-engine          # 从 PyPI（发布后）
# 或从源码
git clone https://github.com/zzzlift/gx-engine.git
cd gx-engine && pip install -e .
```

## 快速上手

```python
from graph_engine import Graph, Node, Edge

g = Graph(directed=True)
n1 = Node(id="1", name="张三", class_="人物")
n2 = Node(id="2", name="李四", class_="人物")
g.add_node(n1); g.add_node(n2)
g.add_edge_from_st(n1, n2, weight=1.0, label="同事")

for e in g.get_neighbors(n1):
    print(e.target.name, e.label)
```

### GraphRAG 语义检索

```python
from graph_rag import SemanticGraph

sg = SemanticGraph()
sg.add_node_with_text("doc1", "人工智能在医疗领域的应用")
sg.add_node_with_text("doc2", "深度学习算法的最新进展")
print(sg.retrieve_context("AI 医疗", hops=1))
```

更多能力（图指标、分区、可视化）见 `examples/` 与模块内文档。

## 在 GNA 图原生智能体中作为主图引擎

[GNA](https://github.com/zzzlift/gna) 通过 `GX_PATH` 环境变量指向本仓库目录即可启用 GX 作为其世界模型图的存储底座（未安装时 GNA 自动回退 networkx）：

```cmd
setx GX_PATH "C:\path\to\gx-engine"
```

## 依赖

必选：`networkx`、`numpy`、`scikit-learn`。可选：`pyvis`/`matplotlib`（可视化）、`torch`/`transformers`（真实语义向量）。

## License

MIT
