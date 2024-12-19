<p align="center"> ������������ ����������� ���������� ��������</p>
<p align="center">���������� �����������</p>
<p align="center">���������� ��������������� ����������� �����������</p>
<p align="center">������� ���</p>
<br><br><br><br><br><br><br>
<p align="center">������������ ������ �3</p>
<p align="center">�� ���������� ������ ������ ���������������� ������</p>
<p align="center">����: ����������� ������ ���������� ��� ������ �� ��������</p>
<br><br><br><br><br>
<p align="right">��������:</p>
<p align="right">������� 2 �����</p>
<p align="right">������ ��-25</p>
<p align="right">���������� �. �.</p>
<p align="right">���������:</p>
<p align="right">�������� �. �.</p>
<br><br><br><br><br>
<p align="center">����� 2024</p>

<hr>

# ����� ������� #
1. �������� ����� �� ����������� ������������ ������ �1 � .md ������� (readme.md) � � ������� ������� �� �������� ��������� (pull request) ���������� ��� � ��������� ��������: trunk\ii0xxyy\task_02\doc (��� xx - ����� ������, yy - ����� ��������, �������� ii02302)..
2. �������� ��� ���������� ��������� ���������� � ��������: **trunk\ii0xxyy\task_01\src**.
## ������� ##
�������. �� C++ ����������� ���������, ������������ ������������� ���� ���-���������. � �������� ������� ���������� ������������ �������������� ������, ���������� � ���������� ������. � ������ ����� �������� ������� ��� ������ ������� ����������� �������, �������� ���������� ����������.

<hr>

# ���������� ������� #

��� ���������:
```
#include <iostream>
#include <vector>
#include <list>
#include <queue>
#include <unordered_set>

using namespace std;

class DirectedGraph {
private:
    struct Vertex {
        int identifier;
        list<int> adjacentVertices;
    };

    vector<Vertex> vertexList;

    // �������� �� ������� �������� �����
    bool hasEulerianCycle() const {
        for (const auto& vertex : vertexList) {
            if (vertex.adjacentVertices.size() % 2 != 0) {
                return false;
            }
        }
        return true;
    }

    // ����������� ������� ��� ������ ������������ �����
    bool searchHamiltonianCycle(int currentVertex, int pathLength, vector<int>& path, vector<bool>& visitedVertices) {
        if (pathLength == vertexList.size()) {
            return path.front() == path.back();
        }

        for (int neighbor : vertexList[currentVertex].adjacentVertices) {
            if (!visitedVertices[neighbor]) {
                visitedVertices[neighbor] = true;
                path[pathLength] = neighbor;
                if (searchHamiltonianCycle(neighbor, pathLength + 1, path, visitedVertices)) {
                    return true;
                }
                visitedVertices[neighbor] = false;
            }
        }
        return false;
    }

public:
    // ���������� ������ �������
    void insertVertex(int id) {
        vertexList.push_back({ id, {} });
    }

    // ���������� ���� (����������������� ����)
    void insertEdge(int start, int end) {
        vertexList[start].adjacentVertices.push_back(end);
        vertexList[end].adjacentVertices.push_back(start);
    }

    // ����������� �����
    void displayGraph() const {
        for (const auto& vertex : vertexList) {
            cout << "Vertex " << vertex.identifier << ": ";
            for (int neighbor : vertex.adjacentVertices) {
                cout << neighbor << " ";
            }
            cout << endl;
        }
    }

    // ��������� �������� �����
    vector<int> retrieveEulerianCycle() {
        vector<int> cycle;
        if (!hasEulerianCycle()) return cycle;

        vector<bool> visited(vertexList.size(), false);
        list<int> traversalStack;
        traversalStack.push_back(0);

        while (!traversalStack.empty()) {
            int currentVertex = traversalStack.back();
            if (!vertexList[currentVertex].adjacentVertices.empty()) {
                int nextVertex = vertexList[currentVertex].adjacentVertices.front();
                traversalStack.push_back(nextVertex);
                vertexList[currentVertex].adjacentVertices.remove(nextVertex);
                vertexList[nextVertex].adjacentVertices.remove(currentVertex);
            }
            else {
                cycle.push_back(currentVertex);
                traversalStack.pop_back();
            }
        }
        return cycle;
    }

    // ��������� ������������ �����
    vector<int> retrieveHamiltonianCycle() {
        vector<int> cycle(vertexList.size(), -1);
        vector<bool> visited(vertexList.size(), false);
        visited[0] = true;
        cycle[0] = 0;

        if (searchHamiltonianCycle(0, 1, cycle, visited)) {
            return cycle;
        }
        return {};
    }

    // ���������� ��������� ������ (���������� BFS)
    DirectedGraph createSpanningTree() const {
        DirectedGraph spanningTree;
        for (int i = 0; i < vertexList.size(); ++i) {
            spanningTree.insertVertex(i);
        }

        vector<bool> visited(vertexList.size(), false);
        visited[0] = true;
        queue<int> bfsQueue;
        bfsQueue.push(0);

        while (!bfsQueue.empty()) {
            int currentVertex = bfsQueue.front();
            bfsQueue.pop();

            for (int neighbor : vertexList[currentVertex].adjacentVertices) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    spanningTree.insertEdge(currentVertex, neighbor);
                    bfsQueue.push(neighbor);
                }
            }
        }
        return spanningTree;
    }
};

int main() {
    setlocale(LC_ALL, "Rus");
    DirectedGraph graph;

    // ��������� �������
    graph.insertVertex(0);
    graph.insertVertex(1);
    graph.insertVertex(2);
    graph.insertVertex(3);
    graph.insertVertex(4);

    // ��������� ����
    graph.insertEdge(0, 1);
    graph.insertEdge(1, 2);
    graph.insertEdge(2, 3);
    graph.insertEdge(3, 4);
    graph.insertEdge(4, 0);

    // ������� ����
    cout << "Graph structure:\n";
    graph.displayGraph();

    // �������� ������� �������� �����
    vector<int> eulerianCycle = graph.retrieveEulerianCycle();
    if (!eulerianCycle.empty()) {
        cout << "Eulerian cycle: ";
        for (int vertex : eulerianCycle) {
            cout << vertex << " ";
        }
        cout << endl;
    }
    else {
        cout << "No Eulerian cycle found.\n";
    }

    // �������� ������� ������������ �����
    vector<int> hamiltonianCycle = graph.retrieveHamiltonianCycle();
    if (!hamiltonianCycle.empty()) {
        cout << "����������� ����: ";
        for (int vertex : hamiltonianCycle) {
            cout << vertex << " ";
        }
        cout << endl;
    }
    else {
        cout << "No Hamiltonian cycle found.\n";
    }

    // �������� ��������� ������
    DirectedGraph spanningTree = graph.createSpanningTree();
    cout << "Spanning Tree structure:\n";
    spanningTree.displayGraph();

    return 0;
}

```     
```

Graph structure:
Vertex 0: 1 4
Vertex 1: 0 2
Vertex 2: 1 3
Vertex 3: 2 4
Vertex 4: 3 0
Eulerian cycle: 0 4 3 2 1 0
No Hamiltonian cycle found.
Spanning Tree structure:
Vertex 0:
Vertex 1:
Vertex 2:
Vertex 3:
Vertex 4: 
```