# 动态开点线段树

主席树思想，可以做到不离散化作1e9内数的权值线段树。

一. 查询区间和，区间更新，带lazytag。

```cpp
namespace DT{
    int root, tot;
    struct node{
        int ls, rs;
        int val, lazy;
    }tree[1000005];//两倍
    int build() {
        ++tot;
        tree[tot].ls = tree[tot].rs = tree[tot].val = tree[tot].lazy = 0;
        return tot;
    }
    void init() {
        tot = 0;
        root = build();
    }
    void apply(int i, int val, int l, int r) {
        tree[i].val += val * (r - l + 1);
        tree[i].lazy += val;
    }
    void push_up(int i) {
        int ls = tree[i].ls;
        int rs = tree[i].rs;
        tree[i].val = tree[ls].val + tree[rs].val;
    }
    void push_down(int i, int l, int r) {
        if (!tree[i].ls) {
            tree[i].ls = build();
        }
        if (!tree[i].rs) {
            tree[i].rs = build();
        }
        int ls = tree[i].ls;
        int rs = tree[i].rs;
        int mid = (l + r)>>1;
        apply(ls, tree[i].lazy, l, mid);
        apply(rs, tree[i].lazy, mid + 1, r);
        tree[i].lazy = 0;
    }
    void update(int i, int l, int r, int L, int R, int val) {
        if (l > R || r < L) {
            return;
        }
        if (l >= L && r <= R) {
            apply(i, val, l, r);
            return;
        }
        push_down(i, l, r);
        int mid = (l + r)>>1;
        if (L <= mid) {
            if (!tree[i].ls) {
                tree[i].ls = build();
            }
            update(tree[i].ls, l, mid, L, R, val);
        }
        if (R > mid) {
            if (!tree[i].rs) {
                tree[i].rs = build();
            }
            update(tree[i].rs, mid + 1, r, L, R, val);
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
        push_down(i, l, r);
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

二. 多棵树合并。例：树链上加数，统计树上每一个点出现最多的数是多少。树上差分 + 动态开点线段树合并。

合并所有树的时间复杂度正比于所有线段树的结点个数和。

```cpp
namespace DT{
    int tot, n;
    int root[100005];//n棵树的根结点编号
    struct node{
        int ls, rs;
        int type;
        int val;
    }tree[10000005];//两倍
    void push_up(int i) {
        int ls = tree[i].ls;
        int rs = tree[i].rs;
        if (tree[ls].type == tree[rs].type) {
            tree[i].val = tree[ls].val + tree[rs].val;
            tree[i].type = tree[ls].type;
        } else {
            if (tree[ls].val == tree[rs].val) {
                tree[i].val = tree[ls].val;
                tree[i].type = min(tree[ls].type, tree[rs].type);
            } else if(tree[ls].val < tree[rs].val){
                tree[i].val = tree[rs].val;
                tree[i].type = tree[rs].type;
            } else {
                tree[i].val = tree[ls].val;
                tree[i].type = tree[ls].type;
            }
        }
    }
    int build() {
        ++tot;
        tree[tot].ls = tree[tot].rs = tree[tot].val = tree[tot].type = 0;
        return tot;
    }

    void init() {
        tot = 0;
        for (int i = 1; i <= n; ++i) {
            root[i] = build();//建n棵空树
        }
    }

    void update(int i, int l, int r, int p, int val) {
        if (l == r) {
            tree[i].val += val;
            tree[i].type = l;
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
    int Merge(int p, int q, int l, int r) {
        if (!p) {
            return q;
        }
        if (!q) {
            return p;
        }
        if (l == r) {
            tree[p].val += tree[q].val;
            return p;
        }
        int mid = (l + r)>>1;
        tree[p].ls = Merge(tree[p].ls, tree[q].ls, l, mid);
        tree[p].rs = Merge(tree[p].rs, tree[q].rs, mid + 1, r);
        push_up(p);
        return p;
    }
}
```