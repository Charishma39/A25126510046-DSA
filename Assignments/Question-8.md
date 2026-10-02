#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};
// Create a new node
struct Node* createNode(int value) {
    struct Node *newNode = (struct Node*)malloc(sizeof(struct Node));

    if (newNode == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}
// Insert a node
struct Node* insert(struct Node *root, int value) {
    if (root == NULL)
        return createNode(value);

    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);

    return root;
}

// Inorder traversal
void inorder(struct Node *root) {
    if (root == NULL)
        return;

    inorder(root->left);
    printf("%d ", root->data);
    inorder(root->right);
}

// Find minimum value node
struct Node* findMin(struct Node *root) {
    while (root != NULL && root->left != NULL) {
        root = root->left;
    }

    return root;
}

// Delete a node
struct Node* deleteNode(struct Node *root, int value) {
    if (root == NULL)
        return NULL;

    if (value < root->data) {
        root->left = deleteNode(root->left, value);
    }
    else if (value > root->data) {
        root->right = deleteNode(root->right, value);
    }
    else {
        // Case 1: No children
        if (root->left == NULL && root->right == NULL) {
            free(root);
            return NULL;
        }
        // Case 2: Only right child
        else if (root->left == NULL) {
            struct Node *temp = root->right;
            free(root);
            return temp;
        }
        // Case 2: Only left child
        else if (root->right == NULL) {
            struct Node *temp = root->left;
            free(root);
            return temp;
        }
        // Case 3: Two children
        else {
            struct Node *successor = findMin(root->right);

            root->data = successor->data;

            root->right = deleteNode(root->right, successor->data);
        }
    }
    return root;
}
// Main function
int main() {
    struct Node *root = NULL;
    int n, value;
    printf("Enter number of values to insert: ");
    scanf("%d", &n);
    printf("Enter %d values:\n", n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &value);
        root = insert(root, value);
    }
    printf("\nInorder before deletion: ");
    inorder(root);
    printf("\n");
    // Delete a leaf node
    printf("\n--- Deleting leaf node 20 ---\n");
    root = deleteNode(root, 20);
    printf("Inorder after deletion: ");
    inorder(root);
    printf("\n");
    // Delete a node with one child
    printf("\n--- Deleting node with one child (60) ---\n");
    root = deleteNode(root, 60);
    printf("Inorder after deletion: ");
    inorder(root);
    printf("\n");
    // Delete a node with two children
    printf("\n--- Deleting node with two children (30) ---\n");
    root = deleteNode(root, 30);
    printf("Inorder after deletion: ");
    inorder(root);
    printf("\n");
    return 0;
}

<img width="517" height="348" alt="image" src="https://github.com/user-attachments/assets/0a0e7ccc-0f8d-4bc0-a79b-475918461525" />
