## 最长上升子序列(LIS)

### 二分+栈模拟 $O(nlogn)$​

只能求长度，不能反应是什么，采取了贪心的反悔机制

```C++
int st[maxn];

int bound(int l, int r, int num)
{
    while (l != r)
    {
        int mid = l + r >> 1;
        if (st[mid] >= num)
            r = mid;
        else
            l = mid + 1;
    }
    return l;
}

int findd(int l, int r)
{
    int tp = 0;
    for (int i = l; i <= r; i++)
    {
        if (tp == 0 || st[tp - 1] < num[i])
            st[tp++] = num[i];
        else
        {
            int way = bound(0, tp - 1, num[i]);
            st[way] = num[i];
        }
    }
    return tp;
}
```



### $dp$​​ $O(n^2)$​​​

记忆搜索，每个位置的$dp$记录的是当前位置最大的上升子序列

```C++
int num[maxn];

int main()
{
    for (int i = 0; i < len; i++)
        dp[i] = 1;

    for (int i = 0; i < len; i++)
        for (int j = 0; j < i; j++)
            if (num[i] <= num[j])
                dp[i] = max(dp[i], dp[j] + 1);
}
```



