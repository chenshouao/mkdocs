# 可持久化线段树模板, 以区间第K大数为例

```cpp
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
```