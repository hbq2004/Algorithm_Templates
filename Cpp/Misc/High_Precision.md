

# High Precision


High Precision：高精度



## 1. Print



```cpp
void print(vector<int> &res) {
    for (int i = res.size() - 1; i >= 0; i--) {
        cout << res[i];
    }
    cout << '\n';
}
```





## 2. Cmp



比较两个大数的大小



```cpp
bool cmp(vector<int> &a, vector<int> &b) { // a >= b
    if (a.size() != b.size()) {
        return a.size() > b.size();
    }
    for (int i = a.size() - 1; i >= 0; i--) {
        if (a[i] != b[i]) {
            return a[i] > b[i];
        }
    }
    return true; // a == b
}
```







## 3. Add



```cpp
vector<int> add(vector<int> &a, vector<int> &b) {
    if (a.size() < b.size()) {
        return add(b, a);
    }
	vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i++) {
        t += a[i];
        if (i < b.size()) {
            t += b[i];
        }
        c.push_back(t % 10);
        t /= 10;
    }
    if (t) {
        c.push_back(t);
    }
    return c;
}
```





## 4. Sub





```cpp
bool cmp(vector<int> &a, vector<int> &b) { // a >= b
    if (a.size() != b.size()) {
        return a.size() > b.size();
    }
    for (int i = a.size() - 1; i >= 0; i--) {
        if (a[i] != b[i]) {
            return a[i] > b[i];
        }
    }
    return true; // a == b
}

vector<int> sub(vector<int> &a, vector<int> &b) { // ensure a >= b
    vector<int> c;
    for (int i = 0, t = 0; i < a.size(); i++) {
        t = a[i] - t;
        if (i < b.size()) {
            t -= b[i];
        }
        c.push_back((t + 10) % 10);
		t = (t < 0 ? 1 : 0);
    }
    while (c.size() > 1 && c.back() == 0) {
        c.pop_back();
    }
    return c;
}

int main() {
    vector<int> a, b;
    vector<int> c;
    if (cmp(a, b)) {
        c = sub(a, b); // a - b
    } else {
        c = sub(b, a); // -(b - a)
    }
    
    return 0;
}
```





## 5. Mul





### 1. A × b



```cpp
vector<int> mul(vector<int> &a, LL b) {
    vector<int> c;
    LL t = 0;
    for (int i = 0; i < a.size() || t; i++) {
        if (i < a.size()) {
            t += a[i] * b;
        }
        c.push_back(t % 10);
        t /= 10;
    }
    while (c.size() > 1 && c.back() == 0) {
        c.pop_back();
    }
    return c;
}
```





### 2. A × B



```cpp
vector<int> mul(vector<int> &a, vector<int> &b) {
    int n = a.size(), m = b.size();
    vector<int> c(n + m);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            c[i + j] += a[i] * b[j];
        }
    }
    for (int i = 0, t = 0; i < c.size(); i++) {
        t += c[i];
        c[i] = t % 10;
        t /= 10;
    }
    while (c.size() > 1 && c.back() == 0) {
        c.pop_back();
    }
    return c;
}
```







## 6. Div









```cpp
vector<int> div(vector<int> &a, int b, int &r) {
    vector<int> c;
    r = 0;
    for (int i = a.size() - 1; i >= 0; i--) {
        r = r * 10 + a[i];
        c.push_back(r / b);
        r %= b;
    }
	reverse(c.begin(), c.end());
    while (c.size() > 1 && c.back() == 0) {
        c.pop_back();
    }
    return c;
}
```







