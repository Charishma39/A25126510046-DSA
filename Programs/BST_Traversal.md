#include <stdio.h>
#include <stdlib.h>

/* BST Node */
struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};

struct Node *root = NULL;

/* Create new node */
struct Node *createNode(int value) {
    struct Node *newNode =
        (struct Node *)malloc(sizeof(struct Node));

    if (newNode == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }

    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;

    return newNode;
}

/* Insert into BST */
struct Node *insert(struct Node *root, int value) {
    if (root == NULL)
        return createNode(value);

    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);
    else
        printf("Duplicate value not allowed!\n");

    return root;
}

/* Inorder: Left, Root, Right */
void inorder(struct Node *root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

/* Preorder: Root, Left, Right */
void preorder(struct Node *root) {
    if (root != NULL) {
        printf("%d ", root->data);
        preorder(root->left);
        preorder(root->right);
    }
}

/* Postorder: Left, Right, Root */
void postorder(struct Node *root) {
    if (root != NULL) {
        postorder(root->left);
        postorder(root->right);
        printf("%d ", root->data);
    }
}

/* Search a value */
struct Node *search(struct Node *root, int value) {
    if (root == NULL || root->data == value)
        return root;

    if (value < root->data)
        return search(root->left, value);

    return search(root->right, value);
}

/* Find largest node in left subtree */
struct Node *findLargestInLeftSubtree(struct Node *root) {
    if (root == NULL || root->left == NULL)
        return NULL;

    struct Node *temp = root->left;

    while (temp->right != NULL)
        temp = temp->right;

    return temp;
}

/* Delete a node */
struct Node *deleteNode(struct Node *root, int value) {
    if (root == NULL) {
        printf("Value not found!\n");
        return NULL;
    }

    if (value < root->data) {
        root->left = deleteNode(root->left, value);
    }
    else if (value > root->data) {
        root->right = deleteNode(root->right, value);
    }
    else {
        /* Case 1: No child */
        if (root->left == NULL &&
            root->right == NULL) {
            free(root);
            return NULL;
        }

        /* Case 2: Only right child */
        else if (root->left == NULL) {
            struct Node *temp = root->right;
            free(root);
            return temp;
        }

        /* Case 3: Only left child */
        else if (root->right == NULL) {
            struct Node *temp = root->left;
            free(root);
            return temp;
        }

        /* Case 4: Two children */
        else {
            struct Node *temp =
                findLargestInLeftSubtree(root);

            root->data = temp->data;

            root->left =
                deleteNode(root->left, temp->data);
        }
    }

    return root;
}

/* Main function */
int main() {
    int choice, value;

    while (1) {
        printf("\n\n--- BST MENU ---\n");
        printf("1. Insert\n");
        printf("2. Inorder Traversal\n");
        printf("3. Preorder Traversal\n");
        printf("4. Postorder Traversal\n");
        printf("5. Search\n");
        printf("6. Find Largest in Left Subtree\n");
        printf("7. Delete\n");
        printf("8. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter value: ");
                scanf("%d", &value);
                root = insert(root, value);
                break;

            case 2:
                printf("Inorder: ");
                inorder(root);
                printf("\n");
                break;

            case 3:
                printf("Preorder: ");
                preorder(root);
                printf("\n");
                break;

            case 4:
                printf("Postorder: ");
                postorder(root);
                printf("\n");
                break;

            case 5:
                printf("Enter value to search: ");
                scanf("%d", &value);

                if (search(root, value) != NULL)
                    printf("Value found!\n");
                else
                    printf("Value not found!\n");
                break;

            case 6: {
                struct Node *temp =
                    findLargestInLeftSubtree(root);

                if (temp != NULL)
                    printf("Largest in left subtree: %d\n",
                           temp->data);
                else
                    printf("Left subtree is empty!\n");
                break;
            }

            case 7:
                printf("Enter value to delete: ");
                scanf("%d", &value);
                root = deleteNode(root, value);
                break;

            case 8:
                return 0;

            default:
                printf("Invalid choice!\n");
        }
    }
} 

<img width="280" height="684" alt="Screenshot 2026-10-22 at 8 49 06 PM" src="https://github.com/user-attachments/assets/eb5575d8-6109-41b0-9e04-9540fb981b29" />
<img width="290" height="693" alt="Screenshot 2026-10-22 at 8 49 23 PM" src="https://github.com/user-attachments/assets/e05ef757-da41-43f8-a57c-8c7af6432673" />
<img width="328" height="684" alt="Screenshot 2026-10-22 at 8 49 32 PM" src="https://github.com/user-attachments/assets/de06f644-3346-48a2-8d54-f506d55cf757" />


