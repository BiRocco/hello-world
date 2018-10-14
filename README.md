#include<iostream>
#include<cstdio>
#include<cstring>
#include<string>
#include<set>
#include<map>
#include<queue>
#include<stack>
using namespace std;
int temp = 0;
string g;
string target = "12345678x";
struct node
{
	string pre;
	char op;
};
map<string, node>mymap;
queue<string>myq;
set<string>check;
bool UP(string &g)//上移函数
{
	int pos;
	for (int i = 0; i < 9; i++)
	{
		if (g[i] == 'x')
		{
			pos = i;
			break;
		}
	}
	if (pos <= 2)return false;
	else
	{
		swap(g[pos], g[pos - 3]);
		return true;
	}
}

bool DOWN(string &g)//下移函数
{
	int pos;
	for (int i = 0; i < 9; i++)
	{
		if (g[i] == 'x')
		{
			pos = i;
			break;
		}
	}
	if (pos >= 6)
		return false;
	else
	{
		swap(g[pos], g[pos + 3]);
		return true;
	}
}

bool LEFT(string &g)//左移函数
{
	int pos;
	for (int i = 0; i < 9; i++)
	{
		if (g[i] == 'x')
		{
			pos = i;
			break;
		}
	}
	if (pos == 0 || pos % 3 == 0)
		return false;
	else
	{
		swap(g[pos], g[pos - 1]);
		return  true;
	}
}

bool RIGHT(string &g)//右移函数
{
	int pos;
	for (int i = 0; i < 9; i++)
	{

		if (g[i] == 'x')
		{
			pos = i;
			break;
		}
	}
	if (pos == 2 || pos == 5 || pos == 8)
		return  false;
	else
	{
		swap(g[pos], g[pos + 1]);
		return true;
	}
}

int judge(string &g,string &g2)
{
	int value = 0;
	for (int i = 0; i < 9; i++)
	{
		if (g[i] == g2[i])
			value++;
	}
	return value;
}

int bfs()
{
	while (!myq.empty())
		myq.pop();
	mymap.clear();
	check.clear();
	myq.push(g);
	check.insert(g);
	while (!myq.empty())
	{
		string last = myq.front();
		myq.pop();
		if (last == "12345678x")
			return 1;
		for (int i = 0; i < 4; i++)
		{
			string next = last;
			if (i == 0)
			{
				if (UP(next) && check.count(next) == 0)
				{
					if (judge(next,target) >= judge(last,target)) 
					{
						myq.push(next);
						check.insert(next);
						mymap[next].pre = last;
						mymap[next].op = 'U';
						temp++;
					}
					
				}
			}
			else if (i == 1)
			{
				if (DOWN(next) && check.count(next) == 0)
				{
					if (judge(next, target) >= judge(last, target))
					{
						myq.push(next);
						check.insert(next);
						mymap[next].pre = last;
						mymap[next].op = 'D';
						temp++;
					}
					
				}
			}
			else if (i == 2)
			{
				if (LEFT(next) && check.count(next) == 0)
				{
					if (judge(next, target) >= judge(last, target))
					{
						myq.push(next);
						check.insert(next);
						mymap[next].pre = last;
						mymap[next].op = 'L';
						temp++;
					}
				}
			}
			else if (i == 3)
			{
				if (RIGHT(next) && check.count(next) == 0)
				{
					if (judge(next, target) >= judge(last, target))
					{
						myq.push(next);
						check.insert(next);
						mymap[next].pre = last;
						mymap[next].op = 'R';
						temp++;
					}
				}
			}
		}

	}
	return 0;
}

int main()
{
	char c;
	for (int i = 0; i<9; i++)
	{
		cin >> c;
		g += c;
	}
	
	int ans = bfs();
	if (ans == 0)
		cout << "无解" << endl;
	else
	{
		string target = "12345678x";
		stack<char>OP;
		while (g != target)
		{
			OP.push(mymap[target].op);
			target = mymap[target].pre;
		}
		while (!OP.empty())
		{
			cout << OP.top();
			OP.pop();
		}
		cout<<temp<<endl;
	}
}
