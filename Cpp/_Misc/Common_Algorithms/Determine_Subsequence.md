
# Determine Subsequence


```cpp
// Determine whether s is a subsequence of t

bool check(string &s, string &t) {
    int n = s.size(), m = t.size(); // (n <= m)

    int i = 0;
    for (auto c : t) {
        if (i < n && s[i] == c) {
            if (++i == n) {
                break;
            }            
        }
    }
    return i == n;
}
```