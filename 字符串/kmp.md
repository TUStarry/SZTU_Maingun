## kmp

```C++
const int maxn = 1e6 + 10;
int nex[maxn];
void getnext(int len, char *s)
{
    int i = 0, j = -1;
    nex[0] = -1;
    while (i < len)
    {
        if (j == -1 || s[i] == s[j])
            nex[++i] = ++j;
        else
            j = nex[j];
    }
}

bool kmp(char *zhu, char *zi, int zhu_len, int zi_len)
{
    int i = 0, j = 0;
    while (i < zhu_len)
    {
        if (j == -1 || zhu[i] == zi[j])
        {
            i++;
            j++;
            if (j == zi_len)
                return true;
        }
        else
            j = nex[j];
    }
    return false;
}
```

