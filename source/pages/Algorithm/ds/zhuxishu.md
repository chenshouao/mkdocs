# 可持久化线段树

求静态区间第K大数

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int n,m,N,cnt,a[200005],b[200005];
namespace ZXtree{
    int root[200005];
    struct node{
        int t;
        int rs;
        int ls;
    }tree[200005<<5];//一般开大30-40倍
    void push_up(int rt){
        tree[rt].t = tree[tree[rt].ls].t + tree[tree[rt].rs].t;//主席树不能根据下标直接确定儿子结点
    }
    int build(int l,int r){
        int rt = ++cnt;//动态开点
        if(l == r){
            tree[rt].t = 0;
            return rt;
        }
        int mid = (l + r)>>1;
        tree[rt].ls = build(l, mid);
        tree[rt].rs = build(mid + 1, r);
        push_up(rt);
        return rt;
    }
    int updata(int l,int r,int pos,int pre){
        int rt = ++cnt;//动态开点
        if(l == r){
            tree[rt].t = tree[pre].t + 1;
            return rt;
        }
        tree[rt].ls = tree[pre].ls;//新开的根节点先接在前一个结点儿子上
        tree[rt].rs = tree[pre].rs;
        int mid = (r+l)>>1;
        if(pos <= mid){
            tree[rt].ls = updata(l,mid,pos,tree[pre].ls);//如果要修改左儿子就要开一个新点当左儿子
        }else{
            tree[rt].rs = updata(mid + 1,r,pos,tree[pre].rs);//如果要修改右儿子就要开一个新点当右儿子
        }
        push_up(rt);
        return rt;
    }
    int query(int l,int r,int rt1,int rt2,int k){//两棵树一起递归下去
        if(l == r){
            return l;
        }
        int Ls1 = tree[rt1].ls;
        int Ls2 = tree[rt2].ls;
        int Rs1 = tree[rt1].rs;
        int Rs2 = tree[rt2].rs;
        int sum = tree[Ls2].t - tree[Ls1].t;
        int mid = (l + r)>>1;
        if(sum >= k){
            return query(l,mid,Ls1,Ls2,k);
        }else{
            return query(mid + 1,r,Rs1,Rs2,k - sum);//注意！这里是k-sum
        }
    }
}
void init(){//离散化
    cnt=0;
    for(int i=1;i<=n;++i){
        b[i]=a[i];
    }
    sort(b+1,b+1+n);
    N=unique(b+1,b+1+n)-(b+1);
    for(int i=1;i<=n;i++){
        a[i]=lower_bound(b+1,b+1+N,a[i])-b;
    }
}

int main() {
    scanf("%d%d",&n, &m);
    for(int i = 1; i <= n; ++i){
        scanf("%d", &a[i]);
    }
    init();
    ZXtree::root[0] = ZXtree::build(1, N);
    for(int i = 1; i <= n; ++i){
        ZXtree::root[i] = ZXtree::updata(1, N, a[i], ZXtree::root[i-1]);
    }
    for(int i = 1; i <= m; ++i){
        int l, r, k;
        scanf("%d%d%d",&l,&r,&k);
        printf("%d\n",b[ZXtree::query(1,N,ZXtree::root[l-1],ZXtree::root[r],k)]);//反离散化
    }
    return 0;
}
```