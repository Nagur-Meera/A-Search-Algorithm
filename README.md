# A* Search Algorithm

This repository demonstrates the implementation of an **A* Search Algorithm** using a graph and heuristic values in Python.

## Concept
The **A* Search Algorithm** is an informed search strategy that finds the shortest path from a start node to a goal node. It uses both the cost to reach a node and an estimated cost from the node to the goal.

### Key Characteristics:
- Uses a cost function `f(n) = g(n) + h(n)`:
  - `g(n)` is the cost to reach node `n` from the start.
  - `h(n)` is the heuristic estimate of the cost to reach the goal from `n`.
- Selects the node with the minimum `f(n)` value.
- Guarantees finding the optimal path if the heuristic is admissible (never overestimates the true cost).
![image](https://github.com/user-attachments/assets/7c9726f1-6c9b-40e2-9e33-90483fa6805b)

## Problem Representation
The graph used in this implementation is represented as an adjacency list with edge costs:

```python
graph = {
    'S': [('A', 1), ('G', 10)],
    'A': [('B', 2), ('C', 1)],
    'B': [('D', 5)],
    'C': [('D', 3), ('G', 4)],
    'D': [('G', 1)],
    'G': []
}
```

Each node also has a heuristic value representing the estimated cost from that node to the goal:

```python
heuristic = {
    'S': 5, 'A': 3, 'B': 4, 'C': 2, 'D': 6, 'G': 0
}
```

## Algorithm Implementation
The core logic is as follows:

1. Start from the initial node.
2. Keep track of the cost to reach each node (`g` value).
3. Evaluate each node using `f(n) = g(n) + h(n)`.
4. Expand the node with the minimum `f(n)` value.
5. Update the cost and parent if a better path is found.
6. Repeat until the goal node is reached or no nodes are left to explore.

### Python Code
```python
def find_min_f(open_list, g_values):
    min_node = open_list[0]
    min_f = g_values[min_node] + heuristic[min_node]

    for node in open_list:
        f_value = g_values[node] + heuristic[node]
        if f_value < min_f:
            min_f = f_value
            min_node = node

    return min_node

def a_star(start, goal):
    open_list = [start]
    closed_list = []

    g_values = {start: 0}
    parent = {start: None}

    while open_list:
        current = find_min_f(open_list, g_values)
        open_list.remove(current)
        closed_list.append(current)

        print(f"Visiting: {current}")

        if current == goal:
            path = []
            while current is not None:
                path.append(current)
                current = parent[current]
            path.reverse()
            print("Goal Reached!")
            print("Path:", " -> ".join(path))
            print("Cost:", g_values[goal])
            return

        for neighbor, cost in graph[current]:
            if neighbor in closed_list:
                continue

            tentative_g = g_values[current] + cost

            if neighbor not in open_list:
                open_list.append(neighbor)
            elif tentative_g >= g_values.get(neighbor, float('inf')):
                continue

            parent[neighbor] = current
            g_values[neighbor] = tentative_g

    print("Goal not reachable.")

a_star('S', 'G')
```

## Output
Example output:
```
Visiting: S
Visiting: A
Visiting: C
Visiting: G
Goal Reached!
Path: S -> A -> C -> G
Cost: 6
```

## How It Works
- Starts at node `S`.
- Evaluates `A` and `G`, selecting `A` as it has a lower `f` value.
- Explores neighbors of `A`, moving towards `C`.
- Finally, reaches `G` through the optimal path `S -> A -> C -> G` with a cost of 6.

## Limitations
- Performance depends on the heuristic function.
- If the heuristic is not admissible, it may not guarantee the optimal path.

