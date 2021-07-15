# 动态开点线段树

主席树思想，可以做到不离散化作1e9内数的权值线段树。

一. 查询区间和

```cpp
namespace DT{
    int root, tot;
    struct node{
        int ls, rs;
        int val;
    }tree[1000005];//两倍

    void push_up(int i) {
        int ls = tree[i].ls;
        int rs = tree[i].rs;
        tree[i].val = tree[ls].val + tree[rs].val;
    }

    int build() {
        ++tot;
        tree[tot].ls = tree[tot].rs = tree[tot].val = 0;
        return tot;
    }

    void init() {
        tot = 0;
        root = build();
    }

    void update(int i, int l, int r, int p, int val) {
        if (l == r) {
            tree[i].val += val;
            return;
        }
        int mid = (l + r)>>1;
        if (p <= mid) {
            if (!tree[i].ls) {
                tree[i].ls = build();
            }
            update(tree[i].ls, l, mid, p, val);
        } else {
            if (!tree[i].rs) {
                tree[i].rs = build();
            }
            update(tree[i].rs, mid + 1, r, p, val);
        }
        push_up(i);
    }
    int query(int i, int l, int r, int L, int R) {
        if (l > R || r < L) {
            return 0;
        }
        if (l >= L && r <= R) {
            return tree[i].val;
        }
        int mid = (l + r)>>1;
        int res = 0;
        if (tree[i].ls) {
            res += query(tree[i].ls, l, mid, L, R);
        }
        if (tree[i].rs) {
            res += query(tree[i].rs, mid + 1, r, L, R);
        }
        return res;
    }
}
```