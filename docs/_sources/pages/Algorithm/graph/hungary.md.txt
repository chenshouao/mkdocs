# 匈牙利算法

时间复杂度O(nm)，求二分图最大匹配。

```cpp
namespace Hungary{
    int vis[1005];
    int match[1005];
    bool dfs(int u) {
        for (int v : G[u]) if(!vis[v]){
            vis[v] = 1;
            if (!match[v] || dfs(match[v])) {
                match[v] = u;
                return true;
            }
        }
        return false;
    }
    int solve() {
        int ans = 0;
        memset(match, 0, sizeof(match));
        for (int i = 1; i <= n; ++i) {
            memset(vis, 0, sizeof(vis));
            ans += dfs(i);
        }
        return ans;
    }
}
```