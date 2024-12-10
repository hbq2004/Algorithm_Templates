# Gcd

Gcd：最大公约数



`std::__gcd()` 会比 `std::gcd()` 快一点。

但是 `gcd()` 可以处理负数的情况，( 其实等价于 `__gcd` 对结果取绝对值 )。

```cpp
i64 gcd(i64 a, i64 b) {
    return b ? gcd(b, a % b) : a;
}
```

```cpp
i64 gcd(i64 a, i64 b) {
    return b ? gcd(b, a % b) : abs(a);
}
```


# Exgcd


Exgcd：扩展欧几里得

```cpp
i64 exgcd(i64 a, i64 b, i64 &x, i64 &y) {
    if (!b) {
        x = 1, y = 0;
        return a;
    }
    i64 d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}
```

```cpp
array<i64, 3> exgcd(i64 a, i64 b) {
    if (!b) {
        return {a, 1, 0};
    }
    auto [d, x, y] = exgcd(b, a % b);
    return {d, y, x - a / b * y};
}
```