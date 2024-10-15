# Square Table

## 普通 ST 表

[Acwing 算法提高课-第六章-基础算法-RMQ](https://www.acwing.com/activity/content/16/) 

```cpp
void init() {
	for (int j = 0; j < M; j++) {
        for (int i = 1; i + (1 << j) - 1 <= n; i++) {
			if (!j) {
                f[i][j] = w[i];
            } else {
                f[i][j] = max(f[i][j - 1], f[i + (1 << j - 1)][j - 1]);
            }
        }
    }
}

int query(int l, int r) {
    int k = __lg(r - l + 1);
    return max(f[l][k], f[r - (1 << k) + 1][k]);
}
```

## 封装 ST 表

```cpp
struct RMQ {
    int n, m;
    vector<vector<int>> mx, mn, gd;
    
    RMQ() {}
    RMQ(const vector<int> &a) {
        init(a);
    }
    
    void init(const vector<int> &a) {
        n = a.size(), m = __lg(n);
        mx = vector<vector<int>>(n + 1, vector<int>(m + 1)); 
        mn = vector<vector<int>>(n + 1, vector<int>(m + 1));
        gd = vector<vector<int>>(n + 1, vector<int>(m + 1));
        
        for (int j = 0; j <= m; j++) {
            for (int i = 1; i + (1 << j) - 1 <= n; i++) {
                if (!j) {
                    mx[i][j] = mn[i][j] = gd[i][j] = a[i - 1];
                } else {
                    mx[i][j] = std::max(mx[i][j - 1], mx[i + (1 << (j - 1))][j - 1]);
                    mn[i][j] = std::min(mn[i][j - 1], mn[i + (1 << (j - 1))][j - 1]);
                    gd[i][j] = std::gcd(gd[i][j - 1], gd[i + (1 << (j - 1))][j - 1]);
                }
            }
        }
    }
    
    int max(int l, int r) {
        int k = __lg(r - l + 1);
        return std::max(mx[l][k], mx[r - (1 << k) + 1][k]);
    }

    int min(int l, int r) {
        int k = __lg(r - l + 1);
        return std::min(mn[l][k], mn[r - (1 << k) + 1][k]);
    }
    
    int gcd(int l, int r) {
     	int k = __lg(r - l + 1);
        return std::gcd(gd[l][k], gd[r - (1 << k) + 1][k]);
    }
};
```