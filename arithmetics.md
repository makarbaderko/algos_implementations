# Арифметические алгоритмы (Arithmetic Algorithms)

## НОД (Наибольший общий делитель)

### Python
#### Custom function
Алгоритм Евклида

```
def gcd(a, b): 
    while b != 0: 
        a, b = b, a % b 
    return a
```
#### Math approach
```
math.gcd(a, b)
```
---
### C++
#### Custom function
```
int gcd(int a, int b) {
   if (b == 0){
   		return a;
   	}
   return gcd(b, a % b);
}
```
### STD
```
#include<numeric>

std::gcd(a, b);
```
--
## НОК (Наименьшее общее кратное)
### Python
#### Custom function
```
def lcm(a, b):
	if a > b:
		greater = a
	else:
		greater = b
	while True:
		if greater % x == 0 and greater % y == 0:
			return greater
		greater += 1
```
### GCD -> LCM Approach
This code works because LCM * GCD = a * b

```
def lcm(a, b):
   lcm = (a * b) // gcd(a,b)
   return lcm
```
#### Math approach
```
math.lcm(a, b)
```
--
### C++
#### Custom function
```
int lcm(int a, int b){
	int greater = (a > b) ? a : b
	while(true){
		if ((greater % a == 0) && (greater % b == 0){
			return greater
		} 
		greater++;
	}
}
```
#### STD
```
#include<numeric>

std::lcm(a, b);
```
--
## Решето Эратосфена
Выводит все простые числа меньшие N
### Python
```
def sieve(n):
    prime = [True for i in range(n+1)]
    primes = []
    p = 2
    while (p * p <= n):
        if (prime[p] == True):
            for i in range(p * p, n+1, p):
                prime[i] = False
        p += 1
 
    # Print all prime numbers
    for p in range(2, n+1):
        if prime[p]:
            primes.append(p)
    return primes
```