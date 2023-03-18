def read_graph(filename):
    with open(filename, 'r') as f:
        n = int(f.readline().strip())
        weights = []
        for i in range(n):
            weights.append(list(map(int, f.readline().strip().split())))
        return weights

def find_min_edge(weights, connected):
    min_edge = (float('inf'), None, None)
    for i in range(len(weights)):
        for j in range(len(weights[i])):
            if weights[i][j] == 0:
                continue
            if connected[i] != connected[j] and weights[i][j] < min_edge[0]:
                min_edge = (weights[i][j], i, j)
    return min_edge

def boruvka(weights):
    n = len(weights)
    connected = [i for i in range(n)]
    result = []
    while len(set(connected)) > 1:
        min_edges = [find_min_edge(weights, connected) for i in range(n)]
        for i in range(n):
            weight, node1, node2 = min_edges[i]
            if connected[node1] != connected[node2]:
                result.append((node1, node2, weight))
                old_comp, new_comp = connected[node1], connected[node2]
                for j in range(n):
                    if connected[j] == old_comp:
                        connected[j] = new_comp
    return result

weights = read_graph('input.txt')
print(boruvka(weights))
