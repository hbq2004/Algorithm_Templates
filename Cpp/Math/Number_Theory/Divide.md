# Divide

分解质因数

- $\sqrt n$


```cpp
int divide(int n) {
    int c = 0;
    for (int i = 2; i <= n / i; i++) {
        if (n % i == 0) {
            int s = 0;
            while (n % i == 0) {
                n /= i;
                s++;
            }
            c += s;
        }
    }
    if (n > 1) {
        c++;
    }
    return c;
}
```


- $\log n \sim \sqrt n$

先筛出 $\sqrt x$ 以内的所有质数，枚举所有质数分解。


```cpp
vector<int> minp, primes;

void sieve(int n) {
    minp.assign(n + 1, 0);
    primes.clear();

    for (int i = 2; i <= n; i++) {
        if (minp[i] == 0) {
            minp[i] = i;
            primes.push_back(i);
        }
        for (auto p : primes) {
            if (i * p > n) {
                break;
            }
            minp[i * p] = p;
            if (p == minp[i]) {
                break;
            }
        }
    }
}

int divide(int x) {
    int c = 0;
    for (auto p : primes) {
        if (x == 1) {
            break;
        }
        while (x % p == 0) {
            x /= p;
            c++;
        }
    }
    if (x > 1) {
        c++;
    }
    return c;
}
```