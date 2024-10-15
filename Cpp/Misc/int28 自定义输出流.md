
`__int128` 自定义输出流——

```
ostream &operator<<(ostream &os, i128 n) {
    string s;
    if (!n) {
        s = "0";
        return os << s;
    }
    while (n) {
        s += '0' + n % 10;
        n /= 10;
    }
    reverse(s.begin(), s.end());
    return os << s;
}
```