# 보강 정리 (C++)

## 입출력/유틸 템플릿
```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
using pii = pair<int,int>;
#define rep(i,a,b) for (int i = (a); i < (b); ++i)
#define all(x) (x).begin(), (x).end()

inline void fast_io() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
}

#ifdef LOCAL
#define dbg(x) cerr << #x << " = " << (x) << "\n"
#else
#define dbg(x) ((void)0)
#endif
```

## 시뮬레이션/구현 스니펫
```cpp
// 2D 회전 (시계 90도)
vector<string> rotate90(const vector<string>& g) {
    int n = g.size(), m = g[0].size();
    vector<string> r(m, string(n, '#'));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j)
            r[j][n - 1 - i] = g[i][j];
    return r;
}

// 구간 범위/경계 체크
inline bool in_bounds(int x, int y, int n, int m) {
    return 0 <= x && x < n && 0 <= y && y < m;
}
```

## 완전탐색/백트래킹 패턴
```cpp
// 순열 생성 (next_permutation)
vector<vector<int>> all_permutations(vector<int> a) {
    sort(all(a));
    vector<vector<int>> res;
    do res.push_back(a); while (next_permutation(all(a)));
    return res;
}

// 부분집합 비트마스크
vector<vector<int>> all_subsets(const vector<int>& a) {
    int n = a.size();
    vector<vector<int>> res;
    for (int mask = 0; mask < (1 << n); ++mask) {
        vector<int> cur;
        for (int i = 0; i < n; ++i) if (mask & (1 << i)) cur.push_back(a[i]);
        res.push_back(move(cur));
    }
    return res;
}

// 백트래킹 템플릿 (예: N-Queens)
bool backtrack(int row, vector<int>& col_used, int n, vector<int>& pos) {
    if (row == n) return true;
    for (int c = 0; c < n; ++c) {
        if (col_used[c]) continue;
        bool ok = true;
        for (int r = 0; r < row; ++r)
            if (abs(r - row) == abs(pos[r] - c)) { ok = false; break; }
        if (!ok) continue;
        pos[row] = c; col_used[c] = 1;
        if (backtrack(row + 1, col_used, n, pos)) return true;
        col_used[c] = 0;
    }
    return false;
}
```

## 동적 계획법(DP) 기본
```cpp
// LIS (O(n log n))
int lis_length(const vector<int>& a) {
    vector<int> d;
    for (int x : a) {
        auto it = lower_bound(all(d), x);
        if (it == d.end()) d.push_back(x);
        else *it = x;
    }
    return d.size();
}

// 0/1 배낭 (O(nW))
int knap01(const vector<int>& w, const vector<int>& v, int W) {
    vector<int> dp(W + 1, 0);
    for (size_t i = 0; i < w.size(); ++i)
        for (int cap = W; cap >= w[i]; --cap)
            dp[cap] = max(dp[cap], dp[cap - w[i]] + v[i]);
    return *max_element(all(dp));
}

// 격자 경로 DP (장애물: -1)
int grid_paths(const vector<vector<int>>& g) {
    int n = g.size(), m = g[0].size();
    vector<vector<int>> dp(n, vector<int>(m, 0));
    if (g[0][0] == -1) return 0;
    dp[0][0] = 1;
    for (int i = 0; i < n; ++i) for (int j = 0; j < m; ++j) {
        if (g[i][j] == -1) { dp[i][j] = 0; continue; }
        if (i) dp[i][j] += dp[i - 1][j];
        if (j) dp[i][j] += dp[i][j - 1];
    }
    return dp[n - 1][m - 1];
}
```

## 문자열 알고리즘
```cpp
// KMP prefix 함수
vector<int> prefix_function(const string& s) {
    int n = s.size();
    vector<int> pi(n, 0);
    for (int i = 1; i < n; ++i) {
        int j = pi[i - 1];
        while (j > 0 && s[i] != s[j]) j = pi[j - 1];
        if (s[i] == s[j]) ++j;
        pi[i] = j;
    }
    return pi;
}

// Z-Algorithm
vector<int> z_function(const string& s) {
    int n = s.size();
    vector<int> z(n, 0);
    for (int i = 1, l = 0, r = 0; i < n; ++i) {
        if (i <= r) z[i] = min(r - i + 1, z[i - l]);
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) ++z[i];
        if (i + z[i] - 1 > r) l = i, r = i + z[i] - 1;
    }
    return z;
}
```

## 세그먼트 트리/슬라이딩 윈도우
```cpp
// 세그먼트 트리 (구간합)
struct SegTree {
    int n; vector<long long> st;
    SegTree(int n_=0): n(1) { while (n < n_) n <<= 1; st.assign(2*n, 0); }
    void build(const vector<long long>& a) {
        int m = a.size();
        for (int i = 0; i < m; ++i) st[n + i] = a[i];
        for (int i = n - 1; i >= 1; --i) st[i] = st[i<<1] + st[i<<1|1];
    }
    void update(int idx, long long val) {
        int p = n + idx; st[p] = val;
        for (p >>= 1; p; p >>= 1) st[p] = st[p<<1] + st[p<<1|1];
    }
    long long query(int l, int r) { // inclusive
        long long res = 0;
        for (l += n, r += n; l <= r; l >>= 1, r >>= 1) {
            if (l & 1) res += st[l++];
            if (!(r & 1)) res += st[r--];
        }
        return res;
    }
};

// 슬라이딩 윈도우 최대값 (deque)
vector<int> sliding_max(const vector<int>& a, int k) {
    deque<int> dq; vector<int> res;
    for (int i = 0; i < (int)a.size(); ++i) {
        while (!dq.empty() && dq.front() <= i - k) dq.pop_front();
        while (!dq.empty() && a[dq.back()] <= a[i]) dq.pop_back();
        dq.push_back(i);
        if (i + 1 >= k) res.push_back(a[dq.front()]);
    }
    return res;
}
```

## 추가 연습 주제
- 시뮬레이션: 회전/복제/시간 흐름, 우선순위큐 기반 이벤트 처리, 말/로봇 이동.
- 그래프 응용: MST(크루스칼/프림), 이분 탐색 + 판별, SCC/2-SAT.
- 테스트 전략: 입력 파싱 강화, 코너 케이스(빈 그래프, 한 칸 보드, 음수 가중치 여부) 체크 리스트 작성.
