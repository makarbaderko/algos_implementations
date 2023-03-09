# Бинарный поиск
Поиск приближенного значения

```cpp
while (l <= r) {
	        int m = (l + r) / 2;
	        if (arr[m] == target){
	            ans = m;
	        }
	        if (arr[m] < target){
	            l = m + 1;
	        }
	        else{
	            r = m - 1;
	        }
    	}
```