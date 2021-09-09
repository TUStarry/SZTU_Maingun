## AC自动机

### trie图

```C++
#include<iostream>
#include<cstdio>
#include<string>
#include<queue>
using namespace std;
const int maxn = 1e6 + 10;
const int alp = 26;
int trie[maxn][alp];
int fail[maxn];
int tp = 0;

void insert(string s, int num)
{
	int now = 0;
	for (int i = 0; i < s.length(); i++)
	{
		int next = s[i] - 'a';
		if (trie[now][next] == 0)
			trie[now][next] = ++tp;
		now = trie[now][next];
	}
}

void getfail()
{
	queue<int>temp;
	for (int i = 0; i < alp; i++)
	{
		int u = trie[0][i];
		if (u)
		{
			temp.push(u);
			fail[u] = 0;
		}
	}

	while (!temp.empty())
	{
		int now = temp.front();
		temp.pop();
		for (int i = 0; i < alp; i++)
		{
			int u = trie[now][i];
			if (u)
			{
				temp.push(u);
				fail[u] = trie[fail[now]][i];
			}
			else
				trie[now][i] = trie[fail[now]][i];
		}
	}
}

void query(string s)
{
	int now = 0;
	for (int i = 0; i < s.length(); i++)
	{
		int next = s[i] - 'a';
		now = trie[now][next];
		for (int j = now; j; j = fail[j])
		{
			
		}
	}
}

int main()
{
	int n;
	scanf("%d", &n);
	for (int i = 0; i < n; i++)
	{
		string s;
		cin >> s;
		insert(s, i);
	}
	getfail();
	string s;
	cin >> s;
	query(s);
}
```



### 拓扑

```C++
// difficulty: 3
// AC自动机：字符串计数
#include<iostream>
#include<cstdio>
#include<queue>
#include<string>
using namespace std;
const int maxn = 2e5 + 10;
const int alp = 26;
int trie[maxn][alp] = { 0 };
int fail[maxn] = { 0 };
int id[maxn]; // 单词对应的最后一个字符编号
int times[maxn] = { 0 }; // 字母出现次数
int tp = 0;
string ss[maxn];
string sss;
int in[maxn];

void init() // 初始化
{
	for (int i = 0; i < alp; i++)
	{
		for (int j = 0; j <= tp; j++)
			trie[j][i] = 0;
	}

	for (int i = 0; i <= tp; i++)
		in[maxn] = times[i] = fail[i] = id[i] = 0;

	tp = 0;
}

void insert(int num, string s) // 字典树+编号
{
	int now = 0;
	for (int i = 0; i < s.length(); i++)
	{
		int next = s[i] - 'a';
		if (trie[now][next] == 0)
			trie[now][next] = ++tp;
		now = trie[now][next];
	}
	id[num] = now;
}

void getfail()
{
	queue<int> temp;
	for (int i = 0; i < alp; i++)
	{
		int u = trie[0][i];
		if (u)
		{
			temp.push(u);
			fail[u] = 0;
		}
	}

	while (!temp.empty())
	{
		int now = temp.front();
		temp.pop();
		for (int i = 0; i < alp; i++)
		{
			int u = trie[now][i];
			if (u)
			{
				temp.push(u);
				fail[u] = trie[fail[now]][i];
				in[trie[fail[now]][i]]++;
			}
			else
				trie[now][i] = trie[fail[now]][i];
		}
	}
}

void query(string s)
{
	int now = 0;
	for (int i = 0; i < s.length(); i++) // 仅剩下纵向查询
	{
		int next = s[i] - 'a';
		now = trie[now][next];
		times[now]++;
	}
}

// 通过fail指针形成的有向图，进行拓扑叠加
void topu()
{
	queue<int> temp;
	for (int i = 1; i <= tp; i++)
	{
		if (in[i] == 0)
			temp.push(i);
	}

	while (!temp.empty())
	{
		int now = temp.front();
		temp.pop();
		int next = fail[now];
		times[next] += times[now];
		in[next]--;
		if (in[next] == 0)
			temp.push(next);
	}
}

int main()
{
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> ss[i];
		insert(i, ss[i]);
	}
	getfail();
	cin >> sss;
	query(sss);
	topu();
	for (int i = 0; i < n; i++)
		cout << times[id[i]] << endl;
}
```

