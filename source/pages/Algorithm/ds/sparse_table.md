# ST表

维护满足可重复覆盖性质的操作opt。(可重复覆盖性质即opt(x, x) = x)

```cpp
int nums[1000005];
int st[22][1000005];//The most cache-friendly version
int lg[1000005];
bool lg_initial;
void init_lg(){
    for(int i = 2; i <= 1000000; i++){
		lg[i] = lg[i>>1] + 1;
	}
}
void init() {
    if (!lg_initial) {
        lg_initial = 1;
        init_lg();
    }
    for (int i = 1; i <= n; ++i) {
        st[0][i] = nums[i];
    }
    for (int j = 1; (1<<j) <= n; ++j) {
        for (int i = 1; i + (1<<j) - 1 <= n; ++i) {
            st[j][i] = opt(st[j - 1][i], st[j - 1][i + (1<<(j - 1))]);            
        }
    }
}
int query(int l, int r) {
    int len = lg[r - l + 1];
    return opt(st[len][l], st[len][r - (1<<len) + 1]);
}
```