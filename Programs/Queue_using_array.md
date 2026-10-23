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
<img width="272" height="448" alt="Screenshot 2026-10-22 at 9 19 12 PM" src="https://github.com/user-attachments/assets/a9e6ba1d-32dc-4a91-b369-055350b6b65e" />
<img width="258" height="304" alt="Screenshot 2026-10-22 at 9 19 19 PM" src="https://github.com/user-attachments/assets/1817f3d5-d33f-4cf2-92ad-ef5109140e96" />

