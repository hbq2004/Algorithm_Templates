# Mediam

有关中位数的数据结构

## 对顶堆

- 普通对顶堆——

```cpp
priority_queue<int> down; // 大根堆
priority_queue<int, vector<int>, greater<int>> up; // 小根堆

int x; // 待插入的新元素
if (down.empty() || x <= down.top()) {
    down.push(x);
} else {
    up.push(x);
}

if (down.size() > up.size() + 1) { // 下面元素太多
    up.push(down.top());
    down.pop();
}
if (up.size() > down.size()) { // 上面元素太多
    down.push(up.top());
    up.pop();
}
```

- multiset 维护删数——

```cpp
multiset<int> down, up;

auto insert = [&](auto x) {
    if (down.empty() || x <= *down.rbegin()) {
        down.insert(x);
    } else {
        up.insert(x);
    }
};

auto erase = [&](auto x) {
    if (down.size() && *down.rbegin() >= x) {
        down.erase(down.lower_bound(x));
    } else {
        up.erase(up.lower_bound(x));
    }
};

auto adjust = [&]() {
    if (down.size() > up.size() + 1) {
        up.insert(*down.rbegin());
        down.erase(prev(down.end()));
    }
    if (up.size() > down.size()) {
        down.insert(*up.begin());
        up.erase(up.begin());
    }
};
```