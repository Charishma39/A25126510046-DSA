#include<stdio.h>
#include<stdlib.h>

#define MAX 5

void enqueue();
void dequeue();
void display();

int queue_array[MAX];
int rear=-1;
int front=-1;

void enqueue()
{
    int add_item;

    if(rear==MAX-1)
    {
        printf("Queue overflow\n");
    }
    else
    {
        if(front==-1)
        {
            front=0;
        }

        printf("Insert the element in queue:");
        scanf("%d",&add_item);

        rear=rear+1;
        queue_array[rear]=add_item;
    }
}

void dequeue()
{
    if(front==-1||front>rear)
    {
        printf("Queue underflow\n");
        return;
    }
    else
    {
        printf("Element deleted from queue is:%d\n",queue_array[front]);
        front=front+1;
        printf("Front value after deletion:%d\n",front);
    }
}

void display()
{
    int i;

    if(front==-1||front>rear)
    {
        printf("Queue is empty\n");
    }
    else
    {
        printf("Queue is:\n");

        for(i=front;i<=rear;i++)
        {
            printf("%d ",queue_array[i]);
        }

        printf("\n");
    }
}

int main()
{
    int choice;

    while(1)
    {
        printf("\n*** MENU ***\n");
        printf("1.Insert\n");
        printf("2.Delete\n");
        printf("3.Display\n");
        printf("4.Exit\n");
        printf("Enter your choice:");
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
