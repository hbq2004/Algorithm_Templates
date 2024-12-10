# Segment_Merge

Segment Merge：区间合并


```cpp
using PII = pair<int, int>

void merge(vector<PII> &segs) {
    vector<PII> res;
    sort(segs.begin(), segs.end());
    int st = -2e9, ed = -2e9;
    for (auto [l, r] : segs) {
        if (l > ed) {
            if (st != -2e9) {
                res.emplace_back(st, ed);
            }
            st = l, ed = r;
        } else {
            ed = max(ed, r);
        }
    }
    if (st != -2e9) {
        res.emplace_back(st, ed);
    }
    segs = res;
}
```