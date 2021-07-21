# 分块

n*sqrt(n) 维护询问和修改

```cpp
namespace Block{
    const int maxn = 40005;
    const int Sq = 205;
    int st[Sq], ed[Sq], siz[Sq];
    int bel[maxn];
    void init_block(int n) {
        int sq = sqrt(n);
        for (int i = 1; i <= sq; ++i) {
            st[i] = n / sq * (i - 1) + 1;
            ed[i] = n / sq * i;
        }
        ed[sq] = n;
        for (int i = 1; i <= sq; ++i) {
            for (int j = st[i]; j <= ed[i]; ++j) {
                sum[i] += a[j];
                bel[j] = i;
            }
        }
        for (int i = 1; i <= sq; ++i) {
            siz[i] = ed[i] - st[i] + 1;
        }
    }
    void update(int l, int r, ll k) {
        if (bel[l] == bel[r]) {
            for (int i = l; i <= r; ++i){
                //零散块暴力操作
            }
        } else {
            for (int i = l; i <= ed[bel[l]]; ++i){
                //零散块暴力操作
            }
            for (int i = st[bel[r]]; i <= r; ++i){
                //零散块暴力操作
            }
            for (int i = bel[l] + 1; i < bel[r]; ++i) {
            	//整块暴力操作
        	}
        }
    }
    ll query(int l, int r) {
        ll res = 0;
        if (bel[l] == bel[r]) {
            for (int i = l; i <= r; ++i) {
                //零散块暴力操作
            }
        } else {
            for (int i = l; i <= ed[bel[l]]; ++i) {
                //零散块暴力操作
            }
            for (int i = st[bel[r]]; i <= r; ++i) {
                //零散块暴力操作
            }
            for (int i = bel[l] + 1; i < bel[r]; ++i) {
                //整块暴力操作
            }
        }
        return res;
    }
}
```