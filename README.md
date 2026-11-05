# binary-search-tree
#include <iostream>
using namespace std;

// Template Node Structure
template <class T>
struct Node
{
    T data;
    Node* left;
    Node* right;

    Node(T value)
    {
        data = value;
        left = right = NULL;
    }
};

// Binary Search Tree Class
template <class T>
class BST
{
private:
    Node<T>* root;

    // Insert Node
    Node<T>* insert(Node<T>* node, T value)
    {
        if (node == NULL)
            return new Node<T>(value);

        if (value < node->data)
            node->left = insert(node->left, value);

        else if (value > node->data)
            node->right = insert(node->right, value);

        return node;
    }

    // Search Node
    bool search(Node<T>* node, T value)
    {
        if (node == NULL)
            return false;

        if (node->data == value)
            return true;

        if (value < node->data)
            return search(node->left, value);

        return search(node->right, value);
    }

    // Find Minimum Node
    Node<T>* findMin(Node<T>* node)
    {
        while (node && node->left != NULL)
            node = node->left;

        return node;
    }

    // Delete Node
    Node<T>* deleteNode(Node<T>* node, T value)
    {
        if (node == NULL)
            return NULL;

        if (value < node->data)
            node->left = deleteNode(node->left, value);

        else if (value > node->data)
            node->right = deleteNode(node->right, value);

        else
        {
            if (node->left == NULL)
            {
                Node<T>* temp = node->right;
                delete node;
                return temp;
            }

            else if (node->right == NULL)
            {
                Node<T>* temp = node->left;
                delete node;
                return temp;
            }

            Node<T>* temp = findMin(node->right);

            node->data = temp->data;

            node->right =
                deleteNode(node->right, temp->data);
        }

        return node;
    }

    // Inorder Traversal
    void inorder(Node<T>* node)
    {
        if (node != NULL)
        {
            inorder(node->left);
            cout << node->data << " ";
            inorder(node->right);
        }
    }

    // Preorder Traversal
    void preorder(Node<T>* node)
    {
        if (node != NULL)
        {
            cout << node->data << " ";
            preorder(node->left);
            preorder(node->right);
        }
    }

    // Postorder Traversal
    void postorder(Node<T>* node)
    {
        if (node != NULL)
        {
            postorder(node->left);
            postorder(node->right);
            cout << node->data << " ";
        }
    }

    // Free Memory
    void destroy(Node<T>* node)
    {
        if (node != NULL)
        {
            destroy(node->left);
            destroy(node->right);
            delete node;
        }
    }

public:

    BST()
    {
        root = NULL;
    }

    void insert(T value)
    {
        root = insert(root, value);
    }

    void search(T value)
    {
        if (search(root, value))
            cout << "Element Found!\n";
        else
            cout << "Element Not Found!\n";
    }

    void deleteValue(T value)
    {
        root = deleteNode(root, value);
        cout << "Delete Operation Completed!\n";
    }

    void displayInorder()
    {
        cout << "Inorder: ";
        inorder(root);
        cout << endl;
    }

    void displayPreorder()
    {
        cout << "Preorder: ";
        preorder(root);
        cout << endl;
    }

    void displayPostorder()
    {
        cout << "Postorder: ";
        postorder(root);
        cout << endl;
    }

    ~BST()
    {
        destroy(root);
    }
};

// Main Function
int main()
{
    BST<int> tree;
    int choice, value;

    do
    {
        cout << "\n===== BINARY SEARCH TREE =====";
        cout << "\n1. Insert";
        cout << "\n2. Search";
        cout << "\n3. Delete";
        cout << "\n4. Inorder Traversal";
        cout << "\n5. Preorder Traversal";
        cout << "\n6. Postorder Traversal";
        cout << "\n7. Exit";

        cout << "\nEnter Choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            cout << "Enter Value: ";
            cin >> value;
            tree.insert(value);
            cout << "Inserted Successfully!\n";
            break;

        case 2:
            cout << "Enter Value to Search: ";
            cin >> value;
            tree.search(value);
            break;

        case 3:
            cout << "Enter Value to Delete: ";
            cin >> value;
            tree.deleteValue(value);
            break;

        case 4:
            tree.displayInorder();
            break;

        case 5:
            tree.displayPreorder();
            break;

        case 6:
            tree.displayPostorder();
            break;

        case 7:
            cout << "Program Exited!\n";
            break;

        default:
            cout << "Invalid Choice!\n";
        }

    } while (choice != 7);

    return 0;
}