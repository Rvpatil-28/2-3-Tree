# 2-3-Tree
C code for implementation of 2-3 trees.
// Online C compiler to run C program online
#include <stdio.h>
#include <stdlib.h>

// Node structure for a 2-3 tree
 struct Node {
    int lkey, rkey;
    struct Node* left;
    struct Node* center;
    struct Node* right;
};
void inorderTraversal(struct Node* root) {
    if (root) {
        inorderTraversal(root->left);
        printf("each-1-node is %d ", root->lkey);
        if (root->rkey != -1)
            printf("%d \n", root->rkey);
        inorderTraversal(root->center);
        inorderTraversal(root->right);
    }
}

// Function to create a new leaf node for the 2-3 tree
struct Node* createNode(int k) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->lkey=k;
    newNode->rkey = 0;
    newNode->left = NULL;
    newNode->center = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to add a new key/value pair to the node
struct Node* add(struct Node* node,int key)
{
    if(node->lkey<key)
    {
        node->lkey=node->lkey;
        node->rkey=key;
    }
    else
    {
        int x;
        x=node->lkey;
        node->lkey=key;
        node->rkey=x;
    }
}
struct Node * spilttleft(struct Node *root,int key)
{
    struct Node*main=createNode(root->lkey);
    main->left=createNode(key);
    main->right=createNode(root->rkey);
    free(root);  
    return main;
}
struct Node *spiltcenter(struct Node *root,int key)
{
    struct Node*main=createNode(key);
    main->left=createNode(root->lkey);
    main->right=createNode(root->rkey);
    free(root);
    return main;
}
struct Node *spiltright(struct Node *root,int key)
{
    struct Node*main=createNode(root->rkey);
    main->left=createNode(root->lkey);
    main->right=createNode(key);
    free(root);
    return main;
}
struct Node* take_left_node_up(struct Node* root){
    root->lkey=root->left->lkey;
    root->left=root->left->left;
    root->center=root->left->right;
}
struct Node* take_right_node_up(struct Node* node){
    node->rkey=node->right->lkey;
    node->center=node->right->left;
    node->right=node->right->center;
}

// Recursive function to insert a key/value pair into the 2-3 tree
struct Node* insertHelp(struct Node* node, int k) 
{
    struct Node* retval;
    if (node == NULL) 
    { // Empty tree: create a leaf node for root
        return createNode(k);
    }
    if(((node->lkey!=0)&& (node->rkey==0)&&((node->left!=0) && (node->right!=0))))
    {
        if(k<node->lkey)
        {
            node->left=insertHelp(node->left,k);
             // node=take_left_node_up(node->left);
        }
        else
        {
            node->right=insertHelp(node->right,k);
             // node=take_right_node_up(node->right);
        }
     return node;   
    }
    if (node->lkey != 0 && node->rkey == 0)
    { // At leaf node: insert here
        return add(node,k);
    }
    if((node->left!=0) && (node->right!=0))
    {
         if(k<node->lkey)
            node->left=insertHelp(node->left,k);
        else
            node->right=insertHelp(node->right,k);
    }
    if((node->lkey!=0) && (node->rkey!=0))
    // Add to internal node
    {
        if (k < node->lkey) 
        { // Insert left
             retval = spilttleft(node,k);
            return retval;
        } 
        else if ( (k>node->lkey) && (k< node->rkey))
        {
            retval =spiltcenter(node,k);
            return retval;
        }
        else if(k>node->rkey)
        { // Insert right
            retval =spiltright(node,k);
            return retval;
         }
    }
     else
     {
         if(k<node->lkey)
            node->left=insertHelp(node->left,k);
         else if( (k>node->lkey) && (k<node->rkey))
            node->center=insertHelp(node->center,k);
        else
            node->right=insertHelp(node->right,k);
     }
    
}
int main()
{
    struct Node*root;
    int a,key;
    while(a!=3)
    {
        printf("\nenter 1-insert \n 2-display \n 3-exit\n ");
        scanf("%d",&a);
        switch(a)
        {
            case 1:printf("enter key to be inserted :\n");
                    scanf("%d",&key);
                    root=insertHelp(root,key);
                    break;
            case 2:printf("sequence is \n");
                    inorderTraversal(root);
                    break;
            case 3:exit(0);
     }
}

}
