# Power

快速幂：QuickPower

位运算——

```cpp
i64 power(i64 a, i64 b, const int p = P) {
    i64 res = 1;
    for (; b; b >>= 1, a = a * a % p) {
        if (b & 1) {
            res = res * a % p;
        }
    }
    return res;
}
```


分治写法——

```cpp
i64 quickPow(i64 base, i64 power, i64 mod) {
    if (power == 0) {
        return 1 % mod;
    }
    i64 cur = quickPow(base, power / 2, mod);
    return power & 1 ? base * cur % mod * cur % mod : cur * cur % mod;
}
```