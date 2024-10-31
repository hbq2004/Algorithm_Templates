# Tree

有关树的所有性质与算法

## 树的直径

```cpp
int L = 0; // the length of Tree Diameter
auto dfs = [&](auto self, int u, int from = -1) -> int {
  	int d1 = 0, d2 = 0;
    for (auto v : adj[u]) {
        if (v == from) {
            continue;
        }
        int d = self(self, v, u) + w[i];
        if (d >= d1) {
            d2 = d1, d1 = d;
        } else if (d > d2) {
            d2 = d;
        }
    }
    L = max(L, d1 + d2);
    return d1;
};
dfs(dfs, 0);
```

CF 的某道题, Mike 出的 (>= 1900*)，如何存储树的直径上的所有节点

```cpp
using PII = pair<int, int>
#define x first
#define y second

vector<int> p(n);
vector<vector<int>> adj(n);

auto dfs = [&](auto self, int u, int from = -1, int dis = 0) -> PII {
    p[u] = from;
    PII res = {dis, u};
    for (auto v : adj[u]) {
        if (v == from) {
            continue;
        }
        res = max(res, self(self, v, u, dis + 1));
    }
    return res;
};

PII da = dfs(dfs, 0);
PII db = dfs(dfs, da.y);

vector<int> dia; // diameter
int u = db.y;
while (u != -1) {
    dia.emplace_back(u);
    u = p[u];
}
```