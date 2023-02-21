# DFS (Depth First Search)
Поиск в глубину
## Пошаговое представление
1. Выбираем любую вершину из еще не пройденных, обозначим ее как .
2. Запускаем процедуру
 - Помечаем вершину как пройденную
 - Для каждой не пройденной смежной с вершиной (назовем ее ) запускаем
3. Повторяем шаги 1 и 2, пока все вершины не окажутся пройденными.

## Визуализация
![](https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif)

## Python

```python
gr = [] # Graph represented as an adjacency list
n = 100 # Number of vertices
used = [False] * n
def dfs(v):
	used[v] = True
	for unt in gr[v]:
		if not used[unt]:
			dfs(unt)
```
## C++

```cpp
vector<int> used;
vector<vector <int> > gr; //Graph represented as adjacency list
int n; // Number of vertices

void dfs(int v){
    used[v] = 1;
    for(int i = 0; i < gr[v].size(); i++){
        int unt = gr[v][i];
        if (used[unt] == 0){
        	dfs(unt);
        }
    }
}
```
# BFS (Breadth First Search)
Поиск в ширину, волновой обход
## Пошаговое представление
- Пусть задан невзвешенный ориентированный граф G=(V,E) в котором выделена исходная вершина s.
- Требуется найти длину кратчайшего пути (если таковой имеется) от одной заданной вершины до другой. 
- Для алгоритма нам потребуются очередь и множество посещенных вершин used, которые изначально содержат одну вершину s. 

1. На каждом шагу алгоритм берет из начала очереди вершину v и добавляет все непосещенные смежные с v
вершины в used и в конец очереди. 
2. Если очередь пуста, то алгоритм завершает работу.used

## Визуализация

![](https://upload.wikimedia.org/wikipedia/commons/5/5d/Breadth-First-Search-Algorithm.gif?20100504223639)

<img src="https://neerc.ifmo.ru/wiki/images/thumb/2/28/Graph-BFS.gif/240px-Graph-BFS.gif" width="500" height="300" />

## Python
```python
used = []
queue = []     #Initialize a queue

def bfs(used, graph, node): 
  used.append(node)
  queue.append(node)
  while queue:
    m = queue.pop(0) 
    for unt in graph[m]:
      if unt not in used:
        used.append(unt)
        queue.append(unt)
```
## C++
```cpp

```