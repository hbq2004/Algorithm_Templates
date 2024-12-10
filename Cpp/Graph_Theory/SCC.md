# Strongly Connected Components

Strongly Connected Components, SCC：强连通分量

```cpp
int h[N], e[M], ne[M], idx;
int dfn[N], low[N], cur;
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt;

void tarjan(int x) {
    dfn[x] = low[x] = ++cur;
    stk[++top] = x, in_stk[x] = true;
    for (int i = h[x]; ~i; i = ne[i]) {
        int y = e[i];
        if (!dfn[y]) {
            tarjan(y);
            low[x] = min(low[x], low[y]);
        } else if (in_stk[y]) {
            low[x] = min(low[x], dfn[y]);
        }
    }
    if (dfn[x] == low[x]) {
        scc_cnt++;
        int y;
        do {
            y = stk[top--];
            in_stk[y] = false;
            id[y] = scc_cnt;
            siz[scc_cnt]++;
        } while (y != x);
    }
}
```