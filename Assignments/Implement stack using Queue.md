#include <stdio.h>

#define MAX 1000

int q1[MAX], q2[MAX];
int f1 = 0, r1 = -1;
int f2 = 0, r2 = -1;

void push(int x) {
    q2[++r2] = x;

    while (f1 <= r1) {
        q2[++r2] = q1[f1++];
    }

    int i;
    for (i = 0; i <= r2; i++) {
        q1[i] = q2[i];
    }

    f1 = 0;
    r1 = r2;
    f2 = 0;
    r2 = -1;
}

int pop() {
    if (f1 > r1) {
        printf("Stack is empty\n");
        return -1;
    }

    return q1[f1++];
}

int top() {
    if (f1 > r1) {
        printf("Stack is empty\n");
        return -1;
    }

    return q1[f1];
}

int empty() {
    return f1 > r1;
}

int main() {
    push(10);
    push(20);
    push(30);

    printf("Top: %d\n", top());
    printf("Popped: %d\n", pop());
    printf("Top: %d\n", top());
    printf("Empty: %s\n", empty() ? "Yes" : "No");

    return 0;
} 

<img width="110" height="78" alt="Screenshot 2026-10-22 at 11 43 38 PM" src="https://github.com/user-attachments/assets/2723ba67-d625-4c9b-802a-7da29f005370" />
