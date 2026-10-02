#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Page {
    char url[100];
    struct Page *prev;
    struct Page *next;
};

struct Page *head = NULL;
struct Page *current = NULL;

struct Page* createPage(char url[]) {
    struct Page *p = (struct Page*)malloc(sizeof(struct Page));
    strcpy(p->url, url);
    p->prev = NULL;
    p->next = NULL;
    return p;
}

void visitPage(char url[]) {
    struct Page *p = createPage(url);
    if (head == NULL) {
        head = current = p;
    } else {
        // new page goes after the current one, forward history is discarded
        struct Page *temp = current->next;
        while (temp != NULL) {
            struct Page *del = temp;
            temp = temp->next;
            free(del);
        }
        current->next = p;
        p->prev = current;
        current = p;
    }
    printf("Visited: %s\n", url);
}

void moveBackward() {
    if (current == NULL) {
        printf("No pages in history\n");
    } else if (current->prev == NULL) {
        printf("Beginning reached, cannot go back. Current: %s\n", current->url);
    } else {
        current = current->prev;
        printf("Moved back to: %s\n", current->url);
    }
}

void moveForward() {
    if (current == NULL) {
        printf("No pages in history\n");
    } else if (current->next == NULL) {
        printf("End reached, cannot go forward. Current: %s\n", current->url);
    } else {
        current = current->next;
        printf("Moved forward to: %s\n", current->url);
    }
}

void deletePage(char url[]) {
    struct Page *temp = head;
    while (temp != NULL && strcmp(temp->url, url) != 0) {
        temp = temp->next;
    }
    if (temp == NULL) {
        printf("Page %s not found\n", url);
        return;
    }
    if (temp->prev != NULL) temp->prev->next = temp->next;
    else head = temp->next;

    if (temp->next != NULL) temp->next->prev = temp->prev;

    if (temp == current) {
        current = (temp->next != NULL) ? temp->next : temp->prev;
    }
    free(temp);
    printf("Deleted: %s\n", url);
}

void displayForward() {
    struct Page *temp = head;
    if (temp == NULL) {
        printf("No pages in history\n");
        return;
    }
    printf("First to last: ");
    while (temp != NULL) {
        printf("%s", temp->url);
        if (temp->next != NULL) printf(" <-> ");
        temp = temp->next;
    }
    printf("\n");
}

void displayBackward() {
    struct Page *temp = head;
    if (temp == NULL) {
        printf("No pages in history\n");
        return;
    }
    while (temp->next != NULL) temp = temp->next;
    printf("Last to first: ");
    while (temp != NULL) {
        printf("%s", temp->url);
        if (temp->prev != NULL) printf(" <-> ");
        temp = temp->prev;
    }
    printf("\n");
}

int main() {
    int choice;
    char url[100];

    while (1) {
        printf("--- Browser History Menu ---\n");
        printf("1. Visit new page\t2. Move backward\t3. Move forward\t");
        printf("4. Delete a page\t5. Display first to last\t");
        printf("6. Display last to first\t7. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter page URL: ");
                scanf("%s", url);
                visitPage(url);
                break;
            case 2:
                moveBackward();
                break;
            case 3:
                moveForward();
                break;
            case 4:
                printf("Enter page URL to delete: ");
                scanf("%s", url);
                deletePage(url);
                break;
            case 5:
                displayForward();
                break;
            case 6:
                displayBackward();
                break;
            case 7:
                return 0;
            default:
                printf("Invalid choice\n");
        }
    }
}
<img width="566" height="480" alt="image" src="https://github.com/user-attachments/assets/a935aa6a-2585-47f2-b815-20648164a03b" />
