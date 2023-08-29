
___
```
1. Реализация графа (хеш-таблица): узлы = вложенные хеш-таблицы с весами
graph["start"] = {"a": 6, "b": 2}
graph["a"] = {"fin": 1}
graph["b"] = {"a": 3, "fin": 5}
graph["fin"] = {}
2. Реализация хеш-таблицы для хранения стоимостей узлов.
costs = {}
costs["a"] = 6
costs["b"] = 2
costs["fin"] = float("inf")
3. Реализация хеш-таблицы родительских узлов (родителей).
parents = {}
parents["a"] = "start"
parents["b"] = "start"
parents["fin"] = None
4. Функция поиска узла с наименьшей стоимостью.

def find_lowest_cost_node(costs, processed):
    lowest_cost = float("inf")
    lowest_cost_node = None

    for node in costs.keys():

        if node not in processed:
            cost = costs[node]

            if cost < lowest_cost:
                lowest_cost = cost
                lowest_cost_node = node

    return lowest_cost_node
    
5. Реализация алгоритма Дейкстры.

def deikstra(graph, costs, parents, finish_node="fin"):
    processed = []

    node = find_lowest_cost_node(costs, processed)
    while node is not None:
        cost = costs[node]
        neighbors = graph[node]

        for neighbor in neighbors.keys():
            new_cost = cost + neighbors[neighbor]

            if costs[neighbor] > new_cost:
                costs[neighbor] = new_cost
                parents[neighbor] = node

        processed.append(node)
        node = find_lowest_cost_node(costs, processed)

    return costs[finish_node]

```
___
Relations: [[Алгоритм Дейкстры]] 
Tags: #code
References: [[Грокаем алгоритмы]] 
Query: 