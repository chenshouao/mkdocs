# 吉老师线段树

### 维护区间最大、次大，即可做到 logn 对区间取min。

```cpp
namespace Sgt{
    struct node{
        int l, r;
        int mx, se, cnt;
        ll sum;
        int lazy;
    }tree[4000005];
    void push_up(int i) {
        tree[i].sum = tree[ls].sum + tree[rs].sum;
        tree[i].mx = max(tree[ls].mx, tree[rs].mx);
        if (tree[ls].mx == tree[rs].mx) {
            tree[i].se = max(tree[ls].se, tree[rs].se);
            tree[i].cnt = tree[ls].cnt + tree[rs].cnt;
        } else if (tree[ls].mx > tree[rs].mx) {
            tree[i].se = max(tree[ls].se, tree[rs].mx);
            tree[i].cnt = tree[ls].cnt;
        } else {
            tree[i].se = max(tree[rs].se, tree[ls].mx);
            tree[i].cnt = tree[rs].cnt;
        }
    }
    void apply(int i, int val) {
        if (tree[i].mx > val && val > tree[i].se) {
            tree[i].sum += 1LL * (val - tree[i].mx) * tree[i].cnt;
            tree[i].mx = val;
            tree[i].lazy = val;
        }
    }
    void push_down(int i) {
        if (tree[i].lazy == -1) return;
        apply(ls, tree[i].lazy);
        apply(rs, tree[i].lazy);
        tree[i].lazy = -1;
    }
    void build(int i, int l, int r) {
        tree[i].l = l;
        tree[i].r = r;
        tree[i].lazy = -1;
        if (l == r) {
            scanf("%lld", &tree[i].sum);
            tree[i].mx = tree[i].sum;
            tree[i].cnt = 1;
            tree[i].se = -1;
            return;
        }
        int mid = (l + r) / 2;
        build(ls, l, mid);
        build(rs, mid + 1, r);
        push_up(i);
    }
    void update(int i, int l, int r, int val) {
        if (tree[i].l > r || tree[i].r < l || val >= tree[i].mx) {
            return;
        }
        if (tree[i].l >= l && tree[i].r <= r && val > tree[i].se) {
            apply(i, val);
            return;
        }
        push_down(i);
        update(ls, l, r, val);
        update(rs, l, r, val);
        push_up(i);
    }
    int query_max(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return 0;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].mx;
        }
        push_down(i);
        return max(query_max(ls, l, r), query_max(rs, l, r));
    }
    ll query_sum(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return 0;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].sum;
        }
        push_down(i);
        return query_sum(ls, l, r) + query_sum(rs, l, r);
    }
}
```

### 区间加、取max、取min、询问max、min、sum。

```cpp
namespace Sgt{
    struct node{
        int l, r;
        int mx1, mx2, mi1, mi2, mxnum, minum;
        int lazy;
        ll sum;
    }tree[2000005];
    void push_up(int i) {
        tree[i].sum = tree[ls].sum + tree[rs].sum;
        tree[i].mx1 = max(tree[ls].mx1, tree[rs].mx1);
        tree[i].mi1 = min(tree[ls].mi1, tree[rs].mi1);
        if (tree[ls].mx1 == tree[rs].mx1) {
            tree[i].mx2 = max(tree[ls].mx2, tree[rs].mx2);
            tree[i].mxnum = tree[ls].mxnum + tree[rs].mxnum;
        } else if (tree[ls].mx1 > tree[rs].mx1) {
            tree[i].mx2 = max(tree[ls].mx2, tree[rs].mx1);
            tree[i].mxnum = tree[ls].mxnum;
        } else {
            tree[i].mx2 = max(tree[rs].mx2, tree[ls].mx1);
            tree[i].mxnum = tree[rs].mxnum;
        }
        if (tree[ls].mi1 == tree[rs].mi1) {
            tree[i].mi2 = min(tree[ls].mi2, tree[rs].mi2);
            tree[i].minum = tree[ls].minum + tree[rs].minum;
        } else if (tree[ls].mi1 < tree[rs].mi1) {
            tree[i].mi2 = min(tree[ls].mi2, tree[rs].mi1);
            tree[i].minum = tree[ls].minum;
        } else {
            tree[i].mi2 = min(tree[rs].mi2, tree[ls].mi1);
            tree[i].minum = tree[rs].minum;
        }
    }
    void push_down(int i) {
        if(tree[i].lazy) {

            tree[ls].mx1 += tree[i].lazy , tree[ls].mx2 += tree[i].lazy;
            tree[ls].mi1 += tree[i].lazy , tree[ls].mi2 += tree[i].lazy;
            tree[ls].sum += (tree[ls].r - tree[ls].l + 1) * tree[i].lazy;
            tree[ls].lazy += tree[i].lazy;

            tree[rs].mx1 += tree[i].lazy , tree[rs].mx2 += tree[i].lazy;
            tree[rs].mi1 += tree[i].lazy , tree[rs].mi2 += tree[i].lazy;
            tree[rs].sum += (tree[rs].r - tree[rs].l + 1) * tree[i].lazy;
            tree[rs].lazy += tree[i].lazy;

            tree[i].lazy = 0;
        }
        if(tree[ls].mx1 > tree[i].mx1) {
            if(tree[ls].mi1 == tree[ls].mx1) {
                tree[ls].mi1 = tree[i].mx1;
            }
            if(tree[ls].mi2 == tree[ls].mx1) {
                tree[ls].mi2 = tree[i].mx1;
            }
            tree[ls].sum += 1ll * (tree[i].mx1 - tree[ls].mx1) * tree[ls].mxnum;
            tree[ls].mx1 = tree[i].mx1;
        }
        if(tree[rs].mx1 > tree[i].mx1) {
            if(tree[rs].mi1 == tree[rs].mx1) {
                tree[rs].mi1 = tree[i].mx1;
            }
            if(tree[rs].mi2 == tree[rs].mx1) {
                tree[rs].mi2 = tree[i].mx1;
            }
            tree[rs].sum += 1ll * (tree[i].mx1 - tree[rs].mx1) * tree[rs].mxnum;
            tree[rs].mx1 = tree[i].mx1;
        }
        if(tree[ls].mi1 < tree[i].mi1) {
            if(tree[ls].mx1 == tree[ls].mi1) {
                tree[ls].mx1 = tree[i].mi1;
            }
            if(tree[ls].mx2 == tree[ls].mi1) {
                tree[ls].mx2 = tree[i].mi1;
            }
            tree[ls].sum += 1ll * (tree[i].mi1 - tree[ls].mi1) * tree[ls].minum;
            tree[ls].mi1 = tree[i].mi1;
        }
        if(tree[rs].mi1 < tree[i].mi1) {
            if(tree[rs].mx1 == tree[rs].mi1) {
                tree[rs].mx1 = tree[i].mi1;
            }
            if(tree[rs].mx2 == tree[rs].mi1) {
                tree[rs].mx2 = tree[i].mi1;
            }
            tree[rs].sum += 1ll * (tree[i].mi1 - tree[rs].mi1)  * tree[rs].minum;
            tree[rs].mi1 = tree[i].mi1;
        }

    }
    void build(int i, int l, int r) {
        tree[i].l = l;
        tree[i].r = r;
        tree[i].lazy = 0;
        if (l == r) {
            scanf("%lld", &tree[i].sum);
            tree[i].mi1 = tree[i].mx1 = tree[i].sum;
            tree[i].minum = tree[i].mxnum = 1;
            tree[i].mi2 = 2e9;
            tree[i].mx2 = -2e9;
            return;
        }
        int mid = (l + r) / 2;
        build(ls, l, mid);
        build(rs, mid + 1, r);
        push_up(i);
    }
    void update_add(int i, int l, int r, int val) {
        if (tree[i].l > r || tree[i].r < l) {
            return;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            tree[i].sum += 1ll * (tree[i].r - tree[i].l + 1) * val;
            tree[i].mx1 += val;
            tree[i].mx2 += val;
            tree[i].mi1 += val;
            tree[i].mi2 += val;
            tree[i].lazy += val;
            return;
        }
        push_down(i);
        update_add(ls, l, r, val);
        update_add(rs, l, r, val);
        push_up(i);
    }
    void update_min(int i, int l, int r, int val) {
        if (tree[i].l > r || tree[i].r < l || val >= tree[i].mx1) {
            return;
        }
        if (tree[i].l >= l && tree[i].r <= r && val > tree[i].mx2) {
            if (tree[i].mi1 == tree[i].mx1) {
                tree[i].mi1 = val;
            }
            if (tree[i].mi2 == tree[i].mx1) {
                tree[i].mi2 = val;
            }
            tree[i].sum += 1ll * (val - tree[i].mx1) * tree[i].mxnum;
            tree[i].mx1 = val;
            return;
        }
        push_down(i);
        update_min(ls, l, r, val);
        update_min(rs, l, r, val);
        push_up(i);
    }
    void update_max(int i, int l, int r, int val) {
        if (tree[i].l > r || tree[i].r < l || val <= tree[i].mi1) {
            return;
        }
        if (tree[i].l >= l && tree[i].r <= r && val < tree[i].mi2) {
            if (tree[i].mx1 == tree[i].mi1) {
                tree[i].mx1 = val;
            }
            if (tree[i].mx2 == tree[i].mi1) {
                tree[i].mx2 = val;
            }
            tree[i].sum += 1ll * (val - tree[i].mi1) * tree[i].minum;
            tree[i].mi1 = val;
            return;
        }
        push_down(i);
        update_max(ls, l, r, val);
        update_max(rs, l, r, val);
        push_up(i);
    }
    ll query_sum(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return 0;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].sum;
        }
        push_down(i);
        return query_sum(ls, l, r) + query_sum(rs, l, r);
    }
    int query_max(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return -2e9;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].mx1;
        }
        push_down(i);
        return max(query_max(ls, l, r), query_max(rs, l, r));
    }
    int query_min(int i, int l, int r) {
        if (tree[i].l > r || tree[i].r < l) {
            return 2e9;
        }
        if (tree[i].l >= l && tree[i].r <= r) {
            return tree[i].mi1;
        }
        push_down(i);
        return min(query_min(ls, l, r), query_min(rs, l, r));
    }

}
```