#include<stdio.h>
#include<stdlib.h>

struct Node
{
    int data;
    struct Node*prev;
    struct Node*next;
};

struct Node*front=NULL;
struct Node*rear=NULL;

void enqueue()
{
    int value;
    struct Node*newNode=(struct Node*)malloc(sizeof(struct Node));

    printf("Enter element to insert: ");
    scanf("%d",&value);

    newNode->data=value;
    newNode->prev=NULL;
    newNode->next=NULL;

    if(rear==NULL)
    {
        front=rear=newNode;
    }
    else
    {
        rear->next=newNode;
        newNode->prev=rear;
        rear=newNode;
    }

    printf("Element inserted: %d\n",value);
}

void dequeue()
{
    struct Node*temp;

    if(front==NULL)
    {
        printf("Queue Underflow\n");
        return;
    }

    temp=front;

    printf("Element deleted: %d\n",front->data);

    front=front->next;

    if(front==NULL)
    {
        rear=NULL;
    }
    else
    {
        front->prev=NULL;
    }

    free(temp);
}

void display()
{
    struct Node*temp;

    if(front==NULL)
    {
        printf("Queue is empty\n");
        return;
    }

    temp=front;

    printf("Queue is:\n");

    while(temp!=NULL)
    {
        printf("%d ",temp->data);
        temp=temp->next;
    }

    printf("\n");
}

int main()
{
    int choice;

    while(1)
    {
        printf("\n1.Insert\n");
        printf("2.Delete\n");
        printf("3.Display\n");
        printf("4.Exit\n");

        printf("Enter your choice: ");
        scanf("%d",&choice);

        switch(choice)
        {
            case 1:
                enqueue();
                break;

            case 2:
                dequeue();
                break;

            case 3:
                display();
                break;

            case 4:
                exit(0);

            default:
                printf("Wrong choice\n");
        }
    }

    return 0;
}
