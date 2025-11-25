# Prompts

下面是我的代码风格（C++），你本次提供的代码要严格遵循这一风格：

使用如下模板：
```cpp
#define _USE_MATH_DEFINES
#include <bits/stdc++.h>

#define PI M_PI
#define E M_E

using namespace std;

mt19937 rnd(random_device{}());
int rndd(int l, int r){return rnd() % (r - l + 1) + l;}

using ll = long long;
using unll = unsigned long long;
using uint = unsigned int;
using ld = long double;

template < typename T = int >
inline T read(void){
    T ret(0);
    short flag(1);
    char c = getchar();
    while(c != '-' && !isdigit(c))c = getchar();
    if(c == '-')flag = -1, c = getchar();
    while(isdigit(c)){
        ret *= 10;
        ret += int(c - '0');
        c = getchar();
    }
    ret *= flag;
    return ret;
}



int main(){
    

    // fprintf(stderr, "Time: %.6lf\n", (double)clock() / CLOCKS_PER_SEC);
    return 0;
}
```

在符号（+, -, *, >>, =）等两侧均要有空格；

在类型嵌套中要遵循这样的格式：`vector < pair < int, int > > vec;`；

语句右括号之后不要有空格，且`{`不换行不加空格，如：
```cpp
for(int i = 1; i <= N; ++i){
	//codes
}
```

对于仅有一行，或可以用逗号连接的少量语句，不要换行，不要使用大括号，如：
```cpp
if(a == 1)b = 2, c = 3;
else if(a == 2)d = 4;
else c = 5;
```

所有函数以如下形式定义：
```cpp
auto Partition = [](vector < int > &A, int l, int r)->int{
    int val(A[r]);
    int spl(l - 1);
    for(int i = l; i <= r - 1; ++i)
        if(A[i] <= val)swap(A[++spl], A[i]);
    swap(A[++spl], A[r]);
    return spl;
};
```

对于常规输入的 $N, M, Q$ 等，均使用大写字母。

字符串的 $S, T$ 等也使用大写字母。

单字母数组名也尽量使用大写字母，如 `vector < int > A`.

函数命名采用驼峰命名法且首字母大写。变量命名采用驼峰命名法且首字母小写。

如果要使用预初始化数组的时候，长度溢出10，如：
```cpp
vector < vector < int > > dp(N + 10, vector < int >(M + 10, 0));
```

使用 `long long`, `unsigned` 的时候，直接使用我模板里 `using` 的 `ll` 和 `uint` 等。

对于乘以2和除以2，尽量使用`<<`, `>>`。

前缀和命名为 `sum[]`。

定义变量初始化时，如果表达式简单，使用括号初始化，如 `int res(0);`.

`INF` 按需使用 `0x3f3f3f3f` 或者 `INT_MAX >> 1` 这样的数值。

对于形如:

```cpp
if(dpMn[i][k] + dpMn[k + 1][j] + w < dpMn[i][j])
     dpMn[i][j] = dpMn[i][k] + dpMn[k + 1][j] + w;
```

也不要使用这样的数组定义：
```cpp
    ll dpMn[405][405];
    ll dpMx[405][405];
```

直接使用 `vetcor`.

输出时直接使用 `printf` 输出，仅使用 `string` 等特殊情况使用 `cout` 输出

直接用 `max` 或 `min` 函数代替。

在可省略的时候尽量不要使用 `static`。

对于大的数据结构，将其封装到 `class` 中，无需特别严格，例子：

```cpp
class Node{
public:
    Node *ls, *rs;
    int val, siz, cnt;
};

Node* root;

#define siz(p) ((p) ? (p)->siz : 0)
class Tree{
private:
public:
    void Pushup(Node* p){
        if(!p)return;
        p->siz = siz(p->ls) + siz(p->rs) + p->cnt;
    }
    Node* QueryMx(Node* p = root){
        if(!p)return p;
        if(p->rs)return QueryMx(p->rs);
        return p;
    }
    Node* QueryMn(Node* p = root){
        if(!p)return p;
        if(p->ls)return QueryMn(p->ls);
        return p;
    }
    Node* Insert(int val, Node* p = root){
        if(!p)return new Node{nullptr, nullptr, val, 1, 1};
        if(val < p->val)p->ls = Insert(val, p->ls);
        else if(val > p->val)p->rs = Insert(val, p->rs);
        else ++p->cnt;
        Pushup(p);
        return p;
    };
    Node* Delete(int val, Node* p = root){
        if(!p)return p;
        if(val < p->val)p->ls = Delete(val, p->ls);
        else if(val > p->val)p->rs = Delete(val, p->rs);
        else{
            if(p->cnt > 1)--p->cnt;
            else{
                if(!p->ls)delete exchange(p, p->rs);
                else if(!p->rs)delete exchange(p, p->ls);
                else{
                    auto succ = QueryMn(p->rs);
                    p->val = succ->val, p->cnt = succ->cnt;
                    succ->cnt = 1;
                    p->rs = Delete(succ->val, p->rs);
                }
            }
        }Pushup(p); return p;
    }
    int QueryRnk(int val, Node* p = root){
        if(!p)return 0;
        if(val == p->val)return siz(p->ls);
        if(val < p->val)return QueryRnk(val, p->ls);
        return siz(p->ls) + p->cnt + QueryRnk(val, p->rs);
    }
    Node* QueryByRnk(int rnk, Node* p = root){
        if(!p)return p;
        // printf("l siz = %d, rnk = %d, cnt = %d\n", siz(p->ls), rnk, p->cnt); fflush(stdout);
        if(siz(p->ls) + 1 <= rnk && rnk <= siz(p->ls) + p->cnt)return p;
        if(rnk <= siz(p->ls))return QueryByRnk(rnk, p->ls);
        return QueryByRnk(rnk - siz(p->ls) - p->cnt, p->rs);
    }
    Node* QuerySuc(int val, Node* p = root){
        if(!p)return p;
        if(val >= p->val)return QuerySuc(val, p->rs);
        auto res = QuerySuc(val, p->ls);
        return res ? res : p;
    }
    Node* QueryPre(int val, Node* p = root){
        if(!p)return p;
        if(val <= p->val)return QueryPre(val, p->ls);
        auto res = QueryPre(val, p->rs);
        return res ? res : p;
    }
}tr;
}tr;
```

对于一般的结构体，不需要构造函数，直接使用：
```cpp
new Node{nullptr, nullptr, val, 1, 1, 1}
```

并且不要是使用函数，直接在代码段中写 `p = new Node{...}` 即可；

多行赋值等简短语句，可以直接逗号连接置于一行：

```cpp
y->ls = x, x->rs = T2;
```

多个 `if-else` 相同格式且可替换时，直接使用 `switch-case`：

```cpp
switch(opt){
        case 1: root = tr.Insert(val); break;
        case 2: root = tr.Delete(val); break;
        case 3: printf("%d\n", tr.QueryRnk(val) + 1); break;
        case 4: printf("%d\n", tr.QueryByRnk(val)->val); break;
        case 5: printf("%d\n", tr.QueryPre(val)->val); break;
        case 6: printf("%d\n", tr.QuerySuc(val)->val); break;
    }
```

`size` 可以缩写成 `siz`，`height` 不要缩写成 `hgt`。

`max` 和 `min` 分别缩写成 `mx`, `mn`。

默认不要关同步，读入整形直接使用我的 `read()`，如 `int N = read();`.

应用我模板中的所有 `using`，如 `uint`, `ll`。

把所有形如这样的 `delete`，都使用 `exchange` 替换：

```cpp
Node* old = p;
p = tmp;
delete old;
```

```cpp
delete exchange(p, p->ls);
```

