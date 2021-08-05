# KMP单模式串匹配

```cpp
#include<bits/stdc++.h>
using namespace std;
int Next[1000005];
char s[1000005];
char t[1000005];
int n, m;
void get_next() {
    int now = 0, i = 2;//初始now = 0，从i = 2开始计算
    Next[1] = 0;
    while (i <= m) {
        if (t[now + 1] == t[i]) {//可直接拓展
            Next[i++] = ++now;
        } else if(now) {
            now = Next[now];//当now还没降到0就可以用Next数组继续降
        } else {
            Next[i++] = 0;//降到0 Next值就是0
        }
    }
}
void kmp() {
    int i = 1;
    int j = 1;
    while (i <= n) {
        if (s[i] == t[j]) {
            ++i;
            ++j;//匹配成功
        } else if(j != 1) {
            j = Next[j - 1] + 1; // j不等于1，根据Next移动标尺
        } else {
            ++i;//已经无法移动了，i右移一位
        }
        if (j == m + 1) {
            printf("%d\n", i - j + 1);
            j = Next[j - 1] + 1;//匹配成功，也根据Next移动标尺，找下一个可匹配位置
        }
    }
}
int main() {
    scanf("%s", s + 1);
    scanf("%s", t + 1);
    n = strlen(s + 1);
    m = strlen(t + 1);
    get_next();
    kmp();
    for (int i = 1; i <= m; ++i) {
        printf("%d ", Next[i]);
    }
    puts("");
    return 0;
}

```