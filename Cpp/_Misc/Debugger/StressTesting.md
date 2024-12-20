# StressTesting

StressTesting：对拍

## 前置配置

```cpp
using i64 = long long;
using i128 = __int128;
using u32 = unsigned int;
using PII = pair<int, int>;

constexpr int MOD = 998244353;
constexpr u32 umx = UINT_MAX; // random_float

// unsigned int, unsigned long long (#include <random>)
mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());
mt19937_64 rng_64(chrono::steady_clock::now().time_since_epoch().count());
```

## 生成随机数据 (gen)

- 生成随机数——

```cpp
i64 random(i64 n) { // [0, n - 1]
    return ((i128)rng() * rng() % n + MOD) % n;
}

i64 random_negative(i64 n) { // [-n, n]
    return random(2 * n + 1) - n;
}

double random_float(i64 n) { // [0, 1) * (n - 1) => [0, n - 1)
    return rng() / (double)(umx) * random(n);
}
```

- 生成随机树——

```cpp
void getTree(int n) { // Node 1 is the root as default
    for (int i = 2; i <= n; i++) {
        int p = random(i - 1) + 1;
        fout << p << ' ' << i << '\n'; // p -> i
    }
}
```

- 生成随机图——

```cpp
void getGraph(int n, int m) {
    set<PII> S;
    for (int i = 2; i <= n; i++) {
        int p = random(i - 1) + 1;
        fout << p << ' ' << i << '\n'; // (p, i);
        S.insert({p, i});
    }
    for (int i = 0; i < n - m + 1; i++) {
        int u, v;
        do {
            u = random(n) + 1, v = random(n) + 1;
        } while (u == v || S.count({u, v}));
        S.insert({u, v});
        fout << u << ' ' << v << '\n'; // (u, v)
    }
}
```

## 对拍程序

### Windows

```cpp
#include <iostream>
#include <algorithm>
#include <random>
#include <chrono>
#include <fstream>
#include <iomanip>
#include <cstring>
#include <set>

using namespace std;
using u32 = unsigned int;
using i64 = long long;
using i128 = __int128;
using PII = pair<int, int>;

constexpr int MOD = 998244353;
constexpr u32 umx = UINT_MAX;

mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

i64 random(i64 n) { // [0, n - 1]
    return ((i128)rng() * rng() % n + MOD) % n;
}

void generate_data() {
    ofstream fout("input.txt");

    fout.close();
}

int main() {
    for (int i = 1; i <= 1000; i ++ ) {
        printf("interation : %d\n", i);
        generate_data(); 
        // getchar();
        system("sol.exe < input.txt > sol_output.txt");
        system("std.exe < input.txt > std_output.txt");
        if (system("fc sol_output.txt std_output.txt")) {
            puts("Wrong Answer!");
            break;
        }
    }
    return 0;
}
```

### Linux

```sh
while true; do
    ./gen > data.in
    ./bf < data.in > std.out
    ./sol < data.in > sol.out
    if diff std.out sol.out; then
        echo ac
    else
        echo wa
        break
    fi
done
```