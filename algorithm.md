Algorithm

---

```cpp
// Fast I/O setup (call once near program start)
#include <bits/stdc++.h>
using namespace std;

inline void fast_io() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
}
```

```cpp
// Recursion: factorial (O(n) time, O(n) stack)
#include <bits/stdc++.h>
using namespace std;

long long factorial(int n) {
    if (n < 0) throw invalid_argument("n must be non-negative");
    return n <= 1 ? 1LL : n * factorial(n - 1);
}
```

```cpp
// Insertion sort for small arrays (O(n^2) time, O(1) extra space)
#include <bits/stdc++.h>
using namespace std;

template <class T>
void insertion_sort(vector<T>& a) {
    for (size_t i = 1; i < a.size(); ++i) {
        T cur = a[i];
        size_t j = i;
        while (j > 0 && a[j - 1] > cur) {
            a[j] = a[j - 1];
            --j;
        }
        a[j] = cur;
    }
}
```

```cpp
// Quick sort wrapper using std::sort (introsort) (O(n log n))
#include <bits/stdc++.h>
using namespace std;

template <class T>
void quick_sort(vector<T>& a) {
    sort(a.begin(), a.end());
}
```

```cpp
// Counting sort for non-negative integers in [0, max_value] (O(n + k))
#include <bits/stdc++.h>
using namespace std;

vector<int> counting_sort(const vector<int>& a, int max_value) {
    vector<int> cnt(max_value + 1, 0);
    for (int x : a) {
        if (x < 0 || x > max_value) throw invalid_argument("out of range");
        ++cnt[x];
    }
    for (int i = 1; i <= max_value; ++i) cnt[i] += cnt[i - 1];
    vector<int> res(a.size());
    for (int i = (int)a.size() - 1; i >= 0; --i) res[--cnt[a[i]]] = a[i];
    return res;
}
```

```cpp
// Binary search helpers using STL (O(log n))
#include <bits/stdc++.h>
using namespace std;

template <class T>
int lower_bound_index(const vector<T>& a, const T& target) {
    auto it = lower_bound(a.begin(), a.end(), target);
    return it == a.end() ? -1 : int(it - a.begin());
}
```

```cpp
// DFS (iterative) on adjacency list graph (O(V+E))
#include <bits/stdc++.h>
using namespace std;

vector<int> dfs_order(int n, const vector<vector<int>>& g, int start) {
    vector<int> order;
    vector<int> vis(n, 0);
    stack<int> st;
    st.push(start);
    while (!st.empty()) {
        int v = st.top();
        st.pop();
        if (vis[v]) continue;
        vis[v] = 1;
        order.push_back(v);
        for (auto it = g[v].rbegin(); it != g[v].rend(); ++it) {
            if (!vis[*it]) st.push(*it);
        }
    }
    return order;
}
```

```cpp
// BFS shortest path on grid (4-direction) (O(NM))
#include <bits/stdc++.h>
using namespace std;

int bfs_grid(const vector<string>& board, pair<int,int> src, pair<int,int> dst) {
    const int n = board.size();
    const int m = board.empty() ? 0 : board[0].size();
    const vector<pair<int,int>> d{{1,0},{-1,0},{0,1},{0,-1}};
    vector<vector<int>> dist(n, vector<int>(m, -1));
    queue<pair<int,int>> q;
    int sx = src.first, sy = src.second;
    int tx = dst.first, ty = dst.second;
    dist[sx][sy] = 0; q.push(src);
    while (!q.empty()) {
        pair<int,int> cur = q.front(); q.pop();
        int x = cur.first, y = cur.second;
        if (x == tx && y == ty) return dist[x][y];
        for (size_t i = 0; i < d.size(); ++i) {
            int nx = x + d[i].first, ny = y + d[i].second;
            if (0 <= nx && nx < n && 0 <= ny && ny < m && board[nx][ny] != '#' && dist[nx][ny] == -1) {
                dist[nx][ny] = dist[x][y] + 1;
                q.push({nx, ny});
            }
        }
    }
    return -1; // unreachable
}
```

```cpp
// Dijkstra with priority_queue (O(E log V))
#include <bits/stdc++.h>
using namespace std;

vector<long long> dijkstra(int n, const vector<vector<pair<int,int>>>& g, int src) {
    const long long INF = (long long)4e18;
    vector<long long> dist(n, INF);
    priority_queue<pair<long long,int>, vector<pair<long long,int>>, greater<>> pq;
    dist[src] = 0; pq.push({0, src});
    while (!pq.empty()) {
        pair<long long,int> cur = pq.top(); pq.pop();
        long long d = cur.first; int v = cur.second;
        if (d != dist[v]) continue;
        for (size_t i = 0; i < g[v].size(); ++i) {
            int to = g[v][i].first; int w = g[v][i].second;
            long long nd = d + (long long)w;
            if (nd < dist[to]) {
                dist[to] = nd;
                pq.push({nd, to});
            }
        }
    }
    return dist;
}
```

```cpp
// Floyd–Warshall (O(n^3))
#include <bits/stdc++.h>
using namespace std;

vector<vector<long long>> floyd_warshall(const vector<vector<long long>>& w) {
    const long long INF = (long long)4e18;
    int n = w.size();
    vector<vector<long long>> dist = w;
    for (int i = 0; i < n; ++i) dist[i][i] = 0;
    for (int k = 0; k < n; ++k)
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                if (dist[i][k] < INF && dist[k][j] < INF)
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
    return dist;
}
```

```cpp
// Topological sort (Kahn) with cycle detection (O(V+E))
#include <bits/stdc++.h>
using namespace std;

vector<int> topo_sort(int n, const vector<vector<int>>& g) {
    vector<int> indeg(n, 0);
    for (int v = 0; v < n; ++v)
        for (int to : g[v]) ++indeg[to];
    queue<int> q;
    for (int i = 0; i < n; ++i) if (indeg[i] == 0) q.push(i);
    vector<int> order;
    while (!q.empty()) {
        int v = q.front(); q.pop();
        order.push_back(v);
        for (int to : g[v]) if (--indeg[to] == 0) q.push(to);
    }
    if ((int)order.size() != n) order.clear(); // cycle exists
    return order;
}
```

```cpp
// Bipartite matching (Hopcroft–Karp) (O(E sqrt V))
#include <bits/stdc++.h>
using namespace std;

struct HopcroftKarp {
    int n_left, n_right;
    vector<vector<int>> adj;
    vector<int> dist, matchL, matchR;
    HopcroftKarp(int n_left, int n_right)
        : n_left(n_left), n_right(n_right), adj(n_left), matchL(n_left, -1), matchR(n_right, -1) {}

    void add_edge(int u, int v) { adj[u].push_back(v); }

    bool bfs() {
        queue<int> q;
        dist.assign(n_left, -1);
        for (int u = 0; u < n_left; ++u) if (matchL[u] == -1) { dist[u] = 0; q.push(u); }
        bool found = false;
        while (!q.empty()) {
            int u = q.front(); q.pop();
            for (int v : adj[u]) {
                int mu = matchR[v];
                if (mu == -1) { found = true; }
                else if (dist[mu] == -1) { dist[mu] = dist[u] + 1; q.push(mu); }
            }
        }
        return found;
    }

    bool dfs(int u) {
        for (int v : adj[u]) {
            int mu = matchR[v];
            if (mu == -1 || (dist[mu] == dist[u] + 1 && dfs(mu))) {
                matchL[u] = v; matchR[v] = u; return true;
            }
        }
        dist[u] = -1; return false;
    }

    int max_matching() {
        int matching = 0;
        while (bfs())
            for (int u = 0; u < n_left; ++u)
                if (matchL[u] == -1 && dfs(u)) ++matching;
        return matching;
    }
};
```

```cpp
// Dinic's Max Flow (O(E V^2))
#include <bits/stdc++.h>
using namespace std;

struct Dinic {
    struct Edge { int to; long long cap, flow; int rev; };
    int n; vector<vector<Edge>> g; vector<int> level, it;
    Dinic(int n) : n(n), g(n), level(n), it(n) {}
    void add_edge(int u, int v, long long cap) {
        Edge a{v, cap, 0, (int)g[v].size()};
        Edge b{u, 0, 0, (int)g[u].size()};
        g[u].push_back(a); g[v].push_back(b);
    }
    bool bfs(int s, int t) {
        fill(level.begin(), level.end(), -1);
        queue<int> q; level[s] = 0; q.push(s);
        while (!q.empty()) {
            int v = q.front(); q.pop();
            for (const auto& e : g[v]) if (e.cap - e.flow > 0 && level[e.to] == -1) {
                level[e.to] = level[v] + 1; q.push(e.to);
            }
        }
        return level[t] != -1;
    }
    long long dfs(int v, int t, long long f) {
        if (v == t || f == 0) return f;
        for (int& i = it[v]; i < (int)g[v].size(); ++i) {
            Edge& e = g[v][i];
            if (level[e.to] != level[v] + 1 || e.cap - e.flow == 0) continue;
            long long pushed = dfs(e.to, t, min(f, e.cap - e.flow));
            if (pushed) { e.flow += pushed; g[e.to][e.rev].flow -= pushed; return pushed; }
        }
        return 0;
    }
    long long max_flow(int s, int t) {
        long long flow = 0, pushed;
        while (bfs(s, t)) {
            fill(it.begin(), it.end(), 0);
            while ((pushed = dfs(s, t, (long long)4e18))) flow += pushed;
        }
        return flow;
    }
};
```
