# 最小斯坦纳树

给n个点，m条带权边，k个关键点，求使得k个关键点连通的最小边权和。

时间复杂度O(n · 3^k + mlogm · 2^k)

```cpp
#include <bits/stdc++.h>
using namespace std;
int p[505],vis[505],dp[505][4200];
struct edge{
    int v, w;
};
vector<edge>G[505];
priority_queue<pair<int, int>>q;
void add_edge (int u, int v, int w) {
	G[u].push_back({v, w});
}
void dijkstra (int s) {
	memset(vis,0,sizeof(vis));
	while (!q.empty()) {
		pair<int, int> a = q.top(); q.pop();
		if (vis[a.second]) continue;
		vis[a.second]=1;
		for (const auto& e : G[a.second]) {
			if (dp[e.v][s] > dp[a.second][s] + e.w) {
				dp[e.v][s] = dp[a.second][s] + e.w;
				q.push({-dp[e.v][s], e.v});
			}
		}
	}
}
int main () {
	memset(dp,0x3f,sizeof(dp));
	int n, m, k; scanf("%d%d%d",&n,&m,&k);
	for (int i = 1; i <= m; i++) {
		int u, v, w; scanf("%d%d%d", &u, &v, &w);
		add_edge(u, v, w);
        add_edge(v, u, w);
	}
	for (int i = 1; i <= k; i++) {
		scanf("%d", &p[i]);//key-vertex
		dp[p[i]][1<<(i - 1)] = 0;
	}
	for (int s = 1; s < (1<<k); s++) {
		for (int i = 1; i <= n; i++) {
			for (int subs = s&(s-1); subs; subs=s&(subs-1)) {
				dp[i][s]=min(dp[i][s], dp[i][subs] + dp[i][s^subs]);
			}
			if (dp[i][s]!=0x3f3f3f3f) {
                q.push({-dp[i][s],i});
            }
		}
		dijkstra(s);
	}
	printf("%d\n",dp[p[1]][(1<<k)-1]);
	return 0;
}
```