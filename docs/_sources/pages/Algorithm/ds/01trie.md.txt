# 01Trie树

带统计异或最大，统计值域内数的个数。

```cpp
namespace Trie {
    int nodenum = 0;
    int num[35];
    int val[35];
    struct node{
        int val, num;
        //还可以再加tag
        int next[2];
    }trie[10000005];//(100MB)

    void init() {
        for (int i = 0; i <= nodenum; ++i) {
            trie[i].val = trie[i].num = trie[i].next[0] = trie[i].next[1] = 0;
        }
        nodenum = 0;
    }


    void trans(int x) {
        int p = 31;
        while (p >= 1) {
            num[p--] = (x&1);
            x >>= 1;
        }
    }

    void Insert(int x, int val) {
        trans(x);
        int p = 0;
        for (int i = 1; i <= 31; ++i) {
            if(!trie[p].next[num[i]]) {
                trie[p].next[num[i]] = ++nodenum;
            }
            ++trie[p].num;
            p = trie[p].next[num[i]];
        }
        trie[p].val = val;//叶子节点存值
        ++trie[p].num;
    }
    
    int query(int x) {
        trans(x);
        int p = 0;
        for (int i = 1; i <= 31; ++i) {
            if(!trie[p].next[1 ^ num[i]]) {
                p = trie[p].next[num[i]];
            } else {
                p = trie[p].next[1 ^ num[i]];
            }
        }
        return trie[p].val;
    }

    int solve(int x) {
        trans(x);
        int p = 0;
        int res = 0;
        for (int i = 1; i <= 31; ++i) {
            if(num[i] && trie[p].next[0]) {
                res += trie[trie[p].next[0]].num;
            }
            if (trie[p].next[num[i]]) {
                p = trie[p].next[num[i]];
            } else {
                return res;
            }
        }
        return res + trie[p].num;
    }

    int Count(int l, int r) {
        if (l > r) {
            return 0;
        }
        return solve(r) - solve(l - 1);
    }
}
```

