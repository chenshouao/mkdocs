# 求树的直径

1)  树上DP求直径长度。时间复杂度O(n)。

```cpp
int n, ans;
int d[300005];
int vis[300005];
struct edge{
    int v, w;
};

vector<edge>G[300005];

inline void add(int u, int v, int w) {
    G[u].push_back({v, w});
}

void dp(int u) {
    vis[u] = 1;
    for (edge e : G[u]) if(!vis[e.v]) {
        dp(e.v);
        ans = max(d[u] + d[e.v] + e.w, ans);
        d[u] = max(d[u], d[e.v] + e.w);
    }
}

int get_diameter() {
    memset(vis, 0, sizeof(vis));
    memset(d, 0, sizeof(d));
    dp(1);
    return ans;
}
```

2) 2次BFS求树的直径。树上边权必须非负！

```cpp
int n;
int d[300005];
vector<int>G[300005];

inline void add(int u, int v) {
    G[u].push_back(v);
}

int bfs(int s) {
    memset(d, 0xff, sizeof(d));
    queue<int>q;
    q.push(s); d[s] = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : G[u]) if (d[v] == -1) { 
            d[v] = d[u] + 1;
            q.push(v);
        }
    }
    int p = s;
    for (int i = 1; i <= n; ++i) if (d[i] > d[p]) {
        p = i;
    }
    return p;
}

int get_diameter() {
    int p = bfs(1);
    int q = bfs(p);
    return d[q];
}
```

