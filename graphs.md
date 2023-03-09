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

*Пример условия задачи*

*На банкет были приглашены N Очень Важных Персон (ОВП). Были поставлены 2 стола. Столы достаточно большие, чтобы все посетители банкета могли сесть за любой из них. Проблема заключается в том, что некоторые ОВП не ладят друг с другом и не могут сидеть за одним столом. Вас попросили определить, возможно ли всех ОВП рассадить за двумя столами.*

```cpp
vector<vector<int > > gr;
vector<int> color;
vector<int> used;
bool dfs(int v){
	for (int i = 0; i < gr[v].size(); i++){
		int unt = gr[v][i];
		if (!used[unt]){
			used[unt] = true;
			color[unt] = !color[v];
			if (dfs(unt) == false){
				return false;
			}
		} else if (color[v] == color[unt]){
			return false;
		}
	}
	return true;
}

int main(){
	int n, m, a, b;
	cin >> n >> m;
	gr.resize(n+1);
	color.assign(n+1, 0);
	used.assign(n+1, 0);
	for (int i = 0; i < m; i++){
		cin >> a >> b;
		gr[a].push_back(b);
		gr[b].push_back(a);
	}
	bool marker = true;
	for (int i = 1; i <= n; i++){
			if (!dfs(i)){
				marker = false;
				break;
			}
	}
	if (marker){
		cout << "YES" << endl;
		for (int i = 1; i <= n; i++){
			if (color[i] == 1) {
				cout << i << " ";
			}
		}
	cout << endl;
	} else {
		cout << "NO" << endl;
	}
	return 0;
}
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
## 4. Подсчёт и вывод компонент связности
```cpp
#include <iostream>
#include <vector>

using namespace std;


vector<vector< int> > gr;
vector<int> used;
vector<vector <int> > comps;
vector<int>curr_comp;


bool dfs(int v, int color){
	if (used[v] != 0){
		return false;
	}
	used[v] = color;
	curr_comp.push_back(v);
	for (int i = 0; i < gr[v].size(); i++){
		int unt = gr[v][i];
		if (used[unt] == 0){
			dfs(unt, color);
		}
	}
	return false;
}

int main(){
	int n, m, a, b;
	cin >> n >> m;
	gr.resize(n);
	used.assign(n, 0);
	for (int i = 0; i < m; i++){
		cin >> a >> b;
		gr[a-1].push_back(b-1);
		gr[b-1].push_back(a-1);
	}
	int counter = 1;
	int i = 0;
	while (i < n){
		if (used[i] != 0){
			i++;
			continue;
		} else {
			curr_comp.clear();
			dfs(i, counter);
			comps.push_back(curr_comp);
			counter++;
			i++;
		}
	}
	return 0;
}
```
## 5. Бусинки (AAAAAGH!!!)
Текст примера задачи

*Маленький мальчик делает бусы. У него есть много пронумерованных бусинок. Каждая бусинка имеет уникальный номер - целое число в диапазоне от 1 до N. Он выкладывает все бусинки на полу и соединяет бусинки между собой произвольным образом так, что замкнутых контуров не образуется. Каждая из бусинок при этом оказывается соединенной с какой-либо другой бусинкой. Требуется определить, какое максимальное количество последовательно соединенных бусинок присутствует в полученной фигуре.*

Обобщённое условие задачи

*Найти самую длинную последовательность вершин в дереве* 

```python
import sys
sys.setrecursionlimit(3000)
def dfs(v, max_children_size):
	used[v] = True
	best_size = 1
	max1 = -1
	max2 = -1
	max_children_size[v] = 1
	for unt in gr[v]:
		if (not used[unt]):
			best_size = max(dfs(unt, max_children_size), best_size)
			if max_children_size[unt] > max1:
				max2 = max1
				max1 = max_children_size[unt]
			elif max_children_size[unt] > max2:
				max2 = max_children_size[unt]
	best_size = max(best_size, max1 + 1)
	best_size = max(best_size, max1 + max2 + 1)
	max_children_size[v] = max(max_children_size[v], max1 + 1)
	return best_size
n = int(input())
gr = [[] for _ in range(n+1)]
for i in range(n-1):
	a, b = map(int, input().split())
	gr[a].append(b)
	gr[b].append(a)
max_children_size = [0] * (n + 1)
used = [False] * (n + 1)
print(dfs(1, max_children_size))
```

Решение на c++ лучше в проде не писать!!!

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int MAXN = 3001;
vector<int> gr[MAXN];
bool used[MAXN];
int max_children_size[MAXN];

int dfs(int v, int& max_children) {
    used[v] = true;
    int best_size = 1;
    int max1 = -1, max2 = -1;
    max_children = 1;
    for (int i = 0; i < gr[v].size(); i++) {
        int unt = gr[v][i];
        if (!used[unt]) {
            int child_max;
            best_size = max(dfs(unt, child_max), best_size);
            if (child_max > max1) {
                max2 = max1;
                max1 = child_max;
            } else if (child_max > max2) {
                max2 = child_max;
            }
            max_children = max(max_children, child_max + 1);
        }
    }
    best_size = max(best_size, max1 + 1);
    best_size = max(best_size, max1 + max2 + 1);
    max_children_size[v] = max(max_children_size[v], max1 + 1);
    return best_size;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n-1; i++) {
        int a, b;
        cin >> a >> b;
        gr[a].push_back(b);
        gr[b].push_back(a);
    }
    int max_children = 0;
    cout << dfs(1, max_children) << endl;
    return 0;
}
```
# Алгоритм Дейкстры
*Задача*

Дан ориентированный взвешенный граф. Найдите кратчайшее расстояние от одной заданной вершины до другой.

```cpp
vector<vector<int> > readGraph(int n) {
    vector<vector<int> > graph(n, vector<int>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> graph[i][j];
        }
    }
    return graph;
}

vector<int> dijkstra(const vector<vector<int> >& graph, int start, int end) {
    int n = graph.size();
    vector<bool> visited(n, false);
    vector<int> dist(n, INF);
    dist[start] = 0;
    for (int i = 0; i < n; i++) {
        int u = -1;
        for (int j = 0; j < n; j++) {
            if (!visited[j] && (u == -1 || dist[j] < dist[u])) {
                u = j;
            }
        }
        if (dist[u] == INF) {
            break;
        }
        visited[u] = true;
        for (int v = 0; v < n; v++) {
            if (graph[u][v] != -1 && u != v && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }
    return dist;
}
```
*Восстановление пути*

# Алгоритм Флойда
```cpp
vector<vector<int> > floyd(){
	vector<vector<int> > A(n, vector<int> (n));
	for (int i = 0; i < n; i++){
		for (int j = 0; j < n; j++){
			A[i][j] = gr[i][j];
		}
	}
	for (int k = 0; k < n; k++){
		for (int i = 0; i < n; i++){
			for (int j = 0; j < n; j++){
				if (A[i][k] != inf && A[k][j] != inf && A[i][k] + A[k][j] < A[i][j]){
					A[i][j] = A[i][k] + A[k][j];
				}
			}
		}
	}
	return A;
}

int main(){
	int s, t, tmp;
	cin >> n >> s >> t;
	s--;
	t--;
	gr.resize(n, vector<int>(n));
	for(int i = 0; i < n; i++){
		for (int j = 0; j < n; j++){
			cin >> tmp;
			if (tmp != -1){
				gr[i][j] = tmp;
			} else {
				gr[i][j] = inf;
			}
		}
	}
	vector<vector<int> > arr = floyd();
	cout << arr[s][t] << endl;
	return 0;
}
```
*Восстановление пути*