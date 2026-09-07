
# Skill: Generate C++ Code in tsawke Competitive-Programming Style

## 1. Overall Style

The code should look like concise competitive-programming code written for algorithm contests.

Main characteristics:

- Use C++17-style code.
- Prefer compact but readable implementation.
- Avoid excessive abstraction unless it helps the algorithm.
- Use STL heavily, especially `vector`, `pair`, `tuple`, `priority_queue`, `stack`, `list`, `set`, `unordered_map`, `basic_string`.
- Prefer manual fast input using `getchar()`.
- Prefer `printf()` for output.
- Use 1-indexing in most algorithmic arrays.
- Allocate extra buffer space such as `N + 10`, `M + 10`.
- Keep comments sparse.
- Debug comments are usually left as commented-out `printf` or `fprintf(stderr, ...)`.
- Do not over-polish the code into a production style. It should still feel like contest code.

---

## 2. Default Template

Always start from the following default template unless the problem clearly requires something special.

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
using i128 = __int128_t;

template < typename T = int > inline T read(void){
    T ret(0);
    short flag(1);
    char c = getchar();
    while(c != '-' && !isdigit(c))c = getchar();
    if(c == '-')flag = -1, c = getchar();
    while(isdigit(c)){
        ret *= 10;
        ret += int(c - '0');
        c = getchar();
    }ret *= flag;
    return ret;
}



int main(){
    

    // fprintf(stderr, "Time: %.6lf\n", (double)clock() / CLOCKS_PER_SEC);
    return 0;
}
~~~

------

## 3. Formatting Style

### 3.1 Indentation

Use 4 spaces for indentation.

```cpp
for(int i = 1; i <= N; ++i){
    int x = read();
    printf("%d\n", x);
}
```

### 3.2 Braces

Opening braces stay on the same line.

```cpp
int main(){
    return 0;
}
```

For short functions or branches, one-line bodies are acceptable.

```cpp
int Find(int x){return x == fa[x] ? x : fa[x] = Find(fa[x]);}
if(x == y)return false;
```

### 3.3 Spaces Around Template Brackets

Use spaces inside STL template angle brackets.

Preferred:

```cpp
vector < int > A;
vector < vector < int > > dp;
priority_queue < pair < int, int > > q;
```

Avoid:

```cpp
vector<int> A;
vector<vector<int>> dp;
```

### 3.4 Function and Lambda Return Type

For lambdas, explicitly write the trailing return type when the lambda is non-trivial.

```cpp
auto Heuristic = [&](void)->int{
    int ret(0);
    return ret;
};
```

For recursive lambdas, use `auto&& self`.

```cpp
auto dfs = [&](auto&& self, int p)->void{
    for(auto i = head[p]; i; i = i->nxt)
        if(i->to != fa[p])fa[i->to] = p, self(self, i->to);
};
```

------

## 4. Naming Style

### 4.1 Variables

Use short contest-style variable names.

Common names:

- `N`, `M`, `T`, `Q`
- `i`, `j`, `k`, `t`
- `s`, `t`, `x`, `y`, `p`
- `cur`, `res`, `ret`, `idx`
- `fa`, `siz`, `cap`, `head`
- `mp`, `freq`, `timeline`
- `curp`, `curr`, `todos`

Examples:

```cpp
int N = read(), M = read();
ll cur(0), res(-1);
vector < int > fa(N + 1, 0);
```

### 4.2 Classes and Methods

Classes use PascalCase or all-caps abbreviations when common in competitive programming.

Examples:

```cpp
class DSU{};
struct Edge{};
```

Class methods often use PascalCase.

```cpp
int Find(int x);
bool Union(int x, int y);
bool Same(int x, int y);
int Size(int x);
```

### 4.3 Lambdas

Algorithmic helper lambdas may use PascalCase if they behave like named functions.

```cpp
auto Solve = [&](vector < vector < int > > &A, int N, int p)->void{};
auto Heuristic = [&](void)->int{};
```

Small local lambdas may use normal descriptive names.

```cpp
auto AddFrequency = [&](int x)->void{};
auto UpdateTime = [&](int x, int t)->void{};
```

------

## 5. Input and Output Style

### 5.1 Fast Input

Use the custom `read<T>()` function by default.

```cpp
int N = read();
ll x = read < ll >();
```

The style allows spaces in template calls:

```cpp
read < ll >();
```

### 5.2 String Input

For strings, using `cin` is acceptable together with `read()`.

```cpp
string S;
cin >> S;
```

No need to add:

```cpp
ios::sync_with_stdio(false);
cin.tie(nullptr);
```

unless the whole solution uses `cin/cout`.

### 5.3 Character Input

For grid or character problems, use `getchar()` with a filtering loop.

```cpp
mp[i][j] = getchar();
while(mp[i][j] != '1' && mp[i][j] != '0' && mp[i][j] != '*')mp[i][j] = getchar();
```

### 5.4 Output

Prefer `printf`.

```cpp
printf("%d\n", res);
printf("%lld\n", ans);
```

Use `fflush(stdout)` when interactive behavior is involved.

```cpp
printf("B\n"); fflush(stdout);
```

Avoid `cout` unless necessary.

------

## 6. Initialization Style

Use constructor-style initialization for scalar variables.

```cpp
int res(0);
ll cur(0), ans(0);
int sx(-1), sy(-1);
```

For vectors, initialize with explicit sizes and default values.

```cpp
vector < int > C(N + 1, 0);
vector < vector < int > > A(N + 10, vector < int >(N + 10, 0));
```

Use `iota` for index initialization.

```cpp
iota(cols[i].begin(), cols[i].end(), 1);
```

When using 1-indexed ranges inside vectors, it is acceptable to use iterator tricks.

```cpp
iota(next(cands[i].begin()), next(cands[i].begin(), N + 1), 1);
```

------

## 7. Indexing Style

Prefer 1-indexing for most algorithmic data.

```cpp
for(int i = 1; i <= N; ++i)
    C[i] = read();
```

Use 0-indexing mainly for fixed grids, strings, or natural STL containers.

```cpp
for(int i = 0; i < 5; ++i)
    for(int j = 0; j < 5; ++j)
        cin >> mp[i][j];
```

When storing 1-indexed data in 0-indexed containers, use `.at(i - 1)` or `curp[p] - 1` when useful.

```cpp
cur.push({W.at(i - 1), T.at(i - 1), i});
int target = cols[p][curp[p] - 1];
```

------

## 8. STL Usage Style

### 8.1 Vector

Use `vector < T >` with extra capacity.

```cpp
vector < int > cap(M + 10, 0);
vector < vector < int > > S(N + 10, vector < int >(M + 10, 0));
```

### 8.2 Priority Queue

For complex `priority_queue`, write the template across multiple lines.

```cpp
priority_queue <
    tuple < int, int, int >,
    vector < tuple < int, int, int > >,
    less < tuple < int, int, int > >
> cur;
```

For custom comparators, use `function < bool(...) >`.

```cpp
vector < priority_queue < int, vector < int >, function < bool(const int&, const int&) > > > curr(M + 10);
```

### 8.3 Set With Lambda Comparator

Use `set < int, function < bool(...) > >` when the comparator depends on external arrays.

```cpp
set < int, function < bool(const int&, const int&) > > nodes([&](const int &a, const int &b)->bool{
    auto lhs = (ll)vals[a].first * vals[b].second;
    auto rhs = (ll)vals[b].first * vals[a].second;
    return lhs == rhs ? a < b : lhs > rhs;
});
```

### 8.4 List

Use `list` when splice or stable iterators are needed.

```cpp
vector < list < int > > ordered(N + 1);
ordered[rfa].splice(ordered[rfa].end(), ordered[rp]);
```

For cache-like problems, combine `list` with `unordered_map` iterators.

```cpp
list < pair < int, int > > timeline;
unordered_map < int, decltype(timeline.begin()) > mpTime;
```

### 8.5 Structured Bindings

Use structured bindings when extracting tuples or pairs.

```cpp
auto [w, t, idx] = cur.top(); cur.pop();

for(auto const& [s, times] : packs){
    // ...
}
```

------

## 9. Control Flow Style

### 9.1 Compact Loops

Single-line loops without braces are acceptable when the body is simple.

```cpp
for(int i = 1; i <= N; ++i)C[i] = read();
```

Nested loops are often written without braces if the body is short.

```cpp
for(int i = 1; i <= N; ++i)
    for(int j = 1; j <= M; ++j)
        S[i][j] = read();
```

### 9.2 Comma Operator

Using the comma operator to chain simple operations is acceptable.

```cpp
fa[y] = x, siz[x] += siz[y];

todos.push(curr[target]),
curr[target] = p;
```

This gives the code a compact competitive-programming flavor.

### 9.3 Early Continue / Return

Prefer early exits to reduce nesting.

```cpp
if(curp[p] > M)continue;
if(!h)return true;
if(dep + h > lim)return false;
```

------

## 10. Common Data Structures

### 10.1 DSU Template

The DSU style should look like this.

```cpp
class DSU{
private:
    vector < int > fa, siz;
public:
    DSU(int N = 0){init(N);}
    void init(int N){fa.resize(N + 1), siz.assign(N + 1, 1), iota(fa.begin(), fa.end(), 0);}
    int Find(int x){return x == fa[x] ? x : fa[x] = Find(fa[x]);}
    bool Union(int x, int y){
        x = Find(x), y = Find(y);
        if(x == y)return false;
        // if(siz[x] < siz[y])swap(x, y);
        fa[y] = x, siz[x] += siz[y];
        return true;
    }
    bool Same(int x, int y){return Find(x) == Find(y);}
    int Size(int x){return siz[Find(x)];}
}dsu;
```

Optional method:

```cpp
int Count(void){
    int ret(0);
    for(int i = 1; i < (int)fa.size(); ++i)ret += Find(i) == i;
    return ret;
}
```

Notes:

- Global `dsu` object is preferred.
- `Union` does not necessarily use union by size.
- The commented line `// if(siz[x] < siz[y])swap(x, y);` may be kept.
- Method names use `Find`, `Union`, `Same`, `Size`.

### 10.2 Manual Linked-List Graph

For tree/graph adjacency, manual pointer-based edge lists are acceptable.

```cpp
struct Edge{
    Edge* nxt;
    int to;
};
```

Use:

```cpp
vector < Edge* > head(N + 1, nullptr);

head[s] = new Edge{head[s], t};
head[t] = new Edge{head[t], s};
```

DFS can be written as a recursive lambda.

```cpp
auto dfs = [&](auto&& self, int p)->void{
    for(auto i = head[p]; i; i = i->nxt)
        if(i->to != fa[p])fa[i->to] = p, self(self, i->to);
}; dfs(dfs, root);
```

------

## 11. Algorithm Implementation Style

### 11.1 Greedy / Matching / Simulation

Prefer direct simulation with STL containers.

Examples:

- Use `priority_queue` for selecting best candidates.
- Use `stack < int > todos` for pending elements.
- Use `curp` arrays to record the current pointer of each object.
- Use custom comparators for preference lists.

```cpp
stack < int > todos;
for(int i = 1; i <= N; ++i)todos.push(i);

while(!todos.empty()){
    int p = todos.top(); todos.pop();
    // process p
}
```

### 11.2 Search

Use IDA* style with a heuristic lambda and recursive lambda.

```cpp
auto Heuristic = [&](void)->int{
    int ret(0);
    for(int i = 0; i < 5; ++i)
        for(int j = 0; j < 5; ++j)
            ret += mp[i][j] != pattern[i][j];
    return (ret + 1) >> 1;
};
```

DFS style:

```cpp
auto dfs = [&](auto&& self, int dep, int lim, int x, int y, int prex, int prey){
    auto h = Heuristic();
    if(!h)return true;
    if(dep + h > lim)return false;

    for(int t = 1; t <= 8; ++t){
        int tx = x + dx[t], ty = y + dy[t];
        if(tx < 0 || tx >= 5 || ty < 0 || ty >= 5 || (tx == prex && ty == prey))continue;
        swap(mp[x][y], mp[tx][ty]);
        if(self(self, dep + 1, lim, tx, ty, x, y))return true;
        swap(mp[x][y], mp[tx][ty]);
    }return false;
};
```

### 11.3 Sorting

Use lambda comparators with captured references.

```cpp
sort(cols[i].begin(), cols[i].end(), [&, i](const int &a, const int &b)->bool{
    return S[i][a] > S[i][b];
});
```

For multi-line sorting, align arguments naturally.

```cpp
sort(
    next(cands[i].begin()),
    next(cands[i].begin(), N + 1),
    [&, i](const int &a, const int &b)->bool{return A[i][a] > A[i][b];}
);
```

------

## 12. Debugging Style

Debug output should usually be commented out.

```cpp
// printf("[INFO] idx = %d\n", idx);
```

At the end of `main`, keep the timing line commented.

```cpp
// fprintf(stderr, "Time: %.6lf\n", (double)clock() / CLOCKS_PER_SEC);
```

Do not add a full logging system unless explicitly requested.

------

## 13. Comment Style

Comments are sparse and functional.

Good examples:

```cpp
unordered_map < int, decltype(freqList.begin()) > mpList; // frequency -> list iterator
unordered_map < int, int > freq; // key -> frequency
```

Avoid long explanatory comments inside code unless the problem is complex or the user asks for tutorial-style code.

------

## 14. Preferred Idioms

Use these idioms frequently:

```cpp
int N = read(), M = read();

ll cur(0), res(-1);

for(int i = 1; i <= N; ++i)A[i] = read();

while(!q.empty()){
    auto [x, y] = q.top(); q.pop();
}

printf("%lld\n", ans);

if(condition)return false;

vector < int > vec(N + 10, 0);

auto dfs = [&](auto&& self, int p)->void{
    // ...
};
```

Use `max` with initializer list when convenient.

```cpp
res = max({res, cur + cmx + ccur * (times - 1), cur + cmx});
```

Use `reverse`, `iota`, `sort`, `next`, `prev`, `advance`.

```cpp
reverse(S.begin(), S.end());
iota(cols[i].begin(), cols[i].end(), 1);
auto it = prev(timeline.end());
```

------

## 15. Things to Avoid

Avoid these unless explicitly required:

- Do not replace the template with a modern clean template.
- Do not use many macros such as `rep`, `pb`, `fi`, `se`.
- Do not use `endl`.
- Do not use `cout` for normal output.
- Do not add `ios::sync_with_stdio(false)` unless the solution mainly uses iostream.
- Do not over-engineer classes.
- Do not over-comment the algorithm.
- Do not convert all one-line branches into verbose blocks.
- Do not force 0-indexing when 1-indexing is more natural.
- Do not remove the final commented timing line.
- Do not remove `rnd`, `rndd`, `PI`, `E`, or typedefs from the default template.

------

## 16. Code Generation Checklist

When generating code in this style, check the following:

- The default template is preserved.
- Indentation uses 4 spaces.
- STL templates are written as `vector < int >`, not `vector<int>`.
- Input uses `read()`.
- Output uses `printf()`.
- Most arrays are 1-indexed and sized with `N + 10`.
- Short branches may be written in one line.
- Helper structures such as `DSU` follow the established style.
- Recursive DFS is preferably written using `auto&& self`.
- Debug lines are commented out.
- The final timing line remains before `return 0`.
- The code feels like compact contest code, not production software.

------

## 17. Example Skeleton for Future Solutions

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
using i128 = __int128_t;

template < typename T = int > inline T read(void){
    T ret(0);
    short flag(1);
    char c = getchar();
    while(c != '-' && !isdigit(c))c = getchar();
    if(c == '-')flag = -1, c = getchar();
    while(isdigit(c)){
        ret *= 10;
        ret += int(c - '0');
        c = getchar();
    }ret *= flag;
    return ret;
}



int main(){
    int N = read(), M = read();
    vector < int > A(N + 10, 0);
    for(int i = 1; i <= N; ++i)A[i] = read();

    ll res(0);

    // Solve here.

    printf("%lld\n", res);

    // fprintf(stderr, "Time: %.6lf\n", (double)clock() / CLOCKS_PER_SEC);
    return 0;
}

```