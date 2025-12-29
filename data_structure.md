Data Structure

---

```cpp
// Fast I/O setup for coding tests
#include <bits/stdc++.h>
using namespace std;

inline void fast_io() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
}
```

```cpp
// Stack (vector backed) (O(1) push/pop)
#include <bits/stdc++.h>
using namespace std;

template <class T>
struct Stack {
    vector<T> s;
    bool empty() const { return s.empty(); }
    void push(const T& v) { s.push_back(v); }
    T pop() { T v = s.back(); s.pop_back(); return v; }
    const T& top() const { return s.back(); }
};
```

```cpp
// Queue (circular buffer) (O(1) amortized)
#include <bits/stdc++.h>
using namespace std;

template <class T>
struct Queue {
    deque<T> q;
    bool empty() const { return q.empty(); }
    void push(const T& v) { q.push_back(v); }
    T pop() { T v = q.front(); q.pop_front(); return v; }
    const T& front() const { return q.front(); }
};
```

```cpp
// Min-heap priority queue (O(log n) push/pop)
#include <bits/stdc++.h>
using namespace std;

template <class T>
using MinHeap = priority_queue<T, vector<T>, greater<T>>;
```

```cpp
// Hash table usage (unordered_map) (O(1) average)
#include <bits/stdc++.h>
using namespace std;

unordered_map<string, string> sample_kv_store; // insert: sample_kv_store[key] = value;
```

```cpp
// Disjoint Set Union (Union-Find) with path compression + union by size
#include <bits/stdc++.h>
using namespace std;

struct DSU {
    vector<int> p, sz;
    DSU(int n = 0) { init(n); }
    void init(int n) { p.resize(n); iota(p.begin(), p.end(), 0); sz.assign(n, 1); }
    int find(int x) { return p[x] == x ? x : p[x] = find(p[x]); }
    bool unite(int a, int b) {
        a = find(a); b = find(b);
        if (a == b) return false;
        if (sz[a] < sz[b]) swap(a, b);
        p[b] = a; sz[a] += sz[b];
        return true;
    }
};
```

```cpp
// Fenwick Tree (Binary Indexed Tree) for prefix sums (O(log n))
#include <bits/stdc++.h>
using namespace std;

struct Fenwick {
    int n; vector<long long> bit;
    Fenwick(int n) : n(n), bit(n + 1, 0) {}
    void add(int idx, long long delta) {
        for (++idx; idx <= n; idx += idx & -idx) bit[idx] += delta;
    }
    long long sum(int idx) const {
        long long res = 0;
        for (++idx; idx > 0; idx -= idx & -idx) res += bit[idx];
        return res;
    }
    long long range_sum(int l, int r) const { return sum(r) - (l ? sum(l - 1) : 0); }
};
```

```cpp
// Simple binary tree node + traversal helpers
#include <bits/stdc++.h>
using namespace std;

struct Node {
    int val; unique_ptr<Node> left, right;
    explicit Node(int v) : val(v) {}
};

void preorder(const Node* root, vector<int>& out) {
    if (!root) return; out.push_back(root->val); preorder(root->left.get(), out); preorder(root->right.get(), out);
}
```

```cpp
// Adjacency-list graph container + add_edge helper
#include <bits/stdc++.h>
using namespace std;

struct Graph {
    int n; vector<vector<int>> adj;
    explicit Graph(int n) : n(n), adj(n) {}
    void add_edge(int u, int v, bool undirected = false) {
        adj[u].push_back(v); if (undirected) adj[v].push_back(u);
    }
};
```

```cpp
// Deque wrapper (O(1) push/pop at both ends)
#include <bits/stdc++.h>
using namespace std;

template <class T>
struct Deque {
    deque<T> dq;
    void push_front(const T& v) { dq.push_front(v); }
    void push_back(const T& v) { dq.push_back(v); }
    T pop_front() { T v = dq.front(); dq.pop_front(); return v; }
    T pop_back() { T v = dq.back(); dq.pop_back(); return v; }
    bool empty() const { return dq.empty(); }
};
```

```cpp
// Doubly linked list node (rarely needed; prefer std::list)
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val; ListNode *prev = nullptr, *next = nullptr;
    explicit ListNode(int v) : val(v) {}
};
```
