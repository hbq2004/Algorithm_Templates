
# Bipartite Graph Decision

Bipartite Graph Decision：二分图的判定


[AcWing 860. 染色法判定二分图](https://www.acwing.com/activity/content/problem/content/926/) 


```cpp
#include <bits/stdc++.h>
using namespace std;

constexpr int N = 100010, M = 100010 * 2;

int n, m;
int h[N], e[M], ne[M], idx;
int col[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bool dfs(int u, int c) {
    col[u] = c;
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];
        if (!col[v] && !dfs(v, 3 - c) || col[v] == c) {
            return false;
        }
    }
    return true;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    memset(h, -1, sizeof h);

    cin >> n >> m;
    while (m--) {
        int u, v;
        cin >> u >> v;
        add(u, v), add(v, u);
    }

    for (int i = 1; i <= n; i++) {
        if (!col[i] && !dfs(i, 1)) {
            return cout << "No\n", 0;
        }
    }

    cout << "Yes\n";

    return 0;
}
```