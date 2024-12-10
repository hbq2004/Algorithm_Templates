# Trie


## 字符串 Trie


```cpp

```


## 01-Trie


```cpp
struct Trie {
    // Data Range <= 2^30 - 1 = 1'073'741'823 = 1e9
    int tr[N * 30][2], cnt[N * 30], idx = 0; 

    void clear() {
        for (int i = 0; i <= idx; i++){
            memset(tr[i], 0, sizeof tr[i]);
            cnt[i] = 0;
        }
        idx = 0;
    }

    void add(int x, int v) {
        for (int i = 29, p = 0; i >= 0; i--) {
            int u = (x >> i & 1);
            if (!tr[p][u]){
                tr[p][u] = ++idx;
            }
            p = tr[p][u];
            cnt[p] += v;
        }
    }

    int query(int x) {
        int ans = 0;
        for (int i = 29, p = 0; i >= 0; i--) {
            int u = (x >> i & 1);
            if (cnt[tr[p][u ^ 1]] > 0) {
                ans += 1 << i;
                p = tr[p][u ^ 1];
            }
            else {
                p = tr[p][u];
            }
        }
        return ans;
    }
} trie;
```