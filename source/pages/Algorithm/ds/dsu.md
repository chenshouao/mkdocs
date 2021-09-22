# 并查集

一. 按秩合并，路径压缩。

```cpp
namespace dsu{
    int n;
    int fa[1000005];
    int sz[1000005];
    void init() {
        for (int i = 1; i <= n; ++i) {
            fa[i] = i;
            sz[i] = 1;
        }
    }
    int Find(int x) {
        return fa[x] == x?x:fa[x] = Find(fa[x]);
    }
    void Union(int x, int y) {
        int fx = Find(x);
        int fy = Find(y);
        if (fx == fy) {
            return;
        }
        if (sz[fx] > sz[fy]) {
            swap(fx, fy);
        }
        sz[fy] += sz[fx];
        fa[fx] = fy;
    }
}
```

二. 边带权并查集

```cpp
namespace dsu{
    int n;
    int fa[1000005];
    int d[1000005];//权值
    void init() {
        for (int i = 0; i <= n; ++i) {
            fa[i] = i;
            d[i] = 0;
        }
    }
    int Find(int x) {
        if (x == fa[x]) {
            return x;
        }
        int root = Find(fa[x]); //递归计算祖宗
        d[x] += d[fa[x]]; //维护边权
        return fa[x] = root; //路径压缩
    }
    void Union(int x, int y, int w) {
        int fx = Find(x);
        int fy = Find(y);
        d[fy] = w - d[y] + d[x];
        fa[fy] = fx;
    }
}
```