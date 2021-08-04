# 扫描线

线段树维护扫描线求矩形面积并 O(nlogn)

```cpp
#include<bits/stdc++.h>
#define ls i<<1
#define rs i<<1|1
using namespace std;
typedef long long ll;
int X1[100005];
int Y1[100005];
int X2[100005];
int Y2[100005];
int rmp[200005];
struct scanline {
	int l, r, h, val;
	friend bool operator < (scanline x, scanline y){
		return x.h < y.h;
	}
}line[200005];
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
        tree[i].Min = 0;
        tree[i].Sum = rmp[l + 1] - rmp[l];
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
int query() {
    return rmp[tree[1].r + 1] - rmp[tree[1].l] - (tree[1].Min == 0?tree[1].Sum:0);
}
int main() {
    int m = 0;
    ll ans = 0;
    set<int>st;
    unordered_map<int, int>mp;
	int n; scanf("%d",&n);
	for(int i = 1; i <= n; i++){
		scanf("%d%d%d%d", &X1[i], &Y1[i], &X2[i], &Y2[i]);
        st.insert(X1[i]);
        st.insert(X2[i]);
	}
	int tot = 0;
	for (int x : st) {
        mp[x] = ++tot;
        rmp[tot] = x;
	}
	for(int i = 1; i <= n; i++){
        line[++m] = {mp[X1[i]], mp[X2[i]] - 1, Y1[i], 1};
        line[++m] = {mp[X1[i]], mp[X2[i]] - 1, Y2[i], -1};
	}
	sort(line + 1, line + 1 + m);
	build(1, 1, tot - 1);
	for(int i = 1; i < m; i++){
		update(1, line[i].l, line[i].r, line[i].val);
		ans += (ll)query() * (line[i + 1].h - line[i].h);
	}
	printf("%lld\n",ans);
    return 0;
}
```