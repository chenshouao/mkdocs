# Dijkstra 算法

最稳定的最短路算法，复杂度 O((n + m)log(n))​ 。注意：不能处理带负权图！

数据较弱时去掉 vis 数组的操作可能通过带负权的图，不过退化为了类 SPFA​ 算法。

```cpp
namespace Dij{
    long long dist[300005];
    int vis[300005];
    
    struct edge{
        int v;
        long long w;
    };
    vector<edge>G[300005];
    struct node{
        int v;
        long long w;
        friend bool operator < (node a, node b) {
            return a.w > b.w;
        }
    }fa, son;
    
    inline void add(int u, int v, int w) {
        G[u].push_back({v, w});
    }
    
    inline void dijkstra(int s) {
        memset(vis, 0, sizeof(vis));
        memset(dist, 0x3f, sizeof(dist));
        priority_queue<node>q;
        dist[s] = 0;
        q.push({s, 0});
        while (!q.empty()) {
            fa = q.top();
            q.pop();
            if (vis[fa.v])continue;
            vis[fa.v] = 1;
            for (edge e:G[fa.v]) {
                if (fa.w + e.w < dist[e.v]) {
                    dist[e.v] = fa.w + e.w;
                    q.push({e.v, dist[e.v]});
                }
            }
        }
    }
}
```

