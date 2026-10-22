#include <stdio.h>
#define MAX 1000

int st[MAX], minSt[MAX];
int top = -1, minTop = -1;

void push(int value) {
    st[++top] = value;

    if (minTop == -1 || value <= minSt[minTop]) {
        minSt[++minTop] = value;
    }
}

void pop() {
    if (top == -1) {
        printf("Stack is empty\n");
        return;
    }

    if (st[top] == minSt[minTop]) {
        minTop--;
    }

    top--;
}

int getTop() {
    return st[top];
}

int getMin() {
    return minSt[minTop];
}

int main() {
    push(-2);
    push(0);
    push(-3);

    printf("Minimum: %d\n", getMin());

    pop();

    printf("Top: %d\n", getTop());
    printf("Minimum: %d\n", getMin());

    return 0;
}

<img width="136" height="73" alt="Screenshot 2026-10-22 at 11 37 14 PM" src="https://github.com/user-attachments/assets/aef39e18-b32c-4a3d-b08b-b979df5ac7c0" />
