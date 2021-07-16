# 线段树

区间修改，区间查询。O(log(n))

```cpp
namespace SgT{
    struct node{
        int l, r;
        int val;
        int lazy;
    }tree[1000005];
    void push_up(int i) {
        tree[i].val = max(tree[ls].val, tree[rs].val);
    }
    void push_down(int i) {
        if (!tree[i].lazy) return;
        tree[ls].lazy += tree[i].lazy;
        tree[rs].lazy += tree[i].lazy;
        tree[ls].val += tree[i].lazy;
        tree[rs].val += tree[i].lazy;
        tree[i].lazy = 0;
    }
    void build(int i, int l, int r) {
        tree[i].l = l;
        tree[i].r = r;
        tree[i].lazy = 0;
        tree[i].val = 0;
        if (l == r) {
            return;
        }
        int mid = (l + r)>>1;
        build(ls, l, mid);
        build(rs, mid + 1, r);
    }
    void update(int i, int l, int r, int val) {
        if (tree[i].l > r || tree[i].r < l) {
            return;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            tree[i].val += val;
            tree[i].lazy += val;
            return;
        }
        push_down(i);
        update(ls, l, r, val);
        update(rs, l, r, val);
        push_up(i);
    }
    int query(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return 0;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].val;
        }
        push_down(i);
        return max(query(ls, l, r), query(rs, l, r));
    }
}
```