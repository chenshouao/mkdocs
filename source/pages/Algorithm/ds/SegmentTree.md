# 线段树

### 维护区间和。

```cpp
#define ls i<<1
#define rs i<<1|1
namespace Sgt{
    ll a[1000005];
    struct node{
        int l, r; // 该节点代表的区间端点
        ll sum;
        ll lazy;
    }tree[4000005];
    void push_up(int i) {
        tree[i].sum = tree[ls].sum + tree[rs].sum;
    }
    void apply(int i, int val) {
        tree[i].sum += (tree[i].r - tree[i].l + 1) * val;
        tree[i].lazy += val;
    }
    void push_down(int i) {
        if (!tree[i].lazy) return;
        apply(ls, tree[i].lazy);
		apply(rs, tree[i].lazy);
        tree[i].lazy = 0;
    }
    void build(int i, int l, int r) {
        tree[i].l = l;
        tree[i].r = r;
        tree[i].lazy = 0;
        if (l == r) {
            tree[i].sum = a[l];
            return;
        }
        int mid = (l + r) / 2;
        build(i * 2, l, mid);
        build(i * 2 + 1, mid + 1, r);
        push_up(i);
    }
    ll query(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return 0;//无影响值
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].sum;
        }
        push_down(i);
        return query(i * 2, l, r) + query(i * 2 + 1, l, r);
    }

    void update(int i, int l, int r, int val) {
        if (tree[i].l > r || tree[i].r < l) {
            return;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            apply(i, val);
            return;
        }
        push_down(i);
        update(i * 2, l, r, val);
        update(i * 2 + 1, l, r, val);
        push_up(i);
    }
}
```

### 维护区间最小值、最小值出现次数。

```cpp
#define ls i<<1
#define rs i<<1|1
namespace Sgt{
    struct node {
        int l,r;
        int lazy;
        int Min;
        int Sum;
    }tree[800005];
    void push_up(int i){
        tree[i].Min = min(tree[ls].Min, tree[rs].Min);
        tree[i].Sum = (tree[i].Min == tree[ls].Min)?tree[ls].Sum:0;
        tree[i].Sum += (tree[i].Min == tree[rs].Min)?tree[rs].Sum:0;
    }
    void apply(int i, int val) {
        tree[i].lazy += val;
        tree[i].Min += val;
    }
    void push_down(int i) {
        if (!tree[i].lazy) return;
        apply(ls, tree[i].lazy);
        apply(rs, tree[i].lazy);
        tree[i].lazy = 0;
    }
    void build(int i, int l, int r){
        tree[i].l = l;
        tree[i].r = r;
        tree[i].lazy = 0;
        if(l == r){
            tree[i].Min = a[l];
            tree[i].Sum = 1;
            return;
        }
        int mid = (l + r)>>1;
        build(ls, l, mid);
        build(rs, mid + 1, r);
        push_up(i);
    }
    void update(int i, int l, int r, int val){
        if(tree[i].r < l || r < tree[i].l){
            return;
        }
        if(l <= tree[i].l && tree[i].r <= r){
            apply(i, val);
            return;
        }
        push_down(i);
        update(ls, l, r, val);
        update(rs, l, r, val);
        push_up(i);
    }
    pair<int, int> query(int i, int l, int r) {
        if(tree[i].r < l || r < tree[i].l){
            return {2e9, 0};
        }
        if(l <= tree[i].l && tree[i].r <= r){
            return {tree[i].Min, tree[i].Sum};
        }
        push_down(i);
        auto p1 = query(ls, l, r);
        auto p2 = query(rs, l, r);
        if (p1.first == p2.first) {
            return {p1.first, p1.second + p2.second};
        } else if(p1.first < p2.first) {
            return {p1.first, p1.second};
        } else {
            return {p2.first, p2.second};
        }
    }
}
```