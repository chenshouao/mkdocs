# Prim 算法求最小生成树

适合处理稠密图、特别是完全图。复杂度 O(n^2)​

```cpp
namespace Prim{
    int n;
    int mp[5005][5005];
    int d[5005];
    bool vis[5005];
    void add(int u, int v, int w) {
        mp[u][v] = min(mp[u][v], w);
    }
    long long prim() {
        long long sum = 0;
        memset(vis, 0, sizeof(vis));
        memset(d, 0x3f, sizeof(d));
        d[1] = 0;
        for (int i = 1; i <= n; ++i) {
            int k = 0;
            for (int j = 1; j <= n; ++j) {
                if (!vis[j] && (k == 0 || d[j] < d[k])) {
                    k = j;
                }
            }
            if (!k) {
                return -1; // Graph cant connect
            }
            vis[k] = 1;
            sum += d[k];
            for (int i = 1; i <= n; ++i) {
                d[i] = min(d[i], mp[k][i]);
            }
        }
        return sum;
    }
}
```