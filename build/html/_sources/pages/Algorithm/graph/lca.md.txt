# 倍增法求 LCA

预处理 O(nlog(n))​ 查询 ​O(log(n))​ 用 ​ST 表维护 dfs 序 可以做到 ​O(1) 查询，不过一般没必要。

```cpp
namespace LCA{
    int lg[500005];
    int dep[500005];
    int fa[500005][20];

    vector<int>G[500005];
    inline void init() {//@1 调用顺序
        for(int i = 2; i <= 500000; i++){
            lg[i] = lg[i>>1] + 1; //预处理log数组，用浮点数log可能会出错！
        }
    }

    inline void add(int u, int v) {
        G[u].push_back(v);
    }

    void dfs(int cur,int Fa){ //@2
        fa[cur][0] = Fa;
        dep[cur] = dep[Fa] + 1;
        for(int i = 1; i <= lg[dep[cur]]; i++){
            fa[cur][i] = fa[fa[cur][i-1]][i-1];
        }
        for(int v : G[cur]){
            if(v != Fa){
                dfs(v, cur);
            }
        }
    }
    
    int lca(int u, int v){ // @3
        if(dep[u] < dep[v]){
            swap(u, v);
        }
        while(dep[u] > dep[v]){
            u = fa[u][lg[dep[u] - dep[v]]];
        }
        if(u == v){
            return u;
        }
        for(int i = lg[dep[u]]; i>=0; i--){
            if(fa[u][i] != fa[v][i]){
                u = fa[u][i];
                v = fa[v][i];
            }
        }
        return fa[u][0];
    }
}
```