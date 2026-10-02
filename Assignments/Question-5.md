#include <stdio.h>
#include <stdlib.h>
struct Node
{
    int data;
    struct Node *next;
};
struct Node *head = NULL;
void insertBeginning(int x)
{
    struct Node *newNode;
    newNode = malloc(sizeof(struct Node));
    newNode->data = x;
    newNode->next = head;
    head = newNode;
}
void insertEnd(int x)
{
    struct Node *newNode, *temp;
    newNode = malloc(sizeof(struct Node));
    newNode->data = x;
    newNode->next = NULL;
    if (head == NULL)
    {
        head = newNode;
        return;
    }
    temp = head;

    while (temp->next != NULL)
        temp = temp->next;
    temp->next = newNode;
}
void search(int x)
{
    struct Node *temp = head;
    while (temp != NULL)
    {
        if (temp->data == x)
        {
            printf("%d found\n", x);
            return;
        }
        temp = temp->next;
    }
    printf("%d not found\n", x);
}
void deleteNode(int x)
{
    struct Node *temp = head;
    struct Node *prev = NULL;

    while (temp != NULL && temp->data != x)
    {
        prev = temp;
        temp = temp->next;
    }
    if (temp == NULL)
    {
        printf("%d not found\n", x);
        return;
    }
    if (prev == NULL)
        head = temp->next;
    else
        prev->next = temp->next;
    free(temp);

    printf("%d deleted\n", x);
}
void display()
{
    struct Node *temp = head;

    while (temp != NULL)
    {
        printf("%d ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}
int main()
{
    insertBeginning(20);
    insertBeginning(10);
    insertEnd(30);
    display();
    search(20);
    deleteNode(20);
    display();
    return 0;
}
<img width="537" height="151" alt="image" src="https://github.com/user-attachments/assets/020b9206-7199-4d16-bafa-07f0bfc76e3a" />
