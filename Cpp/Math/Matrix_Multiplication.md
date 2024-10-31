# Matrix Multiplication

矩阵乘法：Matrix Multiplication



```cpp
void mul(int c[][N], int a[][N], int b[][N]) { // c = a * b
    static int temp[N][N] {};
    memset(temp, 0, sizeof temp);
    
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            for (int k = 0; k < N; k++) {
                temp[i][j] = (temp[i][j] + 1LL * a[i][k] * b[k][j] % MOD) % MOD;
            }
        }
    }
    memcpy(c, temp, sizeof temp);
}
```