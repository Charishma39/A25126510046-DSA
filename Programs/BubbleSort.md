#include<stdio.h>

int main()
{
    int n,i,j,t;
    printf("Enter array size:");
    scanf("%d",&n);

    int a[n];

    printf("Enter array values:");
    for(i=0;i<n;i++)
    {
        scanf("%d",&a[i]);
    }

    printf("\nValues before sorting\n");
    for(i=0;i<n;i++)
    {
        printf("%d ",a[i]);
    }

    printf("\nSorting in process...");

    for(i=0;i<n-1;i++)
    {
        for(j=0;j<n-1-i;j++)
        {
            if(a[j]>a[j+1])
            {
                t=a[j];
                a[j]=a[j+1];
                a[j+1]=t;
            }
        }
    }

    printf("\nValues after sorting\n");
    for(i=0;i<n;i++)
    {
        printf("%d ",a[i]);
    }

    return 0;
}
