#! https://zhuanlan.zhihu.com/p/684074732
# [edu7 DE](https://codeforces.com/group/OMOWqOnaA2/contest/507070)

**进算竞小群加我**

## D. Optimal Number Permutation

您的数组 a 包含从 1 到 n 的所有整数两次。您可以任意排列 a 中的任何数字。

令数字 i 位于置换数组 a 中的位置 $x_i, y_i$ ( $x_i < y_i$)。

让我们定义值 $d_i = y_i - x_i$  （ 数字 i 的位置之间的距离）。

排列数组 a 中的数字，以最小化 总和$s=\sum\limits_{i=1}^{n} (n-i)|d_i+i-n|$的值 。

**分析**

$s=\sum\limits_{i=1}^{n} (n-i)|d_i+i-n|$

$s=\sum\limits_{i=1}^{n} (n-i)|y_i-x_i+i-n|$

$n-i> 0 ,  |y_i-x_i+i-n|\ge 0$

当 $\forall i\in [1,n] ,|y_i-x_i+i-n| =  0$ 取得最小值

就是使得

$y_i-x_i+i-n=0$

$y_i-x_i=n-i$

$1,n$

$n+1,2n-1$

$2,n-1$

这样交叉

取出$[1,n]$ 为第一段区间

取出$[n+1,2n]$ 为第二段区间

使得第一段区间能够安放$[1,3,5,7\dots]$

使得第二段区间能够安放$[2,4,6,8\dots]$

直到$n$ 停下，然后剩下还没有放的地方就是 $i=n$ 的归宿

**code**

[Submission #248360401 - Codeforces](https://codeforces.com/group/OMOWqOnaA2/contest/507070/submission/248360401)

```C++
// Codeforces - edu7 D. Optimal Number Permutation
// https://codeforces.com/group/OMOWqOnaA2/contest/507070/problem/D 2024-02-26
// 21:55:33

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
constexpr int N = 1e6 + 10;
int a[N];
void solve() {
  cin >> n;
  int l = 1, r = n;
  int _l = n + 1, _r = 2 * n - 1;
  for (int i = 1; i <= n; i += 2) {
    if (i < n) a[l++] = a[r--] = i;
    if (i + 1 < n) a[_l++] = a[_r--] = i + 1;
  }
  for (int i = 1; i <= n + n; i++) {
    if (a[i] == 0) a[i] = n;
    cout << a[i] << " ";
  }
  cout << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```



## E. Ants in Leaves

树是一个没有环的连通图。一棵树的叶子是与另一个顶点精确连接的任何顶点。

给您一棵具有 $n$ 个顶点和位于顶点 1 的根的树。树的每片叶子上都有一只蚂蚁。一秒钟内，一些蚂蚁可以同时从它们所在的顶点到达父顶点。**除了树的根之外**，**没有两只蚂蚁可以同时位于同一个顶点**。

求所有蚂蚁都到达树根所需的最短时间。请注意，一开始蚂蚁只在树叶中。

**分析**

维护深度的序列和前缀数量

有个贪心的性质需要找一下

就是优先让深度浅的上

这样能使得这棵树更快的跑完

然后dfs一遍找出所有叶节点

按照深度排序

如果前面没有跑完就是前面的dp+1

如果前面的跑完了就是 当前的深度

每棵树的答案是最后一个数

答案取最大的一个就行

**code**

[Submission #248366006 - Codeforces](https://codeforces.com/group/OMOWqOnaA2/contest/507070/submission/248366006)

```C++
// Codeforces - edu7 E. Ants in Leaves
// https://codeforces.com/group/OMOWqOnaA2/contest/507070/problem/E 2024-02-26
// 22:17:44

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<vector<int>> G(n);
  for (int i = 1; i < n; i++) {
    int a, b;
    cin >> a >> b;
    a--, b--;
    G[a].emplace_back(b);
    G[b].emplace_back(a);
  }
  vector<int> now;
  auto dfs = [&](auto self, int u, int fa, int dep) -> void {
    if (G[u].size() == 1) now.emplace_back(dep);
    for (auto it : G[u]) {
      if (it == fa)
        continue;
      else
        self(self, it, u, dep + 1);
    }
  };
  vector<int> ans;
  int _ans = 0;
  for (auto it : G[0]) {
    now.clear();
    ans.clear();
    dfs(dfs, it, 0, 1);
    sort(all(now));
    for (int i = 0; i < now.size(); i++) {
      if (not i)
        ans.emplace_back(now[i]);
      else
        ans.emplace_back(max(ans.back() + 1, now[i]));
    }
    _ans = max(_ans, ans.back());
  }
  cout << _ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

