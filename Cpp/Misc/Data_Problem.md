# Data Problem


Data Problem：日期问题

```cpp
constexpr int months[] = {
    0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31
};

int is_leap(int year) {
    return year % 100 && year % 4 == 0 || year % 400 == 0;
}

int get_month_days(int year, int month) {
    int res = months[month];
    if (month == 2) res += is_leap(year);
    return res;
}

int get_total_days(int y, int m, int d) {
    int res = 0;
    for (int i = 1; i < y; i ++ )
        res += 365 + is_leap(i);

    for (int i = 1; i < m; i ++ )
        res += get_month_days(y, i);

    return res + d;
}
```