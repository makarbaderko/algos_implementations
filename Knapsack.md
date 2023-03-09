# Задача о рюкзаке

## 1. 0-1 Рюкзак
Дано N предметов массой m1, …, mN и стоимостью c1, …, cN соответственно.

Ими наполняют рюкзак, который выдерживает вес не более M. Какую наибольшую стоимость могут иметь предметы в рюкзаке?

```cpp
int main(){
	int n, m;
	cin >> n >> m;
	vector<int> cost(n);
	vector<int> mass(n);
	for (int i = 0; i < n; i++){
		cin >> mass[i];
	}
	for (int i = 0; i < n; i++){
		cin >> cost[i];
	}
	//Knapsack 0-1 Max Cost
	vector<vector<int> >dp(n+1, vector<int> (m+1));
	for (int i = 0; i <= n; i++){
		for (int j = 0; j <= m; j++){
			if (i == 0 || j == 0){
				dp[i][j] = 0;
				//first row/column
			} else if (mass[i-1] <= j){
				dp[i][j] = max(dp[i-1][j], cost[i-1] + dp[i-1][j - mass[i-1]]);
			} else {
				dp[i][j] = dp[i-1][j];
			}
		}
	}
	cout << dp[n][m] << endl;
	return 0;
}
```

## 2. 0-1 рюкзак: точный вес
Дано N золотых слитков массой m1, …, mN. Ими наполняют рюкзак, который выдерживает вес не более M. Можно ли набрать вес в точности M?
*Здесь мы просто в каждой ячейке проверяем, что вес равен искомому. Если да, то обновляем флаг*

```cpp
	int n, m, tmp;
	cin >> n >> m;
	vector<int> cost(n);
	vector<int> mass(n);
	for (int i = 0; i < n; i++){
		cin >> tmp;
		mass[i] = tmp;
		cost[i] = tmp;
	}
	//Knapsack 0-1 Max Cost
	vector<vector<int> >dp(n+1, vector<int> (m+1));
	bool flag = false;
	for (int i = 0; i <= n; i++){
		for (int j = 0; j <= m; j++){
			if (i == 0 || j == 0){
				dp[i][j] = 0;
				//first row/column
			} else if (mass[i-1] <= j){
				dp[i][j] = max(dp[i-1][j], cost[i-1] + dp[i-1][j - mass[i-1]]);
			} else {
				dp[i][j] = dp[i-1][j];
			}
			if (dp[i][j] == m){
				flag = true;
			}
		}
	}
	if (flag == true){
		cout << "YES" << endl;
	} else{
		cout <<	"NO" << endl;
	}
	// cout << dp[n][m] << endl;
	return 0;
```