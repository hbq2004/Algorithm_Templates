# SegmentTree


## 普通线段树板子

无法区间合并——

```cpp
template<class Info>
struct SEG {
    int n;
    vector<Info> info;
    SEG(): n(0) {}
    SEG(int n_, Info v_ = Info()) {
        init(n_, v_);
    }
    template<class T>
    SEG(vector<T> init_) {
        init(init_);
    }
    void init(int n_, Info v_ = Info()) {
        init(vector(n_, v_));
    }
    template<class T>
    void init(vector<T> init_) {
        n = init_.size();
        info.assign(4 << __lg(n), Info());
        function<void(int, int, int)> build = [&](int u, int l, int r) {
            if (r - l == 1) {
                info[u] = init_[l];
                return;
            }
            int m = (l + r) >> 1;
            build(u << 1, l, m);
            build(u << 1 | 1, m, r);
            pull(u);
        };
        build(1, 0, n);
    }
    
    void pull(int u) {
    	info[u] = info[u << 1] + info[u << 1 | 1];
    }
    
    void modify(int u, int l, int r, int x, const Info &v) {
        if (r - l == 1) {
            info[u] = v;
            return;
        }
        int m = (l + r) >> 1;
        if (x < m) {
            modify(u << 1, l, m, x, v);
        } else {
            modify(u << 1 | 1, m, r, x, v);
        }
        pull(u);
    }
    void modify(int x, const Info &v) {
        modify(1, 0, n, x, v);
    }
    
    Info query(int u, int l, int r, int x, int y) {
        if (l >= y || r <= x) {
            return Info();
        }
        if (l >= x && r <= y) {
            return info[u];
        }
        int m = (l + r) >> 1;
        return query(u << 1, l, m, x, y) + query(u << 1 | 1, m, r, x, y);
    }
    Info query(int x, int y) {
        return query(1, 0, n, x, y);
    }
};

struct Info {
    // TODO: Information, Initialization

};

Info operator+(const Info &l, const Info &r) {
    Info u {};
    // TODO: Merge informations from bottom to top

};
```


## 带懒标记线段树


```cpp
template<class Info, class Tag>
struct SEG {
    int n;
    vector<Info> info;
    vector<Tag> tag;
    SEG(): n(0) {}
    SEG(int n_, Info v_ = Info()) {
        init(n_, v_);
    }
    template<class T>
    SEG(vector<T> init_) {
        init(init_);
    }
    void init(int n_, Info v_ = Info()) {
        init(vector(n_, v_));
    }
    template<class T>
    void init(vector<T> init_) {
        n = init_.size();
        info.assign(4 << __lg(n), Info());
        tag.assign(4 << __lg(n), Tag());
        function<void(int, int, int)> build = [&](int u, int l, int r) {
            if (r - l == 1) {
                info[u] = init_[l];
                return;
            }
            int m = (l + r) >> 1;
            build(u << 1, l, m);
            build(u << 1 | 1, m, r);
            pull(u);
        };
        build(1, 0, n);
    }

    void pull(int u) {
        info[u] = info[u << 1] + info[u << 1 | 1];
    }

    void apply(int u, const Tag &v) {
        info[u].apply(v);
        tag[u].apply(v);
    }
    void push(int u) {
        apply(u << 1, tag[u]);
        apply(u << 1 | 1, tag[u]);
        tag[u] = Tag();
    }

    void modify(int u, int l, int r, int x, const Info &v) {
        if (r - l == 1) {
            info[u] = v;
            return;
        }
        int m = (l + r) >> 1;
        push(u);
        if (x < m) {
            modify(u << 1, l, m, x, v);
        } else {
            modify(u << 1 | 1, m, r, x, v);
        }
        pull(u);
    }
    void modify(int x, const Info &v) {
        modify(1, 0, n, x, v);
    }

    Info query(int u, int l, int r, int x, int y) {
        if (l >= y || r <= x) {
            return Info();
        }
        if (l >= x && r <= y) {
            return info[u];
        }
        push(u);
        int m = (l + r) >> 1;
        return query(u << 1, l, m, x, y) + query(u << 1 | 1, m, r, x, y);
    }
    Info query(int x, int y) {
        return query(1, 0, n, x, y);
    }

    void Apply(int u, int l, int r, int x, int y, const Tag &v) {
        if (l >= y || r <= x) {
            return;
        }
        if (l >= x && r <= y) {
            apply(u, v);
            return;
        }
        push(u);
        int m = (l + r) >> 1;
        Apply(u << 1, l, m, x, y, v);
        Apply(u << 1 | 1, m, r, x, y, v);
        pull(u);
    }
    void Apply(int x, int y, const Tag &v) {
        return Apply(1, 0, n, x, y, v);
    }
};

struct Tag {
    // TODO: Lazy Tag

    Tag() {}
    Tag() {}
    void apply(const Tag &t) & {

    }
};

struct Info {
    // TODO: Information, Initialization
    
    void apply(const Tag &t) & {

    }
};

Info operator+(const Info &l, const Info &r) {
    Info u {};
    // TODO: Merge informations from bottom to top

    return u;
};
```