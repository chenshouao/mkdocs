# tarjan求割点

```cpp
//求割点
int tot;
vector<int>G[100005];
inline void add(int u, int v) {
    G[u].push_back(v);
}

namespace Tarjan{
    int dfn[100005];
    int low[100005];
    int fa[100005];//fa[i] = i表示根节点
    int vis[100005];
    bool cut[100005];
    void tarjan(int u) {
        low[u] = dfn[u] = ++tot;
        vis[u] = 1;
        int child = 0;
        for (int v:G[u]) {
            if (!vis[v]) {
                ++child;
                fa[v] = u;
                tarjan(v);
                low[u] = min(low[u], low[v]);
                if (fa[u] != u && low[v] >= dfn[u]) {
                    cut[u] = 1;
                }
            } else {
                if (v != fa[u]) {
                    low[u] = min(low[u], dfn[v]);
                }
            }
        }
        if (fa[u] == u && child >= 2) {
            cut[u] = 1;
        }
    }
}
```