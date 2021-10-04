# 线性预处理1 ~ n在模意义下的乘法逆元

```cpp
inv[0] = inv[1] = 1;
for (int i = 2; i <= 10000000; ++i) {
    inv[i] = (mod - mod / i) * inv[mod % i] % mod;
}
```