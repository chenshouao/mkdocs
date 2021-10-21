# 树上启发式合并

一般问题是:统计以 v 为根的子树中满足某些性质的节点数、统计一棵树中两节点权值与其LCA权值满足某些性质的节点对数。更改add、del和cal方法就能做到解决不同问题。另外，和 LCA 相关的问题一般先 cal 一整棵子树再add。其他的问题对子树的每个节点先 cal 再 add。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e5 + 5;

int n;

// G[u]: 存储与 u 相邻的结点
vector<int>G[N];

int sz[N], son[N], a[N], L[N], R[N], Node[N], totdfn;
int mp[1048576][17][2];
ll ans;
void add(int u) {
    for (int i = 0; i != 17; ++i) {
        ++mp[a[u]][i][(u>>i)&1];
    }
}
void cal(int u, int val) {
    for (int i = 0; i != 17; ++i) {
        int b = ((u>>i)&1);
        ans += (ll)mp[a[u] ^ val][i][!b] * (1<<i);
    }
}
void del(int u) {
    for (int i = 0; i != 17; ++i) {
        --mp[a[u]][i][(u>>i)&1];
    }
}

void pre_dfs(int u, int p) {
    L[u] = ++totdfn;
    Node[totdfn] = u;
    sz[u] = 1;
    for (int v : G[u]) if (v != p) {
        pre_dfs(v, u);
        sz[u] += sz[v];
        if (!son[u] || sz[son[u]] < sz[v]) {
            son[u] = v;
        }
    }
    R[u] = totdfn;
}

void dfs(int u, int p, bool keep) {
    // 计算轻儿子的答案
    for (int v : G[u]) if (v != p && v != son[u]) {
        dfs(v, u, false);
    }
    // 计算重儿子答案并保留计算过程中的数据（用于继承）
    if (son[u]) {
        dfs(son[u], u, true);
    }
    for (int v : G[u]) if (v != p && v != son[u]) {
        // 子树结点的 DFS 序构成一段连续区间，可以直接遍历
        for (int i = L[v]; i <= R[v]; i++) {
            cal(Node[i], a[u]);
        }
        for (int i = L[v]; i <= R[v]; i++) {
            add(Node[i]);
        }
    }
    cal(u, a[u]);
    add(u);
    if (keep == false) {
        for (int i = L[u]; i <= R[u]; i++) {
            del(Node[i]);
        }
    }
    //清除子树贡献
}

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
    }
    for (int i = 1; i < n; i++) {
        int u, v; scanf("%d%d", &u, &v);
        G[u].push_back(v);
        G[v].push_back(u);
    }
    pre_dfs(1, 0);
    dfs(1, 0, false);
    printf("%lld\n", ans);
    return 0;
}
 
```