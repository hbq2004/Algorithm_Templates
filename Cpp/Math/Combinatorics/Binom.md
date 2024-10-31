# Binom

```cpp
int fac(int n) { // n!
    i64 ans = 1;
	for (int i = 1; i <= n; i++) {
        ans = (ans * i) % MOD;
    }
    return ans;
}

int C(i64 a, int b) { // <int(i64, int)>
    if (a < b) {
        return 0;
    }
	int ans = 1;
    for (i64 i = a; i > a - b; i--) {
        ans = (i % MOD * ans) % MOD;
    }
    return (1LL * ans * power(fac(b), MOD - 2)) % MOD;
}
```