# Binom

## 小范围预处理

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

```cpp
/**   组合数（小范围预处理，逆元+杨辉三角）
 *    2023-10-06: https://qoj.ac/submission/203196
 *    2024-03-14: https://qoj.ac/submission/353877
**/
constexpr int P = 1000000007;
constexpr int L = 10000;

int fac[L + 1], invfac[L + 1];
int sumbinom[L + 1][7];

int binom(int n, int m) {
    if (n < m || m < 0) {
        return 0;
    }
    return 1LL * fac[n] * invfac[m] % P * invfac[n - m] % P;
}

int power(int a, int b) {
    int res = 1;
    for (; b; b /= 2, a = 1LL * a * a % P) {
        if (b % 2) {
            res = 1LL * res * a % P;
        }
    }
    return res;
}

fac[0] = 1;
for (int i = 1; i <= L; i++) {
    fac[i] = 1LL * fac[i - 1] * i % P;
}
invfac[L] = power(fac[L], P - 2);
for (int i = L; i; i--) {
    invfac[i - 1] = 1LL * invfac[i] * i % P;
}

sumbinom[0][0] = 1;
for (int i = 1; i <= L; i++) {
    for (int j = 0; j < 7; j++) {
        sumbinom[i][j] = (sumbinom[i - 1][j] + sumbinom[i - 1][(j + 6) % 7]) % P;
    }
}
```


## 大范围预处理 (需要Modint)

```cpp
struct Comb {
    int n;
    std::vector<Z> _fac;
    std::vector<Z> _invfac;
    std::vector<Z> _inv;
    
    Comb() : n{0}, _fac{1}, _invfac{1}, _inv{0} {}
    Comb(int n) : Comb() {
        init(n);
    }
    
    void init(int m) {
        if (m <= n) return;
        _fac.resize(m + 1);
        _invfac.resize(m + 1);
        _inv.resize(m + 1);
        
        for (int i = n + 1; i <= m; i++) {
            _fac[i] = _fac[i - 1] * i;
        }
        _invfac[m] = _fac[m].inv();
        for (int i = m; i > n; i--) {
            _invfac[i - 1] = _invfac[i] * i;
            _inv[i] = _invfac[i] * _fac[i - 1];
        }
        n = m;
    }
    
    Z fac(int m) {
        if (m > n) init(2 * m);
        return _fac[m];
    }
    Z invfac(int m) {
        if (m > n) init(2 * m);
        return _invfac[m];
    }
    Z inv(int m) {
        if (m > n) init(2 * m);
        return _inv[m];
    }
    Z binom(int n, int m) {
        if (n < m || m < 0) return 0;
        return fac(n) * invfac(m) * invfac(n - m);
    }
} comb;
```