Algorithm

---

```recursion
#include <stdio.h>
 
long long factorial(int num)
{
    if (num == 0)
    {
        return 1;
    }
    else
    {
        return num * factorial(num - 1);
    }
}
 
 
int main(void)
{
    int test_case;
    int T;
    int num;
    long long value;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; ++test_case)
    {
        scanf("%d", &num);
        value = factorial(num);
        printf("#%d %d! = %lld\n", test_case, num, value);
    }
}
```

---

```insertion sort
#include <stdio.h>
 
#define MAX_NUM 100
 
int input[MAX_NUM];
int num;
 
void insertionSort(void)
{
    int temp;
    int i;
    int j;
 
    for (i = 1; i < num; i++)
    {
        temp = input[i];
        j = i - 1;
 
        while ((j >= 0) && (temp < input[j]))
        {
            input[j + 1] = input[j];
            j = j - 1;
        }
        input[j + 1] = temp;
    }
}
 
void printResult(void)
{
    int i;
 
    for (i = 0; i < num; ++i)
    {
        printf("%d ", input[i]);
    }
    printf("\n");
}
 
int main(void)
{
    int T;
    int test_case;
    int i;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &num);
        for (i = 0; i < num; i++)
        {
            scanf("%d", &input[i]);
        }
        insertionSort();
        printf("#%d ", test_case);
        printResult();
    }
 
    return 0;
}
```

---

```quick sort
#include <stdio.h>
 
#define MAX_NUM 100
 
int input[MAX_NUM];
int num;
 
void quickSort(int first, int last)
{
    int pivot;
    int i;
    int j;
    int temp;
     
    if (first < last)
    {
        pivot = first;
        i = first;
        j = last;
 
        while (i < j)
        {
            while (input[i] <= input[pivot] && i < last)
            {
                i++;
            }
            while (input[j] > input[pivot])
            {
                j--;
            }
            if (i < j)
            {
                temp = input[i];
                input[i] = input[j];
                input[j] = temp;
            }
        }
 
        temp = input[pivot];
        input[pivot] = input[j];
        input[j] = temp;
 
        quickSort(first, j - 1);
        quickSort(j + 1, last);
    }
}
 
void printResult(void)
{
    int i;
 
    for (i = 0; i < num; ++i)
    {
        printf("%d ", input[i]);
    }
    printf("\n");
}
 
int main(void)
{
    int T;
    int test_case;
    int i;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &num);
        for (i = 0; i < num; i++)
        {
            scanf("%d", &input[i]);
        }
        quickSort(0, num - 1);
        printf("#%d ", test_case);
        printResult();
    }
 
    return 0;
}
```

---

```counting sort
#include <stdio.h>
 
#define MAX_N 100
#define MAX_DIGIT 10
 
int N;  // # of data set
int arr[MAX_N];
int cnt[MAX_DIGIT];
int sortedArr[MAX_N];
 
void calculateDigitNumber()
{
    for (int i = 0; i < N; i++)
    {
        cnt[arr[i]]++;
    }
 
    for (int i = 1; i < MAX_DIGIT; i++)
    {
        cnt[i] = cnt[i-1] + cnt[i];
    }
}
 
void executeCountingSort()
{
    for (int i = N-1; i >= 0; i--)
    {
        sortedArr[--cnt[arr[i]]] = arr[i];
    }
}
 
int main(void)
{
    int T;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        scanf("%d", &N);
 
        for (int i = 0; i < N; i++)
        {
            scanf("%d", &arr[i]);
        }
 
        // initialize
        for (int i = 1; i < MAX_DIGIT; i++)
        {
            cnt[i] = 0;
        }
 
        calculateDigitNumber();
        executeCountingSort();
 
        //print the sorted digits
        printf("#%d ", test_case);
        for (int i = 0; i < N; i++) 
        {
            printf("%d ", sortedArr[i]);
        }
        printf("\n");
    }
    return 0;
}
```

---

```binary sort
#include <stdio.h>
 
#define MAX_M 100
 
int T;    // # of test case
int M;    // # of element in array
int N;    // # of numbers to search
int arr[MAX_M];
 
void binarySearch(int* arr, int low, int high, int target)
{
    int mid;
    if (low > high) 
    {
        printf("-1 ");
        return;
    }
 
    mid = (low + high) / 2;
 
    if (target < arr[mid])
    {
        binarySearch(arr, low, mid - 1, target);
    }
    else if (arr[mid] < target)
    {
        binarySearch(arr, mid + 1, high, target);
    }
    else
    {
        printf("%d ", mid);
        return;
    }
}
 
int main(void)
{
    int targetValue;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        printf("#%d ", test_case);
        scanf("%d %d", &M, &N);
 
        for (int i = 0; i < M; i++)
        {
            scanf("%d", &arr[i]);
        }
 
        for (int i = 0; i < N; i++) 
        {
            scanf("%d", &targetValue);
            binarySearch(arr, 0, M-1, targetValue);
        }
        printf("\n");
    }
    return 0;
}
```

---

```dfs searching
#include <stdio.h>
 
#define MAX_VERTEX 30
 
int map[MAX_VERTEX][MAX_VERTEX];
int visit[MAX_VERTEX];
int vertex;
int edge;
int maxEdge;
int start;
int end;
 
void depthFirstSearch(int v, int depth)
{
    int i;
    if (v == end) 
    {
        if (maxEdge < 0 || depth < maxEdge)
        {
            maxEdge = depth;
        }
        return;
    }
 
    visit[v] = 1;
    for (i = 1; i <= vertex; i++) 
    {
        if (map[v][i] == 1 && !visit[i]) 
        {
            depthFirstSearch(i, depth + 1);
            visit[i] = 0;
        }
    }
}
 
 
int main(void)
{
    int T;
    int test_case;
    int i;
    int v1;
    int v2;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d %d %d %d", &vertex, &edge, &start, &end);
 
        for (i = 0; i < edge; i++)
        {
            scanf("%d %d", &v1, &v2);
            map[v1][v2] = 1;
        }
 
        maxEdge = -1;
        depthFirstSearch(start, 0);
        printf("#%d %d\n", test_case, maxEdge);
    }
    return 0;
}
```

---

```dfs searching
#include <stdio.h>
 
#define MAX_N 50
 
int MAP[MAX_N + 2][MAX_N + 2];
int queue[MAX_N * MAX_N][3];
int row;
int column;
int head;
int rear;
 
int isEmpty()
{
    return (head <= rear) ? 1 : 0;
}
 
int enqueue(int x, int y, int c)
{
    queue[head][0] = x;
    queue[head][1] = y;
    queue[head][2] = c;
    head++;
    return 1;
}
 
int dequeue(int *x, int *y, int *c)
{
    if (isEmpty())
    {
        return 0;
    }
    *x = queue[rear][0];
    *y = queue[rear][1];
    *c = queue[rear][2];
    rear++;
    return 1;
}
 
int breadthFirstSearch()
{
    int x;
    int y;
    int c;
 
    enqueue(1, 1, 0);
    MAP[1][1] = 0;
    while (!isEmpty()) 
    {
        dequeue(&x, &y, &c);
        if (x == column && y == row)
        {
            return c;
        }
        if (x + 1 <= column && MAP[x + 1][y]) 
        {
            enqueue(x + 1, y, c + 1);
            MAP[x + 1][y] = 0;
        }
        if (y + 1 <= row && MAP[x][y + 1]) 
        {
            enqueue(x, y + 1, c + 1);
            MAP[x][y + 1] = 0;
        }
        if (x - 1 > 0 && MAP[x - 1][y]) 
        {
            enqueue(x - 1, y, c + 1);
            MAP[x - 1][y] = 0;
        }
        if (y - 1 > 0 && MAP[x][y - 1]) 
        {
            enqueue(x, y - 1, c + 1);
            MAP[x][y - 1] = 0;
        }
    }
    return -1;
}
 
 
int main(void)
{
    int test_case;
    int T;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; test_case++) 
    {
        head = 0;
        rear = 0;
        scanf("%d %d", &row, &column);
 
        for (int i = 1; i <= row; i++) 
        {
            for (int j = 1; j <= column; j++)
            {
                scanf("%d", &MAP[j][i]);
            }
        }
        printf("#%d %d\n", test_case, breadthFirstSearch());
    }
    return 0;
}
```

---

```parametric search
#include <stdio.h>
 
#define MAX_RIBBON 100
 
int K;
int N;
int low, high, mid, numRibbonTape, max;
int sizeRibbonTape[MAX_RIBBON];
 
void search()
{
    mid = low + (high - low) / 2;
    numRibbonTape = 0;
 
    for (int i = 0; i < K ; i++) 
    {
        numRibbonTape += (sizeRibbonTape[i] / mid);
    }
 
    if (numRibbonTape >= N)
    {
        low = mid + 1;
        if (max < mid)
            max = mid;
    }
    else
    {
        high = mid - 1;
    }
}
 
int main(int argc, char** argv)
{
    int T;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        low = 1;
        high = 0 ;
        max = -1;
 
        scanf("%d %d", &K, &N);
 
        for (int i = 0; i < K; i++)
        {
            scanf("%d", &sizeRibbonTape[i]);
            if ( high < sizeRibbonTape[i] )
            {
                high = sizeRibbonTape[i] ;
            }
        }
 
        while (low <= high)
        {
            search();
        }
        printf("#%d ", test_case);
        printf("%d\n", max);
    }
    return 0;
}
```

---

```dynamic programing
#include <stdio.h>
 
#define MAX_X 2
#define MAX_N 1001
 
int N;
int num_pair[MAX_N][2];
 
int dp[MAX_N][MAX_X * MAX_N];
 
int max(int a, int b) {
    return (a > b) ? a : b;
}
 
int solve() {
    int ans = 0;
    for (int i = 0; i < MAX_N; i++) {
        for (int j = 0; j < MAX_X * MAX_N; j++) {
            dp[i][j] = 0;
        }
    }
    for (int i = 0; i < N; i++) {
        ans += num_pair[i][0];
    }
 
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < ans; j++) {
            dp[i][j] = -1;
        }
    }
 
    dp[0][ans] = 0;
    dp[0][ans - num_pair[0][0] - num_pair[0][1]] = num_pair[0][0];
 
    for (int i = 1; i < N; i++) {
        int sum = num_pair[i][0] + num_pair[i][1];
        for (int j = 0; j <= ans; j++) {
            if (dp[i - 1][j] != -1) {
                int diff = j - sum;
                if (diff >= 0) {
                    dp[i][diff] = max(dp[i][diff], dp[i - 1][j] + num_pair[i][0]);
                }
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    int max_value = -1;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j <= ans; j++) {
            max_value = max(max_value, dp[i][j]);
        }
    }
    return ans - max_value;
 
}
 
int main(void)
{
    int T;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &N);
 
        for (int i = 0; i < N; i++)
        {
            scanf("%d %d", &num_pair[i][0], &num_pair[i][1]);
        }
        printf("#%d %d\n", test_case, solve());
    }
    return 0;
}
```

---

```permutation & combination
#include <stdio.h>
 
#define MAX_STRING_LENGTH 10
 
int stackTop = 0;
char combinationStack[MAX_STRING_LENGTH];
 
void swap(char *x, char *y)
{
    char temp;
    temp = *x;
    *x = *y;
    *y = temp;
}
 
void permutation(char *str, int l, int r)
{
    if (l == r)
    {
        printf("%s\n", str);
    }
    else
    {
        for (int i = l; i <= r; i++) 
        {
            swap((str+l), (str+i));
            permutation(str, l+1, r);
            swap((str+l), (str+i)); //backtrack
        }
    }
}
 
void push(char ch) 
{
    combinationStack[stackTop++] = ch;
    combinationStack[stackTop] = '\0';
}
 
void pop() 
{
    combinationStack[--stackTop] = '\0';
}
 
void combination(const char* str, int length, int offset, int k) 
{
    if (k == 0) 
    {
        printf("%s\n", combinationStack);
        return;
    }
    for (int i = offset; i <= length - k; ++i) 
    {
        push(str[i]);
        combination(str, length, i+1, k-1);
        pop();
    }
}
 
int main()
{
    int N, K, T;
    char str[MAX_STRING_LENGTH];
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        scanf("%s%d%d", str, &N, &K);
        str[N] = 0;
        printf("#%d\n", test_case);
 
        permutation(str, 0, N-1);
        combination(str, N, 0, K);
    }
 
    return 0;
}
```

---

```dijkstra
#include <stdio.h>
 
#define N 100
#define INF 100000
 
int map[N + 1][N + 1];
int visit[N + 1];
int dist[N + 1];
int vertex;
int edge;
int start;
int end;
 
void dijkstra(void)
{
    int i;
    int j;
    int min;
    int v;
 
    dist[start] = 0;
 
    for (i = 1; i <= vertex; i++)
    {
        min = INF;
 
        for (j = 1; j <= vertex; j++)
        {
            if (visit[j] == 0 && min > dist[j])
            {
                min = dist[j];
                v = j;
            }
        }
 
        visit[v] = 1;
 
        for (j = 1; j <= vertex; j++)
        {
            if (dist[j] > dist[v] + map[v][j])
            {
                dist[j] = dist[v] + map[v][j];
            }
        }
    }
}
 
int main(void)
{
    int test_case;
    int T;
    int i;
    int j;
    int from;
    int to;
    int value;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d %d %d", &vertex, &start, &end);
        scanf("%d", &edge);
 
        for (i = 1; i <= vertex; i++)
        {
            for (j = 1; j <= vertex; j++)
            {
                if (i != j)
                {
                    map[i][j] = INF;
                }
            }
        }
 
        for (i = 1; i <= edge; i++) 
        {
            scanf("%d %d %d", &from, &to, &value);
            map[from][to] = value;
        }
 
        for (i = 1; i <= vertex; i++)
        {
            dist[i] = INF;
            visit[i] = 0;
        }
 
        printf("#%d ", test_case);
        dijkstra();
        printf("%d \n", dist[end]);
    }
    return 0;
}
```

---

```floyd warshall
#include <stdio.h>
#define INFINITY 999999
 
int weight[101][101];
int result[101][101];
 
void floyd(int n) 
{
    int i, j, k;
 
    for (k = 0; k < n; k++) 
    {
        for (i = 0; i < n; i++) 
        {
            if (k == 0) 
            {
                for (j = 0; j < n; j++) 
                {
                    result[i][j] = weight[i][j];
                }
            }
            for (j = 0; j < n; j++) 
            {
                if (result[i][k] + result[k][j] < result[i][j])
                {
                    result[i][j] = result[i][k] + result[k][j];
                }
            }
        }
    }
}
 
int main() 
{
    int T;
    int n, m, i, j;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        scanf("%d %d", &n, &m);
        for (i = 0; i < n; i++) 
        {
            for (j = 0; j < n; j++) 
            {
                weight[i][j] = INFINITY;
            }
            weight[i][i] = 0;
        }
 
        for (i = 0; i < m; i++) 
        {
            int st, en, w;
            scanf("%d %d %d", &st, &en, &w);
            if (weight[st-1][en-1] > w)
            {
                weight[st-1][en-1] = w;
            }
        }
 
        floyd(n);
 
        printf("#%d\n", test_case);
        for (i = 0; i < n; i++) 
        {
            for (j = 0; j < n; j++) 
            {
                printf("%d ", result[i][j]);
            }
            printf("\n");
        }
    }
}
```

---

```plane sweeping
#include <stdio.h>
 
int N;
#define MAX_N 10000
 
typedef struct rec
{
    int x, y1, y2, end;
} rec;
 
rec make_rec(int _x,int _y1,int _y2,int _end)
{
    rec t = {_x, _y1, _y2, _end};
    return t;
}
 
int rec_greater_than(rec* a, rec* b)
{
    return a->x != b->x ? a->x > b->x : 0;
}
 
int tree[65538],cnt[65538];
 
void update(int x, int left, int right, int nodeLeft, int nodeRight, int val)
{
    if (left > nodeRight || right < nodeLeft)
    {
        return;
    }
    if (left <= nodeLeft && right >= nodeRight)
    {
        cnt[x] += val;
    }
    else
    {
        int mid = (nodeLeft + nodeRight) >> 1;
        update(x * 2, left, right, nodeLeft, mid, val);
        update(x * 2 + 1, left, right, mid + 1, nodeRight, val);
    }
    tree[x] = 0;
    if (cnt[x] > 0)
    {
        tree[x] = nodeRight - nodeLeft + 1;
    }
    if (cnt[x] == 0 && nodeLeft < nodeRight)
    {
        tree[x] = tree[x * 2] + tree[x * 2 + 1];
    }
}
 
int partition(rec a[], int l, int r)
{
    rec pivot, t;
    int i, j;
    pivot = a[l];
    i = l;
    j = r + 1;
 
    while (1) {
        do{
            ++i;
        } while ((!rec_greater_than(&a[i],  &pivot)) && i <= r);
 
        do{
            --j;
        } while (rec_greater_than(&a[j], &pivot));
 
        if (i >= j)
        {
            break;
        }
        t = a[i];
        a[i] = a[j];
        a[j] = t;
    }
    t = a[l];
    a[l] = a[j];
    a[j] = t;
    return j;
}
 
 
void quick_sort(rec a[], int l, int r)
{
    int j;
 
    if (l < r) 
    {
        j = partition(a, l, r);
        quick_sort(a, l, j - 1);
        quick_sort(a, j + 1, r);
    }
}
 
int main()
{
    int T;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        static rec v[MAX_N * 2];
        scanf("%d",&N);
 
        int idx = 0, i, px, ans;
        for (i = 0; i < N; i++) 
        {
            int x1, y1, x2, y2;
            scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
            v[idx++] = make_rec(x1,y1,y2,1);
            v[idx++] = make_rec(x2,y1,y2,-1);
        }
 
        quick_sort(v, 0 , idx - 1);
        px = v[0].x;
        ans = 0;
        for (i = 0; i < idx; i++) 
        {
            ans += (v[i].x - px) * tree[1];
            update(1, v[i].y1, v[i].y2-1, 0, 32768, v[i].end);
            px = v[i].x;
        }
        printf("#%d ", test_case);
        printf("%d\n",ans);
    }
 
    return 0;
}
```

---

```minimum spanning tree
#include<stdio.h>
 
int V;
int graph[100][100];
 
int minKey(int *key, unsigned char *mstSet)
{
    int min = 2147483647;
    int min_index;
 
    for (int v = 0; v < V; v++)
    {
        if (mstSet[v] == 0 && key[v] < min) 
        {
            min = key[v];
            min_index = v;
        }
    }
 
    return min_index;
}
 
void printMST(int parent[])
{
    int weightSum = 0;
    for (int i = 1; i < V; i++)
    {
        weightSum += graph[i][parent[i]];
    }
    printf("%d\n", weightSum);
}
 
void primMST()
{
     int parent[100];
     int key[100];
     unsigned char mstSet[100];
 
     for (int i = 0; i < V; i++) 
     {
        key[i] = 2147483647;
        mstSet[i] = 0;
     }
 
     key[0] = 0;
     parent[0] = -1;
 
     for (int count = 0; count < V-1; count++)
     {
        int u = minKey(key, mstSet);
 
        mstSet[u] = 1;
 
        for (int v = 0; v < V; v++)
        {
          if (graph[u][v] && mstSet[v] == 0 && graph[u][v] <  key[v])
          {
             parent[v]  = u, key[v] = graph[u][v];
          }
        }
     }
 
     printMST(parent);
}
 
int main(void) 
{
    int i, j, T;
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++) 
    {
        printf("#%d ", test_case);
        scanf("%d", &V);
 
        for (i = 0; i < V; i++) 
        {
            for (j = 0; j < V; j++) 
            {
                scanf("%d", &graph[i][j]);
            }
        }
 
        primMST();
    }
 
    return 0;
}
```

---

```topological sorting
#include <stdio.h>
#include <stdlib.h>
 
#define MAX_N 25
#define MAX_M 25
#define CONNECTED 1
#define NOT_CONNECTED 0
#define NOT_UPDATED_YET 0
#define NOT_VISITED -1
#define DUPLICATE -2
 
int map[MAX_N][MAX_N] = {0, };
int count[MAX_N] = {0, };
int test_case, n, m;
 
typedef struct {
    int queue[MAX_N];
    int cur_ptr;
    int last_ptr;
} Queue;
 
void queue_reset(Queue* queue) 
{
    queue->cur_ptr = 0;
    queue->last_ptr = 0;
}
 
int queue_has_item(Queue* queue) 
{
    return queue->last_ptr - queue->cur_ptr > 0;
}
 
int queue_dequeue(Queue* queue) 
{
    return queue->queue[queue->cur_ptr++];
}
 
void queue_enqueue(Queue* queue, const int item) 
{
    queue->queue[queue->last_ptr++] = item;
}
 
typedef struct {
    int stack_set[MAX_N];
    int last_ptr;
} Stack;
 
 
void stack_reset(Stack* stack) 
{
    stack->last_ptr = 0;
}
 
int stack_has_item(Stack* stack) 
{
    return stack->last_ptr > 0;
}
 
int stack_peek(Stack* stack) 
{
    return stack->stack_set[stack->last_ptr - 1];
}
 
int stack_pop(Stack* stack) 
{
    return stack->stack_set[--stack->last_ptr];
}
 
void stack_set_mark_duplicate(Stack* stack, const int item) 
{
    int i;
    for (i = 0; i < stack->last_ptr; i++) 
    {
        if (stack->stack_set[i] == item)
        {
            stack->stack_set[i] = DUPLICATE;
        }
    }
}
 
void stack_set_push(Stack* stack, const int item) 
{
    stack_set_mark_duplicate(stack, item);
 
    stack->stack_set[stack->last_ptr++] = item;
}
 
typedef struct _Node {
    int item;
    struct _Node* prev;
} Node;
 
void node_reset(Node* node) 
{
    Node* cur = node->prev;
    while (cur) 
    {
        Node* temp = cur;
        cur = temp->prev;
        free(temp);
    }
    node->prev = NULL;
}
 
void node_push(Node* node, Node* other) 
{
    if (node->prev == NULL) 
    {
        node->prev = other;
        return;
    }
 
    Node* head = node;
    while (head->prev != NULL) 
    {
        head = head->prev;
    }
 
    head->prev = other;
}
 
int connected(const int src, const int dest) 
{
    return map[src][dest] == CONNECTED;
}
 
void put_starting_point(Queue* queue)
{
    int i;
    for (i = 0; i < n; i++) 
    {
        if (count[i] == 0) 
        {
            queue_enqueue(queue, i);
        }
    }
}
 
void init(Node* nodes) 
{
    int i;
    for (i = 0; i < MAX_N; i++) 
    {
        nodes[i].item = i;
        nodes[i].prev = NULL;
    }
}
 
void reset(Stack* stack, Queue* queue, Node* nodes) 
{
    int i, j;
    for (i = 0; i < MAX_N; i++) 
    {
        for (j = 0; j < MAX_N; j++) 
        {
            map[i][j] = 0;
        }
    }
    for (i = 0; i < MAX_N; i++) 
    {
        count[i] = 0;
    }
 
    stack_reset(stack);
    queue_reset(queue);
    for (i = 0; i < MAX_N; i++) 
    {
        node_reset(&nodes[i]);
    }
}
 
 
void traverse(Node* nodes, const int idx, Stack* stack) 
{
    stack_set_push(stack, nodes[idx].item);
 
    Node* cur = nodes[idx].prev;
    while (cur) 
    {
        traverse(nodes, cur->item, stack);
        cur = cur->prev;
    }
}
 
int main(void) 
{
    int dest, tc, i;
    Queue queue;
    Stack stack;
    Node nodes[MAX_N];
    init(nodes);
 
    scanf("%d", &test_case);
 
    for (tc = 1; tc <= test_case; tc++) 
    {
        scanf("%d %d", &n, &m);
        scanf("%d", &dest);
 
        reset(&stack, &queue, nodes);
 
        for (i = 0; i < m; i++) 
        {
            int src, dest;
            scanf("%d %d", &src, &dest);
            map[src - 1][dest - 1] = CONNECTED;
            count[dest - 1]++;
        }
 
        put_starting_point(&queue);
 
        while (queue_has_item(&queue)) 
        {
            int src = queue_dequeue(&queue);
            for (i = 0; i < n; i++)
            {
                if (connected(src, i)) 
                {
                    Node* node = (Node*) malloc(sizeof(Node));
                    node->item = src;
                    node->prev = NULL;
                    node_push(&nodes[i], node);
 
                    count[i]--;
                    if (count[i] == 0)
                    {
                        queue_enqueue(&queue, i);
                    }
                }
            }
        }
 
        printf("#%d  ", tc);
        if (!nodes[dest - 1].prev) 
        {
            printf("Not reached");
        } 
        else
        {
            traverse(nodes, dest - 1, &stack);
            while (stack_has_item(&stack)) 
            {
                int item = stack_pop(&stack);
                if (item == DUPLICATE)
                {
                    continue;
                }
 
                printf("%d ", item + 1);
            }
        }
        printf("\n");
    }
 
    return 0;
}
```

---

```maximum flow
#include <stdio.h>
 
#define MAX_V 10
 
const int INF = 987654321;
int V;
 
typedef struct
{
    int queueArray[MAX_V];
    int front;
    int rear;
}Queue;
 
void push(Queue *q, int item)  
{
    if ((q->rear + 1) % MAX_V == q->front)
    {
        return;
    }
    q->queueArray[q->rear] = item;
    q->rear = (q->rear + 1) % MAX_V;
}
void pop(Queue * q)
{
    if (q->front == q->rear)
    {
        return;
    }
    q->front = (q->front + 1) % MAX_V;
}
 
int getFront(Queue * q)
{
    return q->queueArray[q->front];
}
 
int isEmpty(Queue *q)
{
    if (q->rear == q->front) 
    { 
        return 1;
    }
    else
    {
        return 0;
    }
}
 
int networkFlow(int source, int sink, int capacity[][MAX_V])
{
    int flow[MAX_V][MAX_V] = { 0, };
    int parent[MAX_V];
    int totalFlow = 0;
    int p;
    while (1)
    {
        for (p = 0; p < V; p++)
        {
            parent[p] = -1;
        }
 
        Queue q;
         
        q.front = 0;
        q.rear = 0;
 
        parent[source] = source;
        push(&q, source);
 
        while (!isEmpty(&q)) 
        {
            int here = getFront(&q); pop(&q);
            int there;
            for (there = 0; there < V; ++there) 
            {
                if (capacity[here][there] - flow[here][there] > 0 && parent[there] == -1)
                {
                    push(&q, there);
                    parent[there] = here;
                }
            }
        }
        if (parent[sink] == -1)
        {
            break;
        }
 
        int amount = INF;
        for (p = sink; p != source; p = parent[p]) 
        {
            if (capacity[parent[p]][p] - flow[parent[p]][p] > amount) 
            {
                amount = amount;
            }
            else {
                amount = capacity[parent[p]][p] - flow[parent[p]][p];
            }
        }
 
        for (p = sink; p != source; p = parent[p]) 
        {
            flow[parent[p]][p] += amount;
            flow[p][parent[p]] -= amount;
        }
        totalFlow += amount;
    }
    return totalFlow;
}
 
int main(int argc, char** argv)
{
    int T;
 
    setbuf(stdout, NULL);
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; ++test_case)
    {
        int capacity[MAX_V][MAX_V] = { 0, };
        int E, here, there, C, answer;
 
        scanf("%d %d", &V, &E);
        for (int i = 0; i < E; ++i) 
        {
            scanf("%d %d %d", &here, &there, &C);
            capacity[here][there] = C;
        }
 
        answer = networkFlow(0, V - 1, capacity);
 
        printf("#%d %d\n", test_case, answer);
    }
    return 0;
}
```

---

```bipartite match
#include <stdio.h>
 
#define MAX 1000
 
int countA, countB;
int matchA[MAX];
int matchB[MAX];
int adj[MAX][MAX];
int visited[MAX];
 
int dfs(int a)
{
    int b;
 
    if (visited[a])
    {
        return 0;
    }
 
    visited[a] = 1;
 
    for (b = 0; b < countB; ++b)
    {
        if (adj[a][b] && (matchB[b] == -1 || dfs(matchB[b])))
        {
            matchA[a] = b;
            matchB[b] = a;
            return 1;
        }
    }
 
    return 0;
}
 
int bipartiteMatch(void)
{
    int size = 0;
    int start;
    int i;
    for (start = 0; start < countA; ++start)
    {
        for (i = 0; i < countA; i++)
        {
            visited[i] = 0;
        }
        if (dfs(start))
        {
            size++;
        }
    }
    return size;
}
 
void initialize(void) 
{
    int i, j;
    for (i = 0; i < countA; i++)
    {
        matchA[i] = -1;
        for (j = 0; j < countB; j++)
        {
            adj[i][j] = 0;
        }
    }
 
    for (i = 0; i < countB; i++)
    {
        matchB[i] = -1;
    }
}
 
int main(int argc, char* argv[]) 
{
 
    int T, adjCount;
     
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &countA);
        scanf("%d", &countB);
 
        initialize();
 
        scanf("%d", &adjCount);
 
        for (int i = 0; i < adjCount; i++) 
        {
            int a, b;
            scanf("%d", &a);
            scanf("%d", &b);
            adj[a - 1][b - 1] = 1;
        }
        printf("#%d %d\n", test_case, bipartiteMatch());
    }
    return 0;
}
```