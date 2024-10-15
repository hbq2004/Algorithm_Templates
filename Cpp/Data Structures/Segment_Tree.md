# SegmentTree

参考：[春春打飞舞 - 刚整理到的超超超超好用的 jiangly 线段树板子 - Acwing](https://www.acwing.com/file_system/file/content/whole/index/content/10936804/) 



## 普通线段树板子

```cpp
struct Info {
	// TODO : Information, Initialization
    
};

Info operator+(const Info &l, const Info &r) {
    Info u {};
	// TODO : Merge Information
    
	return u;
}

template<class Info>
struct SEG {
    const int n;
    vector<Info> info;
    SEG() {}
    SEG(int _n) : n(_n), info(4 << __lg(n)) {}
    SEG(const vector<Info> &init) : SEG(init.size()) {
		function<void(int, int, int)> build = [&](int u, int l, int r) {
            if (l == r) {
                info[u] = init[l - 1];
				return;
            }
            int m = (l + r) >> 1;
            build(u << 1, l, m), build(u << 1 | 1, m + 1, r);
            pull(u);
        };
        build(1, 1, n);
    }
    
    void pull(int u) {
    	info[u] = info[u << 1] + info[u << 1 | 1];
    }
    
    void modify(int u, int l, int r, int x, const Info &v) {
        if (l == r) {
            info[u] = v;
            return;
        }
        int m = (l + r) >> 1;
        if (x <= m) {
            modify(u << 1, l, m, x, v);
        } else {
            modify(u << 1 | 1, m + 1, r, x, v);
        }
        pull(u);
    }
    void modify(int x, const Info &v) {
        modify(1, 1, n, x, v);
    }
    
    Info query(int u, int l, int r, int x, int y) {
        if (l > y || r < x) {
            return Info();
        }
        if (l >= x && r <= y) {
            return info[u];
        }
        int m = (l + r) >> 1;
		return query(u << 1, l, m, x, y) + query(u << 1 | 1, m + 1, r, x, y);
    }
    Info query(int x, int y) {
        return query(1, 1, n, x, y);
    }
};
```


## 带懒标记线段树


```cpp
struct Tag {
    // TODO : Lazy Tag, Initialization
    
    void apply(Tag t) {
        // Update Lazy Tag
        
    }
};

struct Info {
    // TODO : Information, Initialization
    
    void apply(Tag t) {
        // Update Infomation
        
    }
};


Info operator+(const Info &l, const Info &r) {
    Info u {};
	// TODO : Merge Information
    
	return u;
}

template<class Info, class Tag>
struct SEG {
    const int n;
    vector<Info> info;
    vector<Tag> tag;
    SEG() {}
    SEG(int _n) : n(_n), info(4 << __lg(n)), tag(4 << __lg(n)) {}
    SEG(const vector<Info> &init) : SEG(init.size()) {
        function<void(int, int, int)> build = [&](int u, int l, int r) {
            if (l == r) {
                info[u] = init[l - 1];
                return;
            }
            int m = (l + r) >> 1;
            build(u << 1, l, m), build(u << 1 | 1, m + 1, r);
            pull(u);
        };
        build(1, 1, n);
    }
    
    void pull(int u) {
    	info[u] = info[u << 1] + info[u << 1 | 1];
    }
    void apply(int u, const Tag &v) {
		info[u].apply(v), tag[u].apply(v);
    }
    void push(int u) {
        apply(u << 1, tag[u]), apply(u << 1 | 1, tag[u]);
        tag[u] = Tag();
    }
    
    void modify(int u, int l, int r, int x, const Info &v) {
        if (l == r) {
            info[u] = v;
            return;
        }
        int m = (l + r) >> 1;
        if (x <= m) {
            modify(u << 1, l, m, x, v);
        } else {
            modify(u << 1 | 1, m + 1, r, x, v);
        }
        pull(u);
    }
    void modify(int x, const Info &v) {
        modify(1, 1, n, x, v);
    }
    
	void modify(int u, int l, int r, int x, int y, const Tag &v) {
        if (l > y || r < x) {
			return;
        }
        if (l >= x && r <= y) {
            apply(u, v);
            return;
        }
        push(u);
        int m = (l + r) >> 1;
        modify(u << 1, l, m, x, y, v), modify(u << 1 | 1, m + 1, r, x, y, v);
        pull(u);
    }
    void modify(int x, int y, const Tag &v) {
     	modify(1, 1, n, x, y, v);   
    }
    
    Info query(int u, int l, int r, int x, int y) {
        if (l > y || r < x) {
            return Info();
        }
        if (l >= x && r <= y) {
            return info[u];
        }
        push(u);
        int m = (l + r) >> 1;
		return query(u << 1, l, m, x, y) + query(u << 1 | 1, m + 1, r, x, y);
    }
    Info query(int x, int y) {
        return query(1, 1, n, x, y);
    }
};
```