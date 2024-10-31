# Lowest Common Ancestor

## 倍增 LCA

```cpp
int lg = __lg(n);
vector<vector<int>> f(n, vector<int>(lg + 1));
vector<int> dep(n), dis(n);
function<void(int, int, int)> dfs = [&](int u, int depth = 0, int from = -1) {
    dep[u] = depth;
    for (auto v : adj[u]) {
        if (v == from) {
            continue;
        }
        dis[v] = dis[u] + 1;
        f[v][0] = u;
        for (int i = 1; i <= lg; i++) {
            f[v][i] = f[f[v][i - 1]][i - 1];
        }
        dfs(v, depth + 1, u);
    }
};

auto lca = [&](int x, int y) {
    if (dep[x] < dep[y]) {
        swap(x, y);
    }
    for (int i = lg; i >= 0; i--) {
        if ((dep[x] - dep[y]) >> i & 1) {
            x = f[x][i];
        }
    }
    if (x == y) {
        return x;
    }
    for (int i = lg; i >= 0; i--) {
        if (f[x][i] != f[y][i]) {
            x = f[x][i];
            y = f[y][i];
        }
    }
    return f[x][0];
};

auto get = [&](int x, int y) {
    int p = lca(x, y);
    return dis[x] + dis[y] - 2 * dis[p];
};

dfs(0, 0, -1);
```

## RMQ LCA

预处理 $\mathcal O(1)$ LCA

```cpp

```

## 树上差分


```cpp

```




边差分：

```cpp
int dfs_d(int u, int p) {
    int sum = d[u];
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (j == p) {
            continue;
        }
        int t = dfs(j, u);
        if (check(t)) {
            // 更新答案
        }
        sum += t;
    }
    return sum;
}
/*
for query [a, b] :

int p = lca(a, b);
d[a]++, d[b]++, d[p] -= 2;
*/
```







点差分：

```cpp
int dfs_d(int u, int p) {
    int sum = d[u];
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (j == p) {
            continue;
        }
        int t = dfs(j, u);
        sum += t;
    }
    if (check(sum)) {
        // 更新答案
    }
    return sum;
}

/*
for query [a, b] :

int p = lca(a, b);
d[a]++, d[b]++, d[p]--, d[fa[p][0]]--;
*/
```



**点差分和边差分最后的求子树和都可以用 DFS 序列逆序遍历求和**。



`dfn[]` + `fa[][]` 求子树和 / 求到根节点的路径和 ( 前缀和 )：



```cpp
// i 表示 DFS 序列中第 i 个节点

// dfn 求子树和：
for (int i = n; i >= 1; i--) {
    sum[fa[i][0]] += sum[i];
}

// dfn 求到根节点的路径和：
for (int i = 1; i <= n; i++) {
    sum[i] += sum[fa[i][0]];
}
```