# SPFA​ 算法

处理负权图的最短路算法。最坏复杂度 O(nm)​，但在一般图上复杂度​O(cn)​ (c是一个很小的常数)。

```cpp
namespace SPFA{
    struct edge{
        int v;
        int dis;
    }temp;
    int n,m,s;
    int dist[300005];
    int vis[300005]; // 是否在队列中
    int num[300005];
    vector<edge>G[300005];

    inline void add(int u, int v, int w) {
        G[u].push_back({v, w});
    }

    int spfa(int s){
        for (int i = 1; i <= n; ++i) {
            dist[i] = 2147483647;
        }
        memset(vis, 0, sizeof(vis));
        memset(num, 0, sizeof(num));
        dist[s] = 0;
        num[s] = 1;
        queue<int>q;
        q.push(s);
        vis[s] = 1;
        while(!q.empty()){
            int fa = q.front(); q.pop();
            vis[fa] = 0;
            for(edge e : G[fa]){
                if(dist[e.v] > (long long)dist[fa] + e.dis){
                    dist[e.v] = dist[fa] + e.dis;
                    if(!vis[e.v]) {
                        q.push(e.v);
                        vis[e.v] = 1;
                        num[e.v]++;
                        if(num[e.v] > n){
                            return 0; // negetive circle appear!
                        }
                    }
                }
            }
        }
        return 1;
    }
}
```