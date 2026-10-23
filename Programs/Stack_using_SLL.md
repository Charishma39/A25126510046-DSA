#include<stdio.h>
#include<stdlib.h>

struct Node
{
    int data;
    struct Node*next;
};

struct Node*top=NULL;

struct Node*createNode(int value)
{
    struct Node*newNode=(struct Node*)malloc(sizeof(struct Node));

    newNode->data=value;
    newNode->next=NULL;

    return newNode;
}

void push(int value)
{
    struct Node*newNode=createNode(value);

    newNode->next=top;
    top=newNode;

    printf("%d pushed into stack\n",value);
}

void pop()
{
    struct Node*temp;

    if(top==NULL)
    {
        printf("Stack is empty\n");
        return;
    }

    temp=top;

    printf("Popped element:%d\n",top->data);

    top=top->next;

    free(temp);
}

void peek()
{
    if(top==NULL)
    {
        printf("Stack is empty\n");
        return;
    }

    printf("Top element:%d\n",top->data);
}

void displayList()
{
    struct Node*temp=top;

    if(top==NULL)
    {
        printf("Stack is empty\n");
        return;
    }

    printf("Stack elements:\n");

    while(temp!=NULL)
    {
        printf("%d\n",temp->data);
        temp=temp->next;
    }
}

int main()
{
    int choice,value;

    while(1)
    {
        printf("\n1.Push\n");
        printf("2.Pop\n");
        printf("3.Peek\n");
        printf("4.Display\n");
        printf("5.Exit\n");
        printf("Enter your choice:");
        scanf("%d",&choice);

        switch(choice)
        {
            case 1:
                printf("Enter value:");
                scanf("%d",&value);
                push(value);
                break;

            case 2:
                pop();
                break;

            case 3:
                peek();
                break;

            case 4:
                displayList();
                break;

            case 5:
                exit(0);

            default:
                printf("Invalid choice\n");
        }
    }

    return 0;
}
<img width="196" height="494" alt="Screenshot 2026-10-22 at 9 25 31 PM" src="https://github.com/user-attachments/assets/ac98e2c2-3152-487d-a718-6ba7d6476ec9" />
<img width="160" height="273" alt="Screenshot 2026-10-22 at 9 25 39 PM" src="https://github.com/user-attachments/assets/c2dc1eea-c940-4c5c-b6e8-f821d350ca0a" />

