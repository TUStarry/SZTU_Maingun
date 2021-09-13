## 拓展kmp

### 理解

z数组用于求当前串的后缀与当前串的最长公共前缀

ext数组用于求一个串T的后缀与另一个串P的最长公共前缀lcp

```C++
#include <iostream>
#include <string>
#include <cstdio>
using namespace std;
typedef long long ll;
const int maxn = 5e4 + 10;
ll z[maxn], ext[maxn];

// 求解z数组，模式串
void get_z(string s)
{
    ll r = 0, l = 0;
    z[0] = s.length();
    for (int i = 1; i < s.length(); i++)
    {
        if (i > r)
        {
            while (i + z[i] < s.length() && s[i + z[i]] == s[z[i]])
                z[i]++;
            l = i;
            r = i + z[i] - 1;
        }
        else if (z[i - l] >= r - i + 1) // 注意千千万是=，=代表还可以往后延伸，证明刚好是在边界
        {
            z[i] = r - i + 1;
            while (i + z[i] < s.length() && s[i + z[i]] == s[z[i]])
                z[i]++;
            l = i;
            r = i + z[i] - 1;
        }
        else
            z[i] = z[i - l];
    }
}

// 求解extend数组，文本串
void get_ext(string t, string p)
{
    get_z(p);
    ll l = 0, r = -1;
    for (int i = 0; i < t.length(); i++)
    {
        if (i > r)
        {
            while (i + ext[i] < t.length() && ext[i] < p.length() && t[i + ext[i]] == p[ext[i]])
                ext[i]++;
            l = i;
            r = i + ext[i] - 1;
        }
        else if (z[i - l] >= r - i + 1) // 注意千千万是=，=代表还可以往后延伸，证明刚好是在边界
        {
            ext[i] = r - i + 1;
            while (i + ext[i] < t.length() && ext[i] < p.length() && t[i + ext[i]] == p[ext[i]])
                ext[i]++;
            l = i;
            r = i + ext[i] - 1;
        }
        else
            ext[i] = z[i - l];
    }
}
```



### 压缩代码

```C++
#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 1e3 + 10;
int z[maxn], ext[maxn];

void get_z(string s)
{
    int l = 0, r = 0;
    z[0] = s.length();
    for (int i = 1; i < s.length(); i++)
    {
        if (i <= r)
            z[i] = min(z[i - l], r - i + 1); // 精髓
        while (i + z[i] < s.length() && s[z[i]] == s[i + z[i]])
            z[i]++;
        if (z[i] + i - 1 > r)
            l = i, r = z[i] + i - 1; // 更新区间
    }
}

void get_ext(string t, string p)
{
    get_z(p);
    int l = 0, r = -1;
    for (int i = 0; i < t.length(); i++)
    {
        if (i <= r)
            ext[i] = min(z[i - l], r - i + 1);
        while (i + ext[i] < t.length() && ext[i] < p.length() && t[i + ext[i]] == p[ext[i]])
            ext[i]++;
        if (i + ext[i] - 1 > r)
            l = i, r = i + ext[i] - 1;
    }
}
```

