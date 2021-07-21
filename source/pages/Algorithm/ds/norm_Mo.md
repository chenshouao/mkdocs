# 普通莫队

求区间不同数的个数(可以排序询问右端点用树状数组做)

```cpp
#include<bits/stdc++.h>
using namespace std;
namespace Mo{
    const int MAXN = 30005, MAXQ = 200005, MAXM = 1000005;
    int sq;
    struct query {// 把询问以结构体方式保存
        int l, r, id;
        bool operator<(const query &o) const { // 重载<运算符，奇偶化排序
            // 这里只需要知道每个元素归属哪个块，而块的大小都是sqrt(n)，所以可以直接用l/sq
            if (l / sq != o.l / sq) {
                return l < o.l;
            }
            if (l / sq & 1) {
                return r < o.r;
            }
            return r > o.r;
        }
    }Q[MAXQ];
    int A[MAXN], ans[MAXQ], Cnt[MAXM], cur, l = 1, r = 0;
    inline void add(int p) {
        cur += (++Cnt[A[p]] == 1);
    }
    inline void del(int p) {
        cur -= (--Cnt[A[p]] == 0);
    }
}
int main() {
    int n; scanf("%d", &n);
    Mo::sq = sqrt(n);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", &Mo::A[i]);
    }
    int q; scanf("%d", &q);
    for (int i = 0; i < q; ++i) {
        scanf("%d%d", &Mo::Q[i].l, &Mo::Q[i].r);
        Mo::Q[i].id = i;
    } // 把询问离线下来

    sort(Mo::Q, Mo::Q + q); // 排序

    for (int i = 0; i < q; ++i) {
        while (Mo::l > Mo::Q[i].l) {
            Mo::add(--Mo::l);
        }
        while (Mo::r < Mo::Q[i].r) {
            Mo::add(++Mo::r);
        }
        while (Mo::l < Mo::Q[i].l) {
            Mo::del(Mo::l++);
        }
        while (Mo::r > Mo::Q[i].r) {
            Mo::del(Mo::r--);
        }
        Mo::ans[Mo::Q[i].id] = Mo::cur; // 储存答案
    }

    for (int i = 0; i < q; ++i) {
        printf("%d\n", Mo::ans[i]); // 按编号顺序输出
    }

    return 0;
}
```