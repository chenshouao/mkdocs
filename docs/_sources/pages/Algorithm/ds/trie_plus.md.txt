# 可持久化Trie树

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int MAX = 600000 + 5;
int tot;
int root[MAX];
struct node{
    int ch[2];
    int val;
}trie[33 * MAX];
void Insert(int pre, int cur, int v) {
    for (int i = 28; i >= 0; i--) {
        trie[cur].val = trie[pre].val + 1;  // 在原版本的基础上更新
        if ((v & (1 << i)) == 0) {
            if (!trie[cur].ch[0]) {
                trie[cur].ch[0] = ++tot;
            }
            trie[cur].ch[1] = trie[pre].ch[1];
            cur = trie[cur].ch[0];
            pre = trie[pre].ch[0];
        } else {
            if (!trie[cur].ch[1]) {
                trie[cur].ch[1] = ++tot;
            }
            trie[cur].ch[0] = trie[pre].ch[0];
            cur = trie[cur].ch[1];
            pre = trie[pre].ch[1];
        }
    }
    trie[cur].val = trie[pre].val + 1;
}
int query(int pre, int cur, int v) {
    int ret = 0;
    for (int i = 28; i >= 0; i--) {
      int t = ((v & (1 << i)) ? 1 : 0);
      int p1 = trie[pre].ch[!t];
      int q1 = trie[pre].ch[t];
      int p2 = trie[cur].ch[!t];
      int q2 = trie[cur].ch[t];
      if (trie[p2].val - trie[p1].val) {
        ret += (1 << i);
        pre = p1;
        cur = p2;  // 尽量向不同的地方跳
      } else {
        pre = q1;
        cur = q2;
      }
    }
    return ret;
}
int nums[MAX];
int main() {
    int n, m; scanf("%d%d", &n, &m);
    root[0] = ++tot;
    Insert(0, root[0], 0);//插0
    for (int i = 1; i <= n; ++i) {
        scanf("%d", &nums[i]);
        nums[i] ^= nums[i - 1];
        root[i] = ++tot;
        Insert(root[i - 1], root[i], nums[i]);
    }
    while (m--) {
        char opt; scanf(" %c", &opt);
        if (opt == 'A') {
            ++n; scanf("%d", &nums[n]);
            nums[n] ^= nums[n - 1];
            root[n] = ++tot;
            Insert(root[n - 1], root[n], nums[n]);
        } else {
            int l, r, x; scanf("%d%d%d", &l, &r, &x);
            --l, --r;
            if (l == 0) {
                printf("%d\n", query(0, root[r], nums[n] ^ x));
                continue;
            }
            printf("%d\n", query(root[l - 1], root[r], nums[n] ^ x));
        }
    }
    return 0;
}
```