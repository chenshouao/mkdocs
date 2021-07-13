# 线性筛

可以 O(n)​ 求很多积性函数，例如求出每个数的最小素因子即可 ​O(log(n))​ 分解一个 ​1e6​ 以下的数。

```cpp
namespace sieve {
    int cnt;
    int pri[1000005];
    int nxt[1000005];
    bool vis[1000005];
    void init() {
        for (int i = 2; i < MAXN; ++i) {
            if (!vis[i]) {
                pri[cnt++] = i;
                nxt[i] = i;
            }
            for (int j = 0; j < cnt; ++j) {
                if (1ll * i * pri[j] >= MAXN) break;
                vis[i * pri[j]] = 1;
                nxt[i * pri[j]] = pri[j];
                if (i % pri[j] == 0) {
                    break;
                }
            }
        }
    }
}
```