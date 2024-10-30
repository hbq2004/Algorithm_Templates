# Bianry Indexed Tree

## 普通树状数组

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

以下待整理，缺少 `find_first()`，`find_last()`，`select()`。

cup-pyy

```cpp
template<typename T>
struct Fenwick {
    int n;
    vector<T> tr;

    Fenwick(int n) : n(n), tr(n + 1, 0) {}

    int lowbit(int x) {
        return x & -x;
    }

    void modify(int x, T v) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += v;
    }

    void modify(int l, int r, T v) {
        modify(l, v);
        if (r + 1 <= n) modify(r + 1, -v);
    }

    T query(int x) {
        T res = T();
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }

    T query(int l, int r) {
        return query(r) - query(l - 1);
    }

    int find_first(T sum) {
        int ans = 0; T val = 0;
        for (int i = __lg(n); i >= 0; i--) {
            if ((ans | (1 << i)) <= n && val + tr[ans | (1 << i)] < sum) {
                ans |= 1 << i;
                val += tr[ans];
            }
        }
        return ans + 1;
    }

    int find_last(T sum) {
        int ans = 0; T val = 0;
        for (int i = __lg(n); i >= 0; i--) {
            if ((ans | (1 << i)) <= n && val + tr[ans | (1 << i)] <= sum) {
                ans |= 1 << i;
                val += tr[ans];
            }
        }
        return ans;
    }
};
```

jiangly

```cpp
template <typename T>
struct Fenwick {
    int n;
    std::vector<T> a;
    
    Fenwick(int n_ = 0) {
        init(n_);
    }
    
    void init(int n_) {
        n = n_;
        a.assign(n, T {});
    }
    
    void add(int x, const T &v) {
        for (int i = x + 1; i <= n; i += i & -i) {
            a[i - 1] = a[i - 1] + v;
        }
    }
    
    T sum(int x) {
        T ans {};
        for (int i = x; i > 0; i -= i & -i) {
            ans = ans + a[i - 1];
        }
        return ans;
    }
    
    T rangeSum(int l, int r) {
        return sum(r) - sum(l);
    }
    
    int select(const T &k) {
        int x = 0;
        T cur {};
        for (int i = 1 << std::__lg(n); i; i /= 2) {
            if (x + i <= n && cur + a[x + i - 1] <= k) {
                x += i;
                cur = cur + a[x - 1];
            }
        }
        return x;
    }
};
```