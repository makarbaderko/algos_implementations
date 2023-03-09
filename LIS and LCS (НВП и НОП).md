# НОП (Наибольшая общая подпоследовательность)
Даны две последовательности, требуется найти и вывести их наибольшую общую подпоследовательность.

```cpp
int main(){
	int n, m;
	cin >> n;
	int a[n];
	for (int i = 0; i < n; i++){
		cin >> a[i];
	}
	cin >> m;
	int b[m];
	for (int j = 0; j < m; j++){
		cin >> b[j];
	}
	int dp[n+1][m+1];
	for (int i = 0; i <= n; i++){
		for (int j = 0; j <= m; j++){
			dp[i][j] = 0;
		}
	}
	for (int i = 1; i <= n; i++){
		for (int j = 1; j <= m; j++){
			dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
			if (a[i-1] == b[j-1]){
				dp[i][j] = max(dp[i][j], dp[i-1][j-1]+1);
			}
			// cout << dp[i][j] << " ";
		} // cout << endl;
	}
	int i = n;
	int j = m;
	vector<int>ans;
	while (i > 0 && j > 0){
		if(a[i-1] == b[j-1]){
			ans.push_back(a[i-1]);
			i--;
			j--;
		} else if (dp[i-1][j] > dp[i][j-1]){
			i--;
		} else {
			j--;
		}
	}
	reverse(ans.begin(), ans.end());
	for (auto c : ans){
		cout << c << " ";
	} cout << endl;
	return 0;
}
```
# НВП (Наибольшая Возрастающая Подпоследовательность)

Дана последовательность, требуется найти её наибольшую возрастающую подпоследовательность.

```cpp
int main(){
	int n, m, tmp;
	cin >> n;
	int a[n];
	for (int i = 0; i < n; i++){
		cin >> a[i];
	}
	// cout << "Data Read\n";
	vector<int> dp(n, 1);
	vector<int> prev(n, -1);
	for (int i = 1; i < n; i++){
		for (int j = 0; j < i; j++){
			// cout << "Working with i= " << i << " j = " << j << endl;
			if ((a[j] < a[i]) && (dp[j] + 1 > dp[i])){
				prev[i] = j;
				dp[i] = dp[j] + 1;
			}
		}
	}
	// cout << "Loop Finished\n";
	int index = max_element(dp.begin(), dp.end()) - dp.begin();
	// cout << "Max element found\n";
	vector<int>ans;
	while (index != -1){
		// cout << "Index = " << index << " prev = " << prev[index] << endl;
		ans.push_back(a[index]);
		index = prev[index];
	}
	// cout << "Ans calculated and ready to reverse\n";
	reverse(ans.begin(), ans.end());
	for (auto i : ans){
		cout << i << " ";
	}
	cout << endl;
	return 0;
}
```