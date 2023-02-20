# DFS (Depth First Search)
Поиск в глубину
## Пошаговое представление
1. Выбираем любую вершину из еще не пройденных, обозначим ее как .
2. Запускаем процедуру
 - Помечаем вершину как пройденную
 - Для каждой не пройденной смежной с вершиной (назовем ее ) запускаем
3. Повторяем шаги 1 и 2, пока все вершины не окажутся пройденными.
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
