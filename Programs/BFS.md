#include <stdio.h>

int graph[10][10], visited[10], n; int queue[10];

void BFS(int start) { int front = 0, rear = 0;

visited[start] = 1;
queue[rear++] = start;

while (front < rear)
{
    int vertex = queue[front++];
    printf("%d ", vertex);

    for (int i = 0; i < n; i++)
    {
        if (graph[vertex][i] == 1 && visited[i] == 0)
        {
            visited[i] = 1;
            queue[rear++] = i;
        }
    }
}
}

int main() { int i, j, start;

printf("Enter number of vertices: ");
scanf("%d", &n);

printf("Enter adjacency matrix:\n");

for (i = 0; i < n; i++)
{
    for (j = 0; j < n; j++)
    {
        scanf("%d", &graph[i][j]);
    }
}

for (i = 0; i < n; i++)
    visited[i] = 0;

printf("Enter starting vertex (0 to %d): ", n - 1);
scanf("%d", &start);

printf("BFS Traversal: ");
BFS(start);

return 0;
} 

<img width="305" height="180" alt="Screenshot 2026-10-22 at 11 10 45 PM" src="https://github.com/user-attachments/assets/854d56b9-39dc-4806-ae00-a0aaa7075b22" />
