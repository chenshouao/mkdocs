# 二维树状数组

## 单点修改，区间查询

```cpp
int n, m;
ll tree[5005][5005];
int lowbit(int x) {
    return x & -x;
}

ll ask(int x, int y) {
    ll res = 0;
    for (int i = x; i > 0; i -= lowbit(i)) {
        for (int j = y; j > 0; j -= lowbit(j)) {
            res += tree[i][j];
        }
    }
    return res;
}

void update(int x, int y, ll val) {
    for (int i = x; i <= n; i += lowbit(i)) {
        for (int j = y; j <= m; j += lowbit(j)) {
            tree[i][j] += val;
        }
    }
}

ll query(int a, int b, int c, int d) {
    return ask(c, d) - ask(a - 1, d) - ask(c, b - 1) + ask(a - 1, b - 1);
    //表示查询左上角为 (a, b) ，右下角为 (c, d) 的子矩阵内所有数的和
}
```

## 区间修改，单点查询

```cpp
int n, m;
ll tree[5005][5005];

int lowbit(int x) {
    return x & -x;
}

ll ask(int x, int y) {//询问(x, y)
    ll res = 0;

    for (int i = x; i > 0; i -= lowbit(i)) {
        for (int j = y; j > 0; j -= lowbit(j)) {
            res += tree[i][j];
        }
    }

    return res;
}

void revlse(int x, int y, ll val) {

    for (int i = x; i <= n; i += lowbit(i)) {
        for (int j = y; j <= m; j += lowbit(j)) {
            tree[i][j] += val;
        }
    }

}
void update(int a, int b, int c, int d, ll k) {
    revlse(a, b, k);
    revlse(a, d + 1, -k);
    revlse(c + 1, b, -k);
    revlse(c + 1, d + 1, k);//表示左上角为 (a, b) ，右下角为 (c, d) 的子矩阵内所有数都自增k
}
```

## 区间修改、区间查询

```cpp
int n, m;
ll tree[5005]5005], tree2[5005][5005], tree3[5005][5005], tree4[5005][5005];


int lowbit(int x) {
    return x & -x;
}

ll ask(int x, int y) {
    ll res = 0;

    for (int i = x; i > 0; i -= lowbit(i)) {
        for (int j = y; j > 0; j -= lowbit(j)) {
            res += 1LL * (x + 1) * (y + 1) * tree[i][j]
                   - 1LL * (y + 1) * tree2[i][j]
                   - 1LL * (x + 1) * tree3[i][j]
                   + 1LL * tree4[i][j];

        }
    }

    return res;
}

void revlse(int x, int y, ll val) {

    for (int i = x; i <= n; i += lowbit(i)) {
        for (int j = y; j <= m; j += lowbit(j)) {
            tree[i][j] += val;
            tree2[i][j] += val * x;
            tree3[i][j] += val * y;
            tree4[i][j] += val * x * y;
        }
    }

}
void update(int a, int b, int c, int d, ll k) {
    revlse(a, b, k);
    revlse(a, d + 1, -k);
    revlse(c + 1, b, -k);
    revlse(c + 1, d + 1, k);//表示左上角为 (a, b) ，右下角为 (c, d) 的子矩阵内所有数都自增k
}
ll query(int a, int b, int c, int d) {
    return ask(c, d) - ask(a - 1, d) - ask(c, b - 1) + ask(a - 1, b - 1);
    //表示查询左上角为 (a, b) ，右下角为 (c, d) 的子矩阵内所有数的和
}
```