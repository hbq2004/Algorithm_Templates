# Disjoint Set Union


## 普通并查集


1. `p[i]`：表示元素 $i$ 的代表元素
2. `siz[i]`：表示集合 $i$ 的大小
3. `cnt`；表示当前集合的个数


```cpp
struct DSU {
    vector<int> p, siz;
    int cnt;
    
    DSU() {}
    DSU(int n) : p(n + 1), siz(n + 1, 1), cnt(n) {
        iota(p.begin(), p.end(), 0);
    }

    int find(int x) {
        return p[x] == x ? x : p[x] = find(p[x]);
    }

    bool same(int x, int y) {
        return find(x) == find(y); 
    }
    
    bool merge(int x, int y) {
        x = find(x), y = find(y);
        if (x == y) {
            return false;
        }
        cnt--;
        siz[y] += siz[x];
        p[x] = y; // x -> y
        return true;
    }
    
    int size(int x) {
        return siz[find(x)];
    }
};
```

- 启发式合并修改

```cpp
vector<Type> C; // Contribution 
bool merge(int x, int y) {
    x = find(x), y = find(y);
    if (x == y) {
        return false;
    }
    if (C[x] > C[y]) {
        swap(x, y);
    }
    // C[x] -> C[y]
    p[x] = y; // x -> y
    return true;
}
```

以下待整理

## 带权并查集

```cpp
struct DSU {
    vector<int> p, d;
    DSU(int n) : p(n + 1), d(n + 1) {
        iota(p.begin(), p.end(), 0);
    }

    int find(int x) {
        if (x != p[x]) {
            int t = p[x];
            p[x] = find(p[x]);
            d[x] += d[t];
        }
        return p[x];
    }
    
    bool merge(int x, int y, int v) {
        int px = find(x), py = find(y);
        if (px == py) {
            return (d[x] - d[y] == v);
        }
        p[px] = py;
		d[px] = d[y] - d[x] + v;
        return true;
    }
};
```

带点权并查集：



[Acwing 2069. 网络分析](https://www.acwing.com/activity/content/problem/content/2472/) 



此题需要深刻理解 `find()` 函数路径压缩时的具体过程。

```cpp
int find(int x) {
    if (p[x] == x || p[p[x]] == p[x]) {
		return p[x];
    }
	int t = p[x];
    p[x] = find(p[x]);
    d[x] += d[t];
    return p[x];
}

for (int i = 1; i <= n; i++) {
    int t = find(i);
    if (i == t) {
        cout << d[i] << ' ';
    } else {
        cout << d[i] + d[t] << ' ';
    }
}
cout << '\n';
```


## 可持久化并查集

```cpp

```


## 可撤销并查集

```cpp

```



