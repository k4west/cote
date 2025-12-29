Data Structure

---

```stack
#include <stdio.h>
 
#define MAX_N 100
 
int top;
int stack[MAX_N];
 
void stackInit(void)
{
    top = 0;
}
 
int stackIsEmpty(void)
{
    return (top == 0);
}
 
int stackIsFull(void)
{
    return (top == MAX_N);
}
 
int stackPush(int value)
{
    if (stackIsFull())
    {
        printf("stack overflow!");
        return 0;
    }
    stack[top] = value;
    top++;
 
    return 1;
}
 
int stackPop(int *value)
{
    if (stackIsEmpty())
    {
        printf("stack is empty!");
        return 0;
    }
    top--;
    *value = stack[top];
 
    return 1;
}
 
int main(int argc, char* argv[])
{
    int T, N;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &N);
        stackInit();
        for (int i = 0; i < N; i++) 
        {
            int value;
            scanf("%d", &value);
            stackPush(value);
        }
 
        printf("#%d ", test_case);
 
        while (!stackIsEmpty())
        {
            int value;
            if (stackPop(&value) == 1)
            {
                printf("%d ", value);
            }
        }
        printf("\n");
    }
    return 0;
}
```

---

```queue
#include <stdio.h>
 
#define MAX_N 100
 
int front;
int rear;
int queue[MAX_N];
 
void queueInit(void)
{
    front = 0;
    rear = 0;
}
 
int queueIsEmpty(void)
{
    return (front == rear);
}
 
int queueIsFull(void)
{
    if ((rear + 1) % MAX_N == front)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
 
int queueEnqueue(int value)
{
    if (queueIsFull())
    {
        printf("queue is full!");
        return 0;
    }
    queue[rear] = value;
    rear++;
    if (rear == MAX_N)
    {
        rear = 0;
    }
 
    return 1;
}
 
int queueDequeue(int *value)
{
    if (queueIsEmpty())
    {
        printf("queue is empty!");
        return 0;
    }
    *value = queue[front];
    front++;
    if (front == MAX_N)
    {
        front = 0;
    }
    return 1;
}
 
int main(int argc, char* argv[])
{
    int T;
    int N;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &N);
 
        queueInit();
        for (int i = 0; i < N; i++)
        {
            int value;
            scanf("%d", &value);
            queueEnqueue(value);
            printf("setValue");
        }
 
        printf("#%d ", test_case);
 
        while (!queueIsEmpty())
        {
            int value;
            if (queueDequeue(&value) == 1)
            {
                printf("%d ", value);
            }
        }
        printf("\n");
    }
    return 0;
}
```

---

```priority queue
#include <stdio.h>
 
#define MAX_SIZE 100
 
int heap[MAX_SIZE];
int heapSize = 0;
 
void heapInit(void)
{
    heapSize = 0;
}
 
int heapPush(int value)
{
    if (heapSize + 1 > MAX_SIZE)
    {
        printf("queue is full!");
        return 0;
    }
 
    heap[heapSize] = value;
 
    int current = heapSize;
    while (current > 0 && heap[current] < heap[(current - 1) / 2]) 
    {
        int temp = heap[(current - 1) / 2];
        heap[(current - 1) / 2] = heap[current];
        heap[current] = temp;
        current = (current - 1) / 2;
    }
 
    heapSize = heapSize + 1;
 
    return 1;
}
 
int heapPop(int *value)
{
    if (heapSize <= 0)
    {
        return -1;
    }
 
    *value = heap[0];
    heapSize = heapSize - 1;
 
    heap[0] = heap[heapSize];
 
    int current = 0;
    while (current * 2 + 1 < heapSize)
    {
        int child;
        if (current * 2 + 2 == heapSize)
        {
            child = current * 2 + 1;
        }
        else
        {
            child = heap[current * 2 + 1] < heap[current * 2 + 2] ? current * 2 + 1 : current * 2 + 2;
        }
 
        if (heap[current] < heap[child])
        {
            break;
        }
 
        int temp = heap[current];
        heap[current] = heap[child];
        heap[child] = temp;
 
        current = child;
    }
    return 1;
}
 
int main(int argc, char* argv[])
{
    int T, N;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &N);
         
        heapInit();
         
        for (int i = 0; i < N; i++)
        {
            int value;
            scanf("%d", &value);
            heapPush(value);
        }
 
        printf("#%d ", test_case);
 
        for (int i = 0; i < N; i++)
        {
            int value;
            heapPop(&value);
            printf("%d ", value);
        }
        printf("\n");
    }
    return 0;
}
```

---

```hash
#include <stdio.h>
#include <string.h>
#include <memory.h>
 
#define MAX_KEY 64
#define MAX_DATA 128
#define MAX_TABLE 4096
 
typedef struct
{
    char key[MAX_KEY + 1];
    char data[MAX_DATA + 1];
}Hash;
Hash tb[MAX_TABLE];
 
unsigned long hash(const char *str)
{
    unsigned long hash = 5381;
    int c;
 
    while (c = *str++)
    {
        hash = (((hash << 5) + hash) + c) % MAX_TABLE;
    }
 
    return hash % MAX_TABLE;
}
 
int find(const char *key, char *data)
{
    unsigned long h = hash(key);
    int cnt = MAX_TABLE;
 
    while (tb[h].key[0] != 0 && cnt--)
    {
        if (strcmp(tb[h].key, key) == 0)
        {
            strcpy(data, tb[h].data);
            return 1;
        }
        h = (h + 1) % MAX_TABLE;
    }
    return 0;
}
 
int add(const char *key, char *data)
{
    unsigned long h = hash(key);
 
    while (tb[h].key[0] != 0)
    {
        if (strcmp(tb[h].key, key) == 0)
        {
            return 0;
        }
 
        h = (h + 1) % MAX_TABLE;
    }
    strcpy(tb[h].key, key);
    strcpy(tb[h].data, data);
    return 1;
}
 
 
int main(int argc, char* argv[])
{
    int T, N, Q;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        memset(tb, 0, sizeof(tb));
        scanf("%d", &N);
        char k[MAX_KEY + 1];
        char d[MAX_DATA + 1];
 
        for (int i = 0; i < N; i++)
        {
            scanf("%s %s\n", &k, &d);
            add(k, d);
        }
 
        printf("#%d\n", test_case);
 
        scanf("%d", &Q);
        for (int i = 0; i < Q; i++)
        {
            char k[MAX_KEY + 1];
            char d[MAX_DATA + 1];
 
            scanf("%s\n", &k);
 
            if (find(k, d))
            {
                printf("%s\n", d);
            }
            else
            {
                printf("not find\n");
            }
        }
    }
    return 0;
}
```

---

```tree
#include <stdio.h>
 
#define MAX_NODE_NUM 10000
#define MAX_CHILD_NUM 2
 
typedef struct
{
    int parent;
    int child[MAX_CHILD_NUM];
} TreeNode;
TreeNode tree[MAX_NODE_NUM];
int nodeNum;
int edgeNum;
int root;
 
void initTree(void) 
{
    int i;
    int j;
    for (i = 0; i <= nodeNum; i++)
    {
        tree[i].parent = -1;
        for (j = 0; j < MAX_CHILD_NUM; j++)
        {
            tree[i].child[j] = -1;
        }
    }
}
 
void addChild(int parent, int child) 
{
    int i;
    for (i = 0; i < MAX_CHILD_NUM; i++)
    {
        if (tree[parent].child[i] == -1)
        {
            break;
        }
    }
    tree[parent].child[i] = child;
    tree[child].parent = parent;
}
 
int getRoot(void) 
{
    int i;
    int j;
    for (i = 1; i <= nodeNum; i++) 
    {
        if (tree[i].parent == -1) 
        {
            return i;
        }
    }
    return -1;
}
 
void preOrder(int root) 
{
    int i;
    int child;
    printf("%d ", root);
 
    for (i = 0; i < MAX_CHILD_NUM; i++) 
    {
        child = tree[root].child[i];
        if (child != -1)
        {
            preOrder(child);
        }
    }
}
 
int main(void)
{
    int test_case;
    int T;
    int i;
    int parent;
    int child;
 
    scanf("%d", &T);
 
    for (test_case = 1; test_case <= T; ++test_case) 
    {
        scanf("%d %d", &nodeNum, &edgeNum);
 
        initTree();
 
        for (i = 0; i < edgeNum; i++)
        {
            scanf("%d %d", &parent, &child);
            addChild(parent, child);
        }
 
        root = getRoot();
 
        printf("#%d ", test_case);
        preOrder(root);
        printf("\n");
    }
 
    return 0;
}
```

---

```graph
#include <stdio.h>
#include <malloc.h>
 
typedef struct adjlistNode
{
    int vertex;
    adjlistNode *next;
} AdjlistNode;
 
typedef struct
{
    int num_members;
    AdjlistNode *head;
    AdjlistNode *tail;
} AdjList;
 
typedef struct
{
    int num_vertices;
    AdjList * adjListArr;
} Graph;
 
AdjlistNode * createNode(int v)
{
    AdjlistNode * newNode = (AdjlistNode *)malloc(sizeof(AdjlistNode));
 
    newNode->vertex = v;
    newNode->next = NULL;
 
    return newNode;
}
 
Graph * createGraph(int n)
{
 
    Graph * graph = (Graph *)malloc(sizeof(Graph));
    graph->num_vertices = n;
 
    graph->adjListArr = (AdjList *)malloc(n * sizeof(AdjList));
 
    for (int i = 0; i < n; i++)
    {
        graph->adjListArr[i].head = graph->adjListArr[i].tail = NULL;
        graph->adjListArr[i].num_members = 0;
    }
 
    return graph;
}
 
void destroyGraph(Graph * graph)
{
    if (graph)
    {
        if (graph->adjListArr)
        {
            for (int v = 0; v < graph->num_vertices; v++)
            {
                AdjlistNode * adjListPtr = graph->adjListArr[v].head;
                while (adjListPtr)
                {
                    AdjlistNode * tmp = adjListPtr;
                    adjListPtr = adjListPtr->next;
                    free(tmp);
                }
            }
            free(graph->adjListArr);
        }
        free(graph);
    }
}
 
void addEdge(Graph *graph, int src, int dest)
{
    AdjlistNode * newNode = createNode(dest);
    if (graph->adjListArr[src].tail != NULL) 
    {
        graph->adjListArr[src].tail->next = newNode;
        graph->adjListArr[src].tail = newNode;
    }
    else
    {
        graph->adjListArr[src].head = graph->adjListArr[src].tail = newNode;
    }
    graph->adjListArr[src].num_members++;
 
    newNode = createNode(src);
    if (graph->adjListArr[dest].tail != NULL) 
    {
        graph->adjListArr[dest].tail->next = newNode;
        graph->adjListArr[dest].tail = newNode;
    }
    else
    {
        graph->adjListArr[dest].head = graph->adjListArr[dest].tail = newNode;
    }
    graph->adjListArr[dest].num_members++;
}
 
void displayGraph(Graph * graph, int i)
{
 
    AdjlistNode * adjListPtr = graph->adjListArr[i].head;
    while (adjListPtr)
    {
        printf("%d ", adjListPtr->vertex);
        adjListPtr = adjListPtr->next;
    }
    printf("\n");
}
 
int main(int argc, char* argv[])
{
    int T, V, E, Q, sv, ev;
 
    scanf("%d", &T);
     
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d %d %d", &V, &E, &Q);
 
        Graph * graph = createGraph(V);
 
        for (int i = 0; i < E; i++)
        {
            scanf("%d %d", &sv, &ev);
            addEdge(graph, sv, ev);
        }
        printf("#%d\n", test_case);
 
        for (int i = 0; i < Q; i++)
        {
            scanf("%d", &sv);
            displayGraph(graph, sv);
        }
    }
 
    return 0;
}

```

---

```linked list
 #include<stdio.h>
#include<malloc.h>
 
#define NULL (0)
 
typedef struct ListNode
{
    int data;
    struct ListNode* prev;
    struct ListNode* next;
};
 
ListNode* list_create(int _data)
{
    ListNode* node = (ListNode*)malloc(sizeof(ListNode));
 
    node->prev = NULL;
    node->next = NULL;
 
    node->data = _data;
 
    return node;
}
 
ListNode* list_insert(ListNode* _head, ListNode* new_node)
{
    ListNode* next = _head->next;
 
    _head->next = new_node;
    new_node->next = next;
    new_node->prev = _head;
     
    if (next != NULL)
    {
        next->prev = new_node;
    }
 
    return new_node;
}
 
int list_erase(ListNode* head, int _data)
{
    ListNode* it = head->next;
    int ret = 0;
 
    while (it != NULL)
    {
        if (it->data == _data)
        {
            ListNode* prev = it->prev;
            ListNode* next = it->next;
            ListNode* tmp = it;
            it = it->next;
 
            prev->next = next;
            if (next != NULL)
            {
                next->prev = prev;
            }
             
            free(tmp);
            ret++;
        }
        else
        {
            it = it->next;
        }
    }
 
    return ret;
}
 
int main(int argc, char* argv[])
{
    int T, N;
    setbuf(stdout, NULL);
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        scanf("%d", &N);
 
        ListNode* head = list_create(NULL);
        printf("#%d", test_case);
        for (int i = 0; i < N; i++)
        {
            int mode, data;
            scanf("%d%d", &mode, &data);
 
            if (mode == 1)
            {
                ListNode* node = list_create(data);
 
                list_insert(head, node);
            }
            else if (mode == 2)
            {
                printf(" %d", list_erase(head, data));
            }
        }
 
        while (head != NULL)
        {
            ListNode* tmp = head;
            head = head->next;
            free(tmp);
        }
        printf("\n");
    }
    return 0;
}
```

---

```deque
#define _CRT_SECURE_NO_WARNINGS
 
#include<stdio.h>
 
#define MAX 100
 
int arr[MAX];
int front;
int rear;
int size;
 
void dequeInit(int n) {
    front = -1;
    rear = 0;
    size = n;
}
 
bool isFull() {
    return ((front == 0 && rear == size - 1) || front == rear + 1);
}
 
bool isEmpty() {
    return (front == -1);
}
 
void insertFront(int value) {
    if (isFull()) {
        printf("Overflow\n");
    }
 
    if (front == -1) {
        front = rear = 0;
    }
    else if (front == 0) {
        front = size - 1;
    }
    else {
        front = front - 1;
    }
 
    arr[front] = value;
}
 
void insertRear(int value) {
    if (isFull()) {
        printf("Overflow\n");
    }
 
    if (front == -1) {
        front = rear = 0;
    }
    else if (rear == size - 1) {
        rear = 0;
    }
    else {
        rear = rear + 1;
    }
 
    arr[rear] = value;
}
 
int getFront() {
    if (isEmpty()) {
        printf("Underflow\n");
        return -1;
    }
    return arr[front];
}
 
int getRear() {
    if (isEmpty() || rear < 0) {
        printf("Underflow\n");
        return -1;
    }
    return arr[rear];
}
 
void deleteFront() {
    if (isEmpty()) {
        printf("Underflow\n");
        return;
    }
 
    if (front == rear) {
        front = -1;
        rear = -1;
    }
    else if (front == size - 1) {
        front = 0;
    }
    else {
        front = front + 1;
    }
}
 
void deleteRear() {
    if (isEmpty()) {
        printf("Underflow\n");
        return;
    }
 
    if (front == rear) {
        front = -1;
        rear = -1;
    }
    else if (rear == 0) {
        rear = size - 1;
    }
    else {
        rear = rear - 1;
    }
}
 
int main(void) {
 
    int T, N, M;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; ++test_case) {
 
        scanf("%d%d", &N, &M);
 
        dequeInit(N);
 
        printf("#%d ", test_case);
 
        for (int i = 0; i < M; ++i) {
             
            int cmd, elem;
 
            scanf("%d", &cmd);
 
            switch (cmd) {
            case 1:
                scanf("%d", &elem);
                insertFront(elem);
                break;
            case 2:
                scanf("%d", &elem);
                insertRear(elem);
                break;
            case 3:
                printf("%d ", getFront());
                break;
            case 4:
                printf("%d ", getRear());
                break;
            case 5:
                deleteFront();
                break;
            case 6:
                deleteRear();
                break;
            }
        }
 
        printf("\n");
    }
 
    return 0;
}
```

---

```map
#define _CRT_SECURE_NO_WARNINGS
 
#include<stdio.h>
#include<malloc.h>
 
typedef struct Node {
    int key;
    int value;
    Node *left, *right;
};
 
Node *newNode(int k, int v) {
    Node *temp = (Node *)malloc(sizeof(Node));
    temp->key = k;
    temp->value = v;
    temp->left = temp->right = NULL;
    return temp;
}
 
Node *current;
 
Node *putRec(Node *node, int key, int value) {
    if (node == NULL)
        return newNode(key, value);
 
    if (key < node->key)
        node->left = putRec(node->left, key, value);
    else if (key > node->key)
        node->right = putRec(node->right, key, value);
    else
        node->value = value;
 
    return node;
}
 
void put(int key, int value) {
    current = putRec(current, key, value);
}
 
int findRec(Node *node, int key) {
    if (node != NULL) {
        if (key == node->key)
            return node->value;
 
        int ret = -1;
        ret = findRec(node->left, key);
        if (ret != -1)
            return ret;
 
        ret = findRec(node->right, key);
        if (ret != -1)
            return ret;
    }
 
    return -1;
}
 
bool contains(int key) {
    int ret = findRec(current, key);
    if (ret != -1)
        return true;
    return false;
}
 
int get(int key) {
    return findRec(current, key);
}
 
Node *minValueNode(Node *node) {
    Node *current = node;
 
    while (current->left != NULL)
        current = current->left;
 
    return current;
}
 
Node *removeRec(Node *node, int key) {
    if (node == NULL)
        return node;
 
    if (key < node->key)
        node->left = removeRec(node->left, key);
    else if (key > node->key)
        node->right = removeRec(node->right, key);
    else {
        if (node->left == NULL) {
            Node *temp = node->right;
            free(node);
            return temp;
        }
        else if (node->right == NULL) {
            Node *temp = node->left;
            free(node);
            return temp;
        }
 
        Node* temp = minValueNode(node->right);
        node->key = temp->key;
        node->value = temp->value;
        node->right = removeRec(node->right, temp->key);
    }
 
    return node;
}
 
void remove(int key) {
    current = removeRec(current, key);
}
 
int main(void) {
 
    int T, N;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; ++test_case) {
 
        current = NULL;
 
        scanf("%d", &N);
 
        printf("#%d ", test_case);
 
        for (int i = 0; i < N; ++i) {
 
            int cmd, key, value;
 
            scanf("%d%d", &cmd, &key);
 
            switch (cmd) {
            case 1:
                scanf("%d", &value);
                put(key, value);
                break;
            case 2:
                remove(key);
                break;
            case 3:
                int ret = get(key);
                printf("%d ", ret);
            }
        }
        printf("\n");
    }
}
```

---

```set
#define _CRT_SECURE_NO_WARNINGS
 
#include<stdio.h>
#include<malloc.h>
 
typedef struct Node {
    int key;
    Node *left, *right;
};
 
Node *newNode(int item) {
    Node *temp = (Node *)malloc(sizeof(Node));
    temp->key = item;
    temp->left = temp->right = NULL;
    return temp;
}
 
Node *current;
 
Node *addRec(Node *node, int key) {
    if (node == NULL)
        return newNode(key);
 
    if (key < node->key)
        node->left = addRec(node->left, key);
    else if (key > node->key)
        node->right = addRec(node->right, key);
 
    return node;
}
 
void add(int key) {
    current = addRec(current, key);
}
 
bool findRec(Node *node, int key) {
    if (node != NULL) {
        if (key == node->key)
            return true;
        if (findRec(node->left, key))
            return true;
        if (findRec(node->right, key))
            return true;
    }
 
    return false;
}
 
bool contains(int key) {
    return findRec(current, key);
}
 
void printAll(Node *node) {
    if (node != NULL) {
        printAll(node->left);
        printf("%d ", node->key);
        printAll(node->right);
    }
}
 
void printAll() {
    printAll(current);
}
 
Node *minValueNode(Node *node) {
    Node *current = node;
 
    while (current->left != NULL)
        current = current->left;
 
    return current;
}
 
Node *removeRec(Node *node, int key) {
    if (node == NULL)
        return node;
 
    if (key < node->key)
        node->left = removeRec(node->left, key);
    else if (key > node->key)
        node->right = removeRec(node->right, key);
    else {
        if (node->left == NULL) {
            Node *temp = node->right;
            free(node);
            return temp;
        }
        else if (node->right == NULL) {
            Node *temp = node->left;
            free(node);
            return temp;
        }
 
        Node* temp = minValueNode(node->right);
        node->key = temp->key;
        node->right = removeRec(node->right, temp->key);
    }
 
    return node;
}
 
void remove(int key) {
    current = removeRec(current, key);
}
 
int main(void) {
     
    int T, N;
 
    scanf("%d", &T);
 
    for (int test_case = 1; test_case <= T; ++test_case) {
 
        current = NULL;
 
        scanf("%d", &N);
 
        for (int i = 0; i < N; ++i) {
 
            int cmd, key;
 
            scanf("%d%d", &cmd, &key);
 
            switch (cmd) {
            case 1:
                add(key);
                break;
            case 2:
                remove(key);
                break;
            }
        }
 
        printf("#%d ", test_case);
        printAll();
        printf("\n");
    }
}
```
