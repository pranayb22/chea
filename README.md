********************************************************* DAA *************************************************

slip11a

#include <stdio.h>
#include <stdlib.h>

#define MAX_VERTICES 100

int visited[MAX_VERTICES];
int adjMatrix[MAX_VERTICES][MAX_VERTICES];
int vertexCount = 0;

void addEdge(int u, int v) {
adjMatrix[u][v] = 1;
adjMatrix[v][u] = 1;
}

void DFS(int vertex) {
printf("%d ", vertex);
visited[vertex] = 1;
    
for(int i = 0; i < vertexCount; i++) {
if(adjMatrix[vertex][i] == 1 && !visited[i]) {
DFS(i);
}
}
}

int main() {
vertexCount = 5;    
addEdge(0, 1);
addEdge(0, 2);
addEdge(1, 3);
addEdge(2, 4);
    
printf("DFS Traversal: ");
DFS(0);
    
return 0;
}

---------------------------------------------------------------------------------------------------------------
slip11b

#include <stdio.h>
#include <limits.h>
#include <stdbool.h>

#define V 6

int minDistance(int dist[], bool sptSet[]) {
int min = INT_MAX, min_index;
for (int v = 0; v < V; v++) {
 if (sptSet[v] == false && dist[v] <= min) {
 min = dist[v];
 min_index = v;
}
}
return min_index;
}

void printSolution(int dist[]) {
printf("Vertex \t\t Distance from Source\n");
for (int i = 0; i < V; i++) {
printf("%d \t\t %d\n", i, dist[i]);
}
}

void dijkstra(int graph[V][V], int src) {
int dist[V];
bool sptSet[V];
    
for (int i = 0; i < V; i++) {
dist[i] = INT_MAX;
sptSet[i] = false;
}
    
dist[src] = 0;
    
for (int count = 0; count < V - 1; count++) {
int u = minDistance(dist, sptSet);
sptSet[u] = true;
        
for (int v = 0; v < V; v++) {
if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX 
&& dist[u] + graph[u][v] < dist[v]) {
dist[v] = dist[u] + graph[u][v];
}
}
}
    
printSolution(dist);
}

int main() {
int graph[V][V] = {
{0, 4, 0, 0, 0, 0},
{4, 0, 8, 0, 0, 0},
{0, 8, 0, 7, 0, 4},
{0, 0, 7, 0, 9, 14},
{0, 0, 0, 9, 0, 10},
{0, 0, 4, 14, 10, 0}
};
    
dijkstra(graph, 0);
return 0;
}

******************************************************************************************************************

slip7a

#include <stdio.h>
#include <limits.h>
#include <stdbool.h>

#define V 9

int minDistance(int dist[], bool sptSet[]) {
int min = INT_MAX, min_index;
for (int v = 0; v < V; v++)
if (sptSet[v] == false && dist[v] <= min)
min = dist[v], min_index = v;
return min_index;
}

void printSolution(int dist[]) {
printf("Vertex \t\t Distance from Source\n");
for (int i = 0; i < V; i++)
printf("%d \t\t %d\n", i, dist[i]);
}

void dijkstra(int graph[V][V], int src) {
int dist[V];
bool sptSet[V];
for (int i = 0; i < V; i++)
dist[i] = INT_MAX, sptSet[i] = false;
dist[src] = 0;
for (int count = 0; count < V - 1; count++) {
int u = minDistance(dist, sptSet);
sptSet[u] = true;
for (int v = 0; v < V; v++)
if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX
&& dist[u] + graph[u][v] < dist[v])
dist[v] = dist[u] + graph[u][v];
}
printSolution(dist);
}

int main() {
int graph[V][V] = { { 0, 4, 0, 0, 0, 0, 0, 8, 0 },
{ 4, 0, 8, 0, 0, 0, 0, 11, 0 },
{ 0, 8, 0, 7, 0, 4, 0, 0, 2 },
{ 0, 0, 7, 0, 9, 14, 0, 0, 0 },
{ 0, 0, 0, 9, 0, 10, 0, 0, 0 },
{ 0, 0, 4, 14, 10, 0, 2, 0, 0 },
{ 0, 0, 0, 0, 0, 2, 0, 1, 6 },
{ 8, 11, 0, 0, 0, 0, 1, 0, 7 },
{ 0, 0, 2, 0, 0, 0, 6, 7, 0 } };
dijkstra(graph, 0);
return 0;
}

----------------------------------------------------------------------------------------------------
slip7b

#include <stdio.h>
#include <stdlib.h>

#define MAX 6

int adj[MAX][MAX] = {
{0, 0, 0, 0, 0, 0},
{0, 0, 0, 0, 0, 0},
{0, 0, 0, 1, 0, 0},
{0, 0, 0, 0, 1, 0},
{1, 1, 0, 0, 0, 0},
{1, 0, 1, 0, 0, 0}
};

int topOrder[MAX];
int idx;

void topologicalSortUtil(int v, int visited[]) {
visited[v] = 1;
for (int i = 0; i < MAX; i++) {
if (adj[v][i] && !visited[i]) {
topologicalSortUtil(i, visited);
}
}
topOrder[--idx] = v;
}

void topologicalSort() {
int visited[MAX] = {0};
idx = MAX;
for (int i = 0; i < MAX; i++) {
if (!visited[i]) {
topologicalSortUtil(i, visited);
}
}
}

int main() {
topologicalSort();
printf("Topological order: ");
for (int i = 0; i < MAX; i++) {
printf("%d ", topOrder[i]);
}
return 0;
}

********************************************************************************************************************

slip12a

#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#include <time.h>

#define MAX_VERTICES 100

int adjMatrix[MAX_VERTICES][MAX_VERTICES];
int visited[MAX_VERTICES];
int vertexCount = 5;

typedef struct {
int items[MAX_VERTICES];
int front;
int rear;
} Queue;

Queue* createQueue() {
Queue* q = (Queue*)malloc(sizeof(Queue));
q->front = -1;
q->rear = -1;
return q;
}

int isEmpty(Queue* q) {
return q->rear == -1;
}

void enqueue(Queue* q, int value) {
if (q->rear == MAX_VERTICES - 1)
return;
if (q->front == -1)
q->front = 0;
q->rear++;
q->items[q->rear] = value;
}

int dequeue(Queue* q) {
int item;
if (isEmpty(q)) {
item = -1;
} else {
item = q->items[q->front];
q->front++;
if (q->front > q->rear) {
q->front = q->rear = -1;
}
}
return item;
}

void addEdge(int u, int v) {
adjMatrix[u][v] = 1;
adjMatrix[v][u] = 1;
}

void BFS(int startVertex) {
Queue* q = createQueue();
    
visited[startVertex] = 1;
enqueue(q, startVertex);
    
while (!isEmpty(q)) {
int currentVertex = dequeue(q);
printf("%d ", currentVertex);
        
for (int i = 0; i < vertexCount; i++) {
if (adjMatrix[currentVertex][i] && !visited[i]) {
visited[i] = 1;
enqueue(q, i);
}
}
}
free(q);
}

int main() {
addEdge(0, 1);
addEdge(0, 2);
addEdge(1, 3);
addEdge(2, 4);
    
clock_t start = clock();
printf("BFS Traversal: ");
BFS(0);
clock_t end = clock();
    
double time_taken = ((double)(end - start)) / CLOCKS_PER_SEC;
printf("\nTime taken: %f seconds\n", time_taken);
    
printf("Time Complexity: O(V + E)\n");
printf("Where V = Number of vertices, E = Number of edges\n");
    
return 0;
}

----------------------------------------------------------------------------------------------------
slip12b

#include <stdio.h>
#include <time.h>

void selectionSort(int arr[], int n) {
for (int i = 0; i < n-1; i++) {
int min_idx = i;
for (int j = i+1; j < n; j++) {
if (arr[j] < arr[min_idx]) {
min_idx = j;
}
}
int temp = arr[min_idx];
arr[min_idx] = arr[i];
arr[i] = temp;
}
}

int main() {
int arr[] = {64, 25, 12, 22, 11};
int n = sizeof(arr)/sizeof(arr[0]);
    
clock_t start = clock();
selectionSort(arr, n);
clock_t end = clock();
    
printf("Sorted array: ");
for (int i = 0; i < n; i++) {
printf("%d ", arr[i]);
}
    
double time_taken = ((double)(end - start)) / CLOCKS_PER_SEC;
printf("\nTime taken: %f seconds", time_taken);
printf("\nTime Complexity: O(n²) in all cases");
    
return 0;
}
*********************************************************************************************************
slip13a

#include <stdio.h>
#include <limits.h>

int matrixChainOrder(int p[], int n) {
int m[n][n];
int i, j, k, L, q;

for (i = 1; i < n; i++)
m[i][i] = 0;

for (L = 2; L < n; L++) {
for (i = 1; i < n - L + 1; i++) {
j = i + L - 1;
m[i][j] = INT_MAX;
for (k = i; k <= j - 1; k++) {
q = m[i][k] + m[k + 1][j] + p[i - 1] * p[k] * p[j];
if (q < m[i][j])
m[i][j] = q;
}
}
}
return m[1][n - 1];
}

int main() {
int arr[] = {10, 20, 30, 40, 30};
int size = sizeof(arr) / sizeof(arr[0]);

printf("Minimum number of multiplications is %d\n", matrixChainOrder(arr, size));
return 0;
}

---------------------------------------------------------------------------------------------------------
slip13b

#include <stdio.h>
#include <limits.h>

#define MAX 100

void optimalBST(int keys[], int freq[], int n) {
int cost[n][n];
int root[n][n];

for (int i = 0; i < n; i++) {
cost[i][i] = freq[i];
root[i][i] = i;
}

for (int L = 2; L <= n; L++) {
for (int i = 0; i <= n - L; i++) {
int j = i + L - 1;
cost[i][j] = INT_MAX;
int sum = 0;
for (int s = i; s <= j; s++)
sum += freq[s];

for (int r = i; r <= j; r++) {
int c = ((r > i) ? cost[i][r-1] : 0) + 
((r < j) ? cost[r+1][j] : 0) + 
sum;
if (c < cost[i][j]) {
cost[i][j] = c;
root[i][j] = r;
}
}
}
}

printf("Optimal BST Cost: %d\n", cost[0][n-1]);
printf("Root of OBST: %d\n", keys[root[0][n-1]]);
}

int main() {
int keys[] = {10, 20, 30, 40};
int freq[] = {4, 2, 6, 3};
int n = sizeof(keys)/sizeof(keys[0]);

optimalBST(keys, freq, n);

printf("\nComplexity Analysis:\n");
printf("Best Case: O(n^2) - When all frequencies are equal\n");
printf("Worst Case: O(n^3) - General case\n");
printf("Space Complexity: O(n^2)\n");

return 0;
}


**********************************************************************************************************
slip14a

#include <stdio.h>
#include <time.h>

void insertionSort(int arr[], int n) {
int i, key, j;
for (i = 1; i < n; i++) {
key = arr[i];
j = i - 1;
while (j >= 0 && arr[j] > key) {
arr[j + 1] = arr[j];
j = j - 1;
}
arr[j + 1] = key;
}
}

void printArray(int arr[], int n) {
for (int i = 0; i < n; i++) {
printf("%d ", arr[i]);
}
printf("\n");
}

int main() {
int arr[] = {12, 11, 13, 5, 6};
int n = sizeof(arr)/sizeof(arr[0]);
    
clock_t start = clock();
insertionSort(arr, n);
clock_t end = clock();
    
double time_taken = ((double)(end - start)) / CLOCKS_PER_SEC;
    
printf("Sorted array: ");
printArray(arr, n);    
printf("Time taken: %f seconds\n", time_taken);
return 0;
}
