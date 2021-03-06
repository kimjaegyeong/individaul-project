Linked List : 컴퓨터에 자료를 저장하는 구조의 한 종류, 일렬로 연결된 데이터를 저장할 때 사용한다. 또한 길이가 정해져 있지 않은 데이터의 연결된 집합이다. 데이터를 저장할 수 있는 공간에 다음 데이터의 주소를 가지고 있는 구조이다.
배열과는 다르게 데이터를 중간에 삽입하거나, 삭제하기가 가능하다. 해당 노드가 갖고있던 주소를 다음 노드에게 주거나, 받으면서 삽입과 삭제가 이루어진다.
한 방향으로만 이동할 수 있는 List는 단방향 Linked List이고 양 방향으로 이동할 수 있는 List는 양방향 Linked List이다. 이것은 노드에 다음 주소를 가지고 있으면서, 추가로 노드의 앞 부분 주소도 가지고 있다.   
  
Binary Tree: 컴퓨터에 자료를 저장하는 구조의 한 종류, 나무처럼 여러가지 줄기를 가질수도, 그렇지 않을 수도 있다.  
Binart Tree에는 세가지 순회방법이 있다. 그것은 Inorder, Preorder, Postorder이다.  
Inorder은 왼쪽, 뿌리, 오른쪽 순으로 이동한다. 더 이상의 노드가 없을 정도로 맨 왼쪽의 뿌리로 들어가서 맨 왼쪽의 값을 출력하고, 올라가서 자기 자신을 출력하고, 다시 오른쪽으로 가서 더 이상의 노드가 없으면 출력한다.
Preorder: 뿌리, 왼쪽, 오른쪽 순으로 이동한다. 자기자신을 먼저 출력하고, 왼쪽으로 가서 자기자신을 출력하고, 왼쪽으로 가서 자기자신을 출력한다.
더이상의 노드가 없으면 위로 올라가서 오른쪽으로 들어간다. 또한 자신을 출력한다. 
Postorder: 왼쪽, 오른쪽, 뿌리 순으로 이동한다. 맨 왼쪽으로 이동하여 더 이상의 노드가 없으면 그 값을 출력한다. 올라와서 오른쪽을 들어가서, 더 이상의 노드가 없으면 그 값을 출력한다. 다시 위로 올라가서 값을 출력한다.   
  
Graph: tree의 방향을 마음대로 조정하거나, 다른 노드와 서로 데이터를 주고 받기도 한다.  
graph는 directed graph와 undirected graph가 있다. tree는 directed graph에 포함된다. 또한 자기 자신을 가르키는 셀프엣지도 있다.
하나 이상의 cycle이 있으면 cyclic graph라 하고, cycle이 없는 경우는 acyclic이라고 한다.  
graph를 표현하는 방법에는 두 가지가 있다. 그것은 adjacency matrix와 adjacency list이다.  
adjacency matrix는 2차원 배열에 표현하고,
adjacency list 배열에 노드들을 나열하고 관계를 linked lisk로 표현하는 방법이다. 
adjacency matrix는 어떠한 노드들이 연결되어 있는지, 연결되지 않았는지 1과 0의 값을 매트릭스에 삽입한다.   
adjacency list는 어떠한 노드에 연결되어 있는 노드들을 순서 상관없이 List로 나열한다. 

*****************Linked List의 예시*****************
```
#include <stdio.h>
#include <stdlib.h>    

struct NODE {             
    struct NODE *next; 
    int data;           
};

int main()
{
    struct NODE *head = malloc(sizeof(struct NODE));  
                                                    

    struct NODE *node1 = malloc(sizeof(struct NODE));   
    head->next = node1;                               
    node1->data = 10;                           

    struct NODE *node2 = malloc(sizeof(struct NODE));   
    node1->next = node2;                         
    node2->data = 20;                                

    node2->next = NULL;                               
    struct NODE *curr = head->next;   
    while (curr != NULL)            
    {
        printf("%d\n", curr->data);    
        curr = curr->next;           
    }

    free(node2);
    free(node1);  
    free(head);

    return 0;
}
  ```
*****************Binary Tree의 예시***************** 
```
#include <stdio.h>
#include <stdlib.h>

typedef struct NodeStruct
{
    int value;
    struct NodeStruct* leftChild;
    struct NodeStruct* rightChild;
}Node;

Node* root = NULL;

Node* BST_insert(Node* root, int value)
{
    if(root == NULL)
    {
        root = (Node*)malloc(sizeof(Node));
        root->leftChild = root->rightChild = NULL;
        root->value = value;
        return root;
    }
    else
    {
        if(root->value > value)
            root->leftChild = BST_insert(root->leftChild, value);
        else
            root->rightChild = BST_insert(root->rightChild, value);
    }
    return root;
}
Node* findMinNode(Node* root)
{
    Node* tmp = root;
    while(tmp->leftChild != NULL)
        tmp = tmp->leftChild;
    return tmp;
}
Node* BST_delete(Node* root, int value)
{
    Node* tNode = NULL;
    if(root == NULL)
        return NULL;

    if(root->value > value)
        root->leftChild = BST_delete(root->leftChild, value);
    else if(root->value < value)
        root->rightChild = BST_delete(root->rightChild, value);
    else
    {
        // 자식 노드가 둘 다 있을 경우
        if(root->rightChild != NULL && root->leftChild != NULL)
        {
            tNode = findMinNode(root->rightChild);
            root->value = tNode->value;
            root->rightChild = BST_delete(root->rightChild, tNode->value);
        }
        else
        {
            tNode = (root->leftChild == NULL) ? root->rightChild : root->leftChild;
            free(root);
            return tNode;
        }
    }

    return root;
}
Node* BST_search(Node* root, int value)
{
    if(root == NULL)
        return NULL;

    if(root->value == value)
        return root;
    else if(root->value > value)
        return BST_search(root->leftChild, value);
    else
        return BST_search(root->rightChild, value);
}
void BST_print(Node* root)
{
    if(root == NULL)
        return;

    printf("%d ", root->value);
    BST_print(root->leftChild);
    BST_print(root->rightChild);
}

int main()
{
    root = BST_insert(root, 5);
    root = BST_insert(root, 3);
    root = BST_insert(root, 7);
    root = BST_insert(root, 1);
    root = BST_insert(root, 9);
    root = BST_insert(root, 6);

    root = BST_delete(root, 7);

    BST_print(root);
}
  ```
*****************Graph 예시*****************
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct { 
	int vertexCount; 
	int **matrix; 
} graphType;

graphType *createGraph(int vertexCount)
{
	int i;

	graphType *graph = (graphType *)malloc(sizeof(graphType) * vertexCount);
	graph->matrix = (int **)malloc(sizeof(int *) * vertexCount);
	
	graph->vertexCount = vertexCount;
	
	for (i = 0; i < vertexCount; i++) {
		graph->matrix[i] = (int *)malloc(sizeof(int) * vertexCount);
		memset(graph->matrix[i], 0, sizeof(int) * vertexCount);
	}
	return graph;
}

void addEdge(graphType *graph, int start, int goal)
{
	graph->matrix[start - 1][goal - 1] = 1;	
}

void printGraph(graphType *graph)
{
	int i, j;

	for (i = 0; i < graph->vertexCount; i++) {
		printf("start %d ->", i);
		for (j = 0; j < graph->vertexCount; j++) {
			printf("%d ", graph->matrix[i][j]);
		}
		printf("\n");
	}
}

{
	graphType *graph;
	graph = createGraph(5);

	addEdge(graph, 1, 2);
	addEdge(graph, 1, 3);
	addEdge(graph, 1, 4);
	addEdge(graph, 1, 5);
	addEdge(graph, 2, 3);
	addEdge(graph, 2, 5);
	addEdge(graph, 5, 4);

	printGraph(graph);

	return 0;
}
```
