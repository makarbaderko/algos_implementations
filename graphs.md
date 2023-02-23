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

Здесь реализуется задача о кратчайшем пути в невзвешенном графе между двумя данными вершинами.

```cpp
int n;
vector<vector<int> >gr;
vector<int>prevv;
vector<int>used;
queue<int> q;

void bfs(int a, int b){
	q.push(a);
	used[a] = true;
	prevv[a] = -1;
	while (!q.empty()) {
	    int v = q.front();
	    q.pop();
	    for (int u : gr[v]) {
	        if (!used[u]) {
	            used[u] = true;
	            q.push(u);
	            prevv[u] = v;
	        }
	    }
	}
}

```

### Восстановление пути

```cpp
void printPath(int a, int b){
	if (used[b] == 0) {
    	cout << -1 << endl;
	} else {
	    vector<int> path;
	    for (int v = b; v != -1; v = prevv[v])
	        path.push_back(v);
	    reverse(path.begin(), path.end());
	    cout << path.size()-1 << endl;
	    if (path.size() != 1){
		    for (int v : path){
		        cout << v+1 << " ";
		    }
		    cout << endl;
		}
	}
}
```
# Ключевые задачи
## 1. Проверка графа на двудольность и разделение его на две доли

Начинаем покраску с произвольной вершины, которую красим в произвольный цвет. При прохождении по каждому ребру красим следующую вершину в противоположный цвет. Если при переборе соседних вершин мы нашли вершину, уже покрашенную в тот же цвет, что и текущая, то в графе существует нечётный цикл, а значит, он не является двудольным.

```cpp
//TODO
```

## 2. Проверка графа на ацикличность
Граф записывается только в одну сторону, чтобы избежать повторов.

```cpp
//если flag == true, цикл есть.
void dfs(int v){
	if (flag == true){
		return;
	}
	color[v] = 1;
	for (int i = 0; i < gr[v].size(); i++){
		int unt = gr[v][i];
		if (color[unt] == 1) {
			//if gray
			flag = true;
			return;
		} else {
			dfs(unt);
		} if (flag == true){
			return;
		}
	}
	color[v] = 2; //black
}
```
## 3. Проверка графа на "деревянность"
Дерево - связный ацикличкский граф, где между каждой парой вершин существует только один простой путь, и, в нем $E = V - 1$. Это усложенный вариант задачи "Проверка графа на ацикличность".
В коде приведенном ниже используется другой алгоритм проверки цикличности.
В процессе dfsа функция returnится, если обнаруживает уже пройденную вершину.

```cpp
vector<vector <int> >gr;
vector<int> used;

void dfs(int v){
	if (used[v] == 1){
		return;
	}
	used[v] = 1;
	for (int i = 0; i < gr[v].size(); i++){
		int unt = gr[v][i];
		dfs(unt);
	}
	return;
}

int main(){
	for (int i = 0; i < n; i++){
		for (int j = 0; j < n; j++){
			cin >> tmp;
			if (tmp == 1){
				e++;
				gr[i].push_back(j);
			}
		}
	}
	//Check for connectivity
	dfs(0);
	bool isConnected = true;
	int i = 0;
	while(isConnected && i < n){
		if (used[i] == 0){
			isConnected = false;
		}
		i++;
	}
	//Check that E = V - 1
	if (((e/2) == n - 1) && isConnected){
		cout << "YES" << endl;
	} else {
		cout << "NO" << endl;
	}
	return 0;
}
```