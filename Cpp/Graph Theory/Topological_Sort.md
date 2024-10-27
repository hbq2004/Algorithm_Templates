# Topological Sort

[AcWing 848. 有向图的拓扑序列](https://www.acwing.com/activity/content/problem/content/911/) 

## STL

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    vector<vector<int>> adj(n);
    vector<int> ind(n);
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        u--, v--;
        adj[u].emplace_back(v);
        ind[v]++;
    }

    vector<int> seq;
    auto topo = [&]() {
        queue<int> q;
        for (int i = 0; i < n; i++) {
            if (ind[i] == 0) {
                q.emplace(i);
            }
        }
        while (!q.empty()) {
            auto u = q.front();
            q.pop();
            seq.emplace_back(u);
            for (auto v : adj[u]) {
                if (--ind[v] == 0) {
                    q.emplace(v);
                }
            }
        }
    };

    topo();

    if (seq.size() < n) {
        return cout << -1, 0;
    }

    for (auto x : seq) {
        cout << x + 1 << ' ';
    }
    cout << '\n';

    return 0;
}
```

## 链式前向星

```cpp
#include <bits/stdc++.h>
using namespace std;

constexpr int N = 100010, M = N;

int n, m;
int h[N], e[M], ne[M], idx;
int q[N], ind[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bool topo() {
    int hh = 0, tt = -1;
    for (int i = 1; i <= n; i++) {
        if (ind[i] == 0) {
            q[++tt] = i;
        }
    }
    while (hh <= tt) {
        auto u = q[hh++];
        for (int i = h[u]; ~i; i = ne[i]) {
            int v = e[i];
            if (--ind[v] == 0) {
                q[++tt] = v;
            }
        }
    }
    return tt == n - 1; // or use: hh == n ( 判断出队点数 )
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    memset(h, -1, sizeof h);

    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        add(u, v);
        ind[v]++;
    }

    if (!topo()) {
        return cout << -1, 0;
    }

    for (int i = 0; i < n; i++) {
        cout << q[i] << ' ';
    }

    return 0;
}
```
