#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<ctype.h>

#define MAX 50

int stack[MAX];
int top=-1;

void push(int item)
{
    if(top>=MAX-1)
    {
        printf("Stack overflow");
    }
    else
    {
        stack[++top]=item;
    }
}

int pop()
{
    if(top<0)
    {
        printf("Stack underflow");
        return -1;
    }
    else
    {
        return stack[top--];
    }
}

int evaluatePostfix(char*postfix)
{
    int i,op1,op2,result;
    char ch;

    for(i=0;postfix[i]!='\0';i++)
    {
        ch=postfix[i];

        if(isdigit(ch))
        {
            push(ch-'0');
        }
        else
        {
            op2=pop();
            op1=pop();

            switch(ch)
            {
                case '+':
                    result=op1+op2;
                    break;

                case '-':
                    result=op1-op2;
                    break;

                case '*':
                    result=op1*op2;
                    break;

                case '/':
                    result=op1/op2;
                    break;

                default:
                    printf("Invalid operator encountered");
                    exit(1);
            }

            push(result);
        }
    }

    return pop();
}

int main()
{
    char postfix[MAX];
    int result;

    printf("Enter postfix expression:");
    scanf("%s",postfix);

    result=evaluatePostfix(postfix);

    printf("The result of the postfix expression is:%d",result);

    return 0;
}
