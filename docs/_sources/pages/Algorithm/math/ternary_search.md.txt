# 三分法

## 整数三分

```cpp
double sub_ans(ll x) {
    //this is a function whose value was determine by x
    return ;
}
double ternary_search(ll l, ll r) {
	//求凹函数最小值
    double ans = 100000000000000;
    while (l < r) {
        ll m1 = (2 * l + r) / 3;
        ll m2 = (2 * r + l + 2) / 3;
        if (subans(m1) < subans(m2)) {
            ans = min(ans, subans(m1));
            r = m2 - 1;
        } else {
            ans = min(ans, subans(m2));
            l = m1 + 1;
        }
    }
    return ans;
}
double ternary_search(ll l, ll r) {
	//求凸函数最大值
    double ans = 100000000000000;
    while (l < r) {
        ll m1 = (2 * l + r) / 3;
        ll m2 = (2 * r + l + 2) / 3;
        if (subans(m1) > subans(m2)) {
            ans = min(ans, subans(m1));
            r = m2 - 1;
        } else {
            ans = min(ans, subans(m2));
            l = m1 + 1;
        }
    }
    return ans;
}
```

## 浮点数三分

```cpp
double ternary_search(double l, double r) {
	//求凸函数最大值
    double rmid, mid;
    while(fabs(r - l) > eps){
        mid = (l + r) / 2;
        rmid = (mid + r) / 2;
        if(subans(mid) > subans(rmid)){
            r = rmid;
        }else {
            l = mid;
        }
    }
    return subans(l);
}
```