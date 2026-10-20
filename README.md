# graphs_esroka

A small Python library of graph algorithms. It currently provides **Dijkstra's shortest path algorithm**, built on a min-heap, for weighted graphs with non-negative edge weights.

## Installation

Install from [PyPI](https://pypi.org/project/graphs-esroka/) with pip (requires Python 3.8+):

```bash
pip install graphs_esroka
```

Or install the latest version straight from GitHub (also requires Git). The package lives in the `src` folder, so the URL points there:

```bash
pip install "git+https://github.com/ethancs13/graphs_esroka.git#subdirectory=src"
```

To uninstall:

```bash
pip uninstall graphs_esroka
```

## Quick Start

```python
from graphs_esroka import sp

graph = {
    'A': {'B': 4, 'C': 1},
    'B': {'D': 1},
    'C': {'B': 2, 'D': 5},
    'D': {},
}

dist, path = sp.dijkstra(graph, 'A')

print(dist)  # {'A': 0, 'B': 3, 'C': 1, 'D': 4}
print(path)  # {'A': [], 'B': ['A', 'C'], 'C': ['A'], 'D': ['A', 'C', 'B']}
```

## API

### `sp.dijkstra(graph, source)`

Finds the shortest path from `source` to every node it can reach.

#### Parameters

- `graph`: a dictionary of dictionaries, where `graph[u][v]` is the weight of the edge from `u` to `v`. Edges are directed, so for an undirected graph, list each edge in both directions.
- `source`: the node to start from. It must be a key in `graph`.

#### Returns

A tuple `(dist, path)`:

- `dist`: a dictionary mapping each node to its shortest distance from `source`. Nodes that can't be reached keep the value `sys.maxsize`.
- `path`: a dictionary mapping each reachable node to the list of nodes visited **before** it, starting at `source`. The destination itself is not included, so `path['D'] == ['A', 'C', 'B']` means the route is A → C → B → D. Nodes that can't be reached are left out.

#### Requirements for the input graph

- Every node must appear as a key in `graph`, even nodes with no outgoing edges (give those an empty dictionary, like `'D': {}` above). Otherwise `dijkstra` raises a `KeyError`.
- Edge weights must be non-negative numbers. Dijkstra's algorithm does not give correct results with negative weights.

## Running the Example Script

`test.py` reads a graph from a text file and prints the shortest paths from node `0`:

```bash
python src/test.py path/to/graph.txt
```

Each line of the file is one directed edge, written as `source destination weight` (all integers):

```text
0 1 4
0 7 8
1 0 4
1 2 8
...
```

Example output:

```text
Shortest distances from 0:
{0: 0, 1: 4, 2: 12, 3: 19, 4: 21, 5: 11, 6: 9, 7: 8, 8: 14}
spf to 0: []
spf to 1: [0]
spf to 7: [0]
spf to 2: [0, 1]
...
```

## Project Structure

```text
graphs_esroka/
├── src/
│   ├── graphs_esroka/
│   │   ├── __init__.py
│   │   ├── heapq.py      # min-heap used by Dijkstra's algorithm
│   │   └── sp.py         # shortest path algorithms
│   ├── test.py           # example script
│   ├── README.md         # package description used by pip
│   └── pyproject.toml    # package settings used by pip
├── LICENSE
└── README.md
```

## Development

Clone the repository and install it in editable mode, so your code changes take effect without reinstalling:

```bash
git clone https://github.com/ethancs13/graphs_esroka.git
cd graphs_esroka
pip install -e src
```

To build the distribution files (`.whl` and `.tar.gz`) into `src/dist`:

```bash
pip install build
python -m build src
```

## Contributing

Contributions are welcome. The `main` branch is protected, and all development happens on `dev`:

1. Create a branch from `dev` for your change.
2. Open a pull request into `dev`.
3. Changes are merged from `dev` into `main` through a pull request when they are ready for release.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
