#include<stdio.h>

int main()
{
    int low,high,key,n,found=0,i,mid;

    printf("Array size:");
    scanf("%d",&n);

    int a[n];

    printf("Enter array elements:");
    for(i=0;i<n;i++)
    {
        scanf("%d",&a[i]);
    }

    printf("Enter search element:");
    scanf("%d",&key);

    low=0;
    high=n-1;

    while(low<=high)
    {
        mid=(low+high)/2;

        if(a[mid]==key)
        {
            found=1;
            break;
        }
        else if(a[mid]>key)
        {
            high=mid-1;
        }
        else if(a[mid]<key)
        {
            low=mid+1;
        }
    }

    if(found)
    {
        printf("Element is found at %d location",mid+1);
    }
    else
    {
        printf("Element not found");
    }

    return 0;
}

<img width="247" height="85" alt="Screenshot 2026-10-22 at 9 05 24 PM" src="https://github.com/user-attachments/assets/cedc78bf-af98-4955-b1a0-1e5a6b953734" />
