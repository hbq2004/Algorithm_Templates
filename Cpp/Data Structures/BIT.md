# Bianry Indexed Tree

# 普通树状数组

```cpp
template <typename T>
struct Fenwick {
    int n;
    vector<T> tr;
    
    Fenwick(int n) : n(n), tr(n + 1, 0) {}
	
    int lowbit(int x) {
        return x & -x;
    }
    
    void add(int x, const T &v) {
        for (int i = x; i <= n; i += lowbit(i)) {
            tr[i] += v;
        }
    }
    
    T query(int x) {
        T res {};
        for (int i = x; i; i -= lowbit(i)) {
            res += tr[i];
        }
        return res;
    }
    
    T query(int l, int r) {
        return query(r) - query(l - 1);
    }
};
```

以下待整理，缺少 `find_first()`，`find_last()`，`select`。

cup-pyy

```cpp

```

jiangly

```cpp

```