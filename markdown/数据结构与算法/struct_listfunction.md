## 链表的各种操作
```c
#ifndef __FUNCTION_H__
#define __FUNCTION_H__
#include<stdio.h>
#include<stdlib.h>
#include<assert.h>
#include<malloc.h>
#define DataType int

struct Node
{
	DataType data;
	struct Node* _pNext;
};

typedef struct Node Node;
typedef Node *pNode;

pNode InitNodeList(pNode *pHead); /* 初始化 */

pNode BuyNewNode(DataType data);  /* 新建节点 */
/* 头插法 */
void InsertNodeByFrontToTail(pNode *pHead, DataType data);
/* 正序打印 */
void PrintNodeProntToTail(pNode pHead);
/* 尾插法 */
void InserttNodeByTailToFront(pNode *pHead, DataType data);
/* 反转链表—一般方法 */
pNode ReceiveNodeList(pNode pHead);
/* 反转链表—递归实现 */
pNode ReceiveNodeList_DG(pNode pHead);
/* 合并两个有序单链表——一般方法 */
pNode MeryTwoSortNodeChangeOneSortNode(pNode pHead1, pNode pHead2);
/* 合并两个有序单链表——递归实现 */
pNode MeryTwoSortNodeChangeOneSortNode_DG(pNode pHead1, pNode pHead2);

void BubbleSortNodeList(pNode pHead); /* 链表排序——冒泡法实现 */

pNode SearchMIdNode(pNode pHead); /* 查找中间节点——快慢指针法 */

pNode FindLastKNode(pNode pHead, int key); /* 找到倒数第K个节点 */
/* 非头结点前插入data */
void InsertNotIntoKHead(pNode pos, DataType data);
/* 寻找链表中data为K的节点 */
pNode FindDataInNodeList(pNode pHead, DataType k);

void DeleteLastKNode(pNode pHead, int key);/* 删除倒数第K个节点 */

void DeleteNotTailNode(pNode pos); /* 删除单链表的非尾结点 */

void GetCircleForJoseph(pNode pHead); /* 构造约瑟夫环 */
/* 单链表实现约瑟夫环  */
void GetJosephCircle(pNode pHead, size_t K);
 
void GetCircleForList(pNode pHead); /* 构造带环链表 */

pNode isHaveCircle(pNode pHead); /* 判断链表是否带环 */

int GetCircleLength(pNode pHead); /* 求环的长度 */

pNode GetCircleIntoNode(pNode pHead); /* 求环的入口点 */

pNode pFrontNode(pNode pHead); /* 返回链表第一个节点 */

pNode pTailNode(pNode pHead); /* 返回链表最后一个节点 */
/* 非头结点前插入data */
void InsertNotHead(pNode pos, DataType data);
#endif
```

```c
#include"function.h"

pNode BuyNewNode(DataType data)
{
	pNode pTempNode = NULL;
	pTempNode = (pNode)malloc(sizeof(pNode));
	if (pTempNode)
	{
		pTempNode->data = data;
		pTempNode->_pNext = NULL;
	}
	return pTempNode;
}

void InsertNodeByFrontToTail(pNode *pHead, DataType data)
{
	pNode pTemp = NULL;
	if (*pHead == NULL)
		*pHead = BuyNewNode(data);

	else
	{
		pTemp = BuyNewNode(data);
		pTemp->_pNext = *pHead;
		*pHead = pTemp;
	}
}

void InserttNodeByTailToFront(pNode *pHead, DataType data)
{
	pNode pTest = NULL;
	pNode pCur = *pHead;

	pTest = BuyNewNode(data);
	if (*pHead == NULL)
		(*pHead) = pTest;

	else
	{
		while (NULL != pCur->_pNext)
		{
			pCur = pCur->_pNext;
		}
		pCur ->_pNext = pTest;
	}

}

void PrintNodeProntToTail(pNode pHead)
{
	//pNode pTemp = pHead;
	if (NULL == pHead)
		return;

	while (pHead != NULL)
	{
		printf("%d -> ", pHead->data);
		pHead = pHead->_pNext;
	}
	printf("NULL");
	printf("\n");
}

pNode ReceiveNodeList(pNode pHead)
{
	pNode pTest = pHead;
	//pNode pNextNode = NULL;
	pNode pPre = NULL;
	//pNode pReverse = NULL;

	if (pHead == NULL || pHead->_pNext == NULL)
		return pHead;

	while (pTest != NULL)
	{
		pNode pNextNode = pTest->_pNext;
		pTest->_pNext = pPre;
		pPre = pTest;
		pTest = pNextNode;
	}
	return pPre;
}

pNode ReceiveNodeList_DG(pNode pHead)
{
	pNode pNewNode = NULL;
	if (pHead == NULL || pHead->_pNext == NULL)
		return pHead;

	pNewNode = ReceiveNodeList_DG(pHead->_pNext);

	pHead->_pNext->_pNext = pHead;   /* 翻转链表的指向 */
	pHead->_pNext = NULL;         /* 记得赋值NULL，防止链表错乱 */
	return pNewNode;     /* 新链表头永远指向的是原链表的链尾 */

}

pNode MeryTwoSortNodeChangeOneSortNode(pNode pHead1, pNode pHead2)
{
	pNode pNewNode = NULL;
	pNode pTemp = NULL;
	pNode pNextNode = NULL;
	unsigned int Node1Len = 0;
	unsigned int Node2Len = 0;

	while (pHead1->_pNext != NULL)
	{
		++Node1Len;
	}
	while (pHead1->_pNext != NULL)
	{
		++Node2Len;
	}

	if (Node1Len < Node2Len)     /* 长的链表为pHead1 */
	{
		pTemp = pHead1;
		pHead1 = pHead2;
		pHead2 = pTemp;
	}

	while (pHead1 != NULL)
	{
		if (pHead1->data < pHead2->data)
		{
			pNextNode = pHead1;
			pHead1 = pHead1->_pNext;
			pNextNode = pNextNode->_pNext;
		}
		pNextNode = pHead2;
		pHead2 = pHead2->_pNext;
		pNextNode = pNextNode->_pNext;
	}
	if (pHead1 == NULL)
		return pNewNode;
	else
	{
		pNextNode = pHead1;
	}
	return pNewNode;
}

pNode MeryTwoSortNodeChangeOneSortNode_DG(pNode pHead1, pNode pHead2)
{
	pNode pNewHead = NULL;

	if (pHead1 == NULL)
		return pHead2;
	else if (pHead2 == NULL)
		return pHead1;
	else
	{
		if (pHead1->data > pHead2->data)
		{
			pNewHead = pHead2;
			pNewHead->_pNext = MeryTwoSortNodeChangeOneSortNode_DG( pHead1, pHead2->_pNext);
		}
		else
		{
			pNewHead = pHead1;
			pNewHead->_pNext = MeryTwoSortNodeChangeOneSortNode_DG(pHead1->_pNext, pHead2);
		}
	}
	return pNewHead;
}


void BubbleSortNodeList(pNode pHead)
{
	int pTempNode = 0;
	pNode pTail = NULL;
	pNode pPreCur = NULL;
	pNode pCur = NULL;
	int isFlag = 1;

	if (pHead == NULL || pHead->_pNext == NULL)
	{
		return;
	}

	while (pHead != pTail)
	{
		isFlag = 0;

		pPreCur = pHead;
		pCur = pHead->_pNext;

		while (pCur != pTail)
		{
			if (pCur->data < pPreCur->data)
			{
				pTempNode = pCur->data;
				pCur->data = pPreCur->data;
				pPreCur->data = pTempNode;

				isFlag = 1;
			}
			pPreCur = pCur;
			pCur = pCur->_pNext;
		}
		if (!isFlag)
			return;

		pTail = pPreCur;
	}
}


pNode SearchMIdNode(pNode pHead)
{
	pNode pFast = pHead;
	pNode pSlow = pHead;

	if (pHead == NULL || pHead->_pNext == NULL)
		return NULL;


	while (NULL != pFast && NULL != pFast->_pNext)
	{
		pSlow = pSlow->_pNext;
		pFast = pFast->_pNext->_pNext;
	}
	return pSlow;
}

pNode FindLastKNode(pNode pHead, int key)
{
	pNode pCur = pHead;
	pNode pSlow = pHead;
	pNode pFast = pHead;

	if (pHead == NULL )
	{
		return NULL;
	}
	while (key--)
	{
		if (pFast == NULL)
			return NULL;
		pFast = pFast->_pNext;
	}
	while (pFast != NULL)
	{
		pFast = pFast->_pNext;
		pSlow = pSlow->_pNext;
	}
	return pSlow;
}

void DeleteLastKNode(pNode pHead, int key)
{
	pNode pPosDelNode = FindLastKNode(pHead, key+1);

	if (pPosDelNode == pHead)
	{
		free(pPosDelNode);
		return;
	}
	pPosDelNode->_pNext = pPosDelNode->_pNext->_pNext;
}

pNode FindDataInNodeList(pNode pHead, DataType k)
{
	pNode pCur = pHead;
	while (pCur != NULL)
	{
		if (pCur->data == k)
		{
			return pCur;
		}
		pCur = pCur->_pNext;
	}
	return NULL;
}

pNode pFrontNode(pNode pHead)       /* 返回链表第一个节点 */
{
	pNode pPcur = pHead;
	if (pHead == NULL)
		return NULL;
	return pPcur;
}

pNode pTailNode(pNode pHead)    /* 返回链表最后一个节点 */
{
	pNode pPcur = pHead;
	if (pHead == NULL)
		return NULL;
	while (pPcur != NULL && pPcur->_pNext != NULL)
	{
		pPcur = pPcur->_pNext;
	}
	return pPcur;
}

void GetCircleForList(pNode pHead)
{
	pNode pTemp = NULL;
	pTemp = FindDataInNodeList(pHead, 9);
	pTemp->_pNext = FindDataInNodeList(pHead, 7);
}

pNode isHaveCircle(pNode pHead)
{
	pNode pSlowNode = pHead;
	pNode pFastNode = pHead;

	if (pHead == NULL)
		return NULL;

	while (pFastNode != pSlowNode && pFastNode->_pNext != NULL)
	{
		pFastNode = pFastNode->_pNext->_pNext;
		pSlowNode = pSlowNode->_pNext;
	}
	return pSlowNode;
}

int GetCircleLength(pNode pHead)
{
	pNode MeetNode = NULL;
	pNode pCur = NULL;
	size_t count = 0;

	if (isHaveCircle(pHead) == NULL)
		return 0;

	MeetNode = isHaveCircle(pHead);
	pCur = MeetNode->_pNext;

	while (pCur != MeetNode)
	{
		count++;
		pCur = pCur->_pNext;

	}
	return count;
}

pNode GetCircleIntoNode(pNode pHead)
{
	pNode pCur = pHead;
	/* 在判断带环问题时，返回了环中快慢指针的相遇点。*/
	pNode pMeetNodeInCircle = NULL;
	pMeetNodeInCircle = isHaveCircle(pHead);

	while (pCur != pMeetNodeInCircle)
	{
		pCur = pCur->_pNext;
		pMeetNodeInCircle = pMeetNodeInCircle->_pNext;
	}
	return pMeetNodeInCircle;
}

void GetCircleForJoseph(pNode pHead)
{
	pNode front = NULL;
	pNode tail = NULL;
	front = pFrontNode(pHead);
	tail = pTailNode(pHead);

	tail->_pNext = front;
}

void GetJosephCircle(pNode pHead, size_t K)
{
	int count = K;
	pNode pPreNode = pHead;
	pNode pCurNode = NULL;

	if (pHead == NULL)
		return;
	while (pPreNode->_pNext != pPreNode)
	{
		count = K;
		while (--count)
		{
			pCurNode = pPreNode;
			pPreNode = pPreNode->_pNext;
		}

		pCurNode->_pNext = pPreNode->_pNext;
		printf("%d 号出去-> ", pPreNode->data);
		//free(pPreNode);
		pPreNode = pPreNode->_pNext;
	}
	//return pPreNode;
	printf("\n最后留下的人是： %d \n\n", pPreNode->data);
}

int GetNodeListLength(pNode pHead)
{
	int count = 0;
	pNode pCur = pHead;
	if (pHead == NULL)
		return 0;
	while (pCur)
	{
		count++;
	}
	return count;
}

void DeleteNotTailNode(pNode pos)
{
	assert(pos);
	pNode pTempNode = NULL;
	DataType temp = 0;
	if (pos == NULL)
		return;
	else
	{
		/* 交换节点值 */
		temp = pos->data;
		pos->data = pos->_pNext->data;
		pos->_pNext->data = temp;

		pTempNode = pos->_pNext;
		pos->_pNext = pTempNode->_pNext;
		//free(pTempNode);
		pTempNode = NULL;
	}
}

void InsertNotHead(pNode pos, DataType data)
{
	pNode pNewNode = NULL;
	pNode pCur = NULL;
	DataType pTemp = 0;

	if (pos == NULL)
		return;

	pNewNode = BuyNewNode(data);
	if (pNewNode == NULL)
		return;

	pCur = pos;
	pNewNode->_pNext = pCur->_pNext;
	pCur->_pNext = pNewNode;

	pTemp = pCur->data;
	pCur->data = pNewNode->data;
	pNewNode->data = pTemp;
}
```
## Unit Test
```c
#include"function.h"

void test()
{
	pNode pHead1 = NULL;
	pNode pHead2 = NULL;
	pNode pNewHead = NULL;
	pNode pHead3 = NULL;
	pNode pSortNode = NULL;
	pNode LastKNum = 0;
	pNode pReverseNode = NULL;
	pNode pR = NULL;
	pNode pHead4 = NULL;
	pNode pCirclieNode = NULL;
	size_t CircleLength = 0;
	pNode pIntoCircleNode = NULL;
	pNode pHead5 = NULL;
	pNode JueNode = NULL;
	pNode pHead6 = NULL;
	pNode pDelNode = NULL;
	pNode pInsertNode = NULL;
	pNode pH = NULL;

	InsertNodeByFrontToTail(&pHead1, 9);
	InsertNodeByFrontToTail(&pHead1, 8);
	InsertNodeByFrontToTail(&pHead1, 6);
	InsertNodeByFrontToTail(&pHead1, 5);
	InsertNodeByFrontToTail(&pHead1, 2);
	InsertNodeByFrontToTail(&pHead1, 1);

	PrintNodeProntToTail(pHead1);

	printf("\n链表的反转、还原:\n");
	pReverseNode = ReceiveNodeList(pHead1);    /* 反转 */
	PrintNodeProntToTail(pReverseNode);

	pR = ReceiveNodeList_DG(pReverseNode);   /* 反转 */
	PrintNodeProntToTail(pR);

	pHead1 = pR;
	printf("\npHead1: ");
	PrintNodeProntToTail(pHead1);          /* 还原pHead1 */

	printf("\n");

	InserttNodeByTailToFront(&pHead2, 2);
	InserttNodeByTailToFront(&pHead2, 3);
	InserttNodeByTailToFront(&pHead2, 4);
	InserttNodeByTailToFront(&pHead2, 5);
	InserttNodeByTailToFront(&pHead2, 6);
	InserttNodeByTailToFront(&pHead2, 7);

	printf("pHead2: ");
	PrintNodeProntToTail(pHead2);

	printf("\n两个有序链表的合并:\n");
	pNewHead = MeryTwoSortNodeChangeOneSortNode_DG(pHead1, pHead2);
	PrintNodeProntToTail(pNewHead);

	/* 新建无序链表3 */
	InsertNodeByFrontToTail(&pHead3, 9);
	InsertNodeByFrontToTail(&pHead3, 4);
	InsertNodeByFrontToTail(&pHead3, 5);
	InsertNodeByFrontToTail(&pHead3, 7);
	InsertNodeByFrontToTail(&pHead3, 4);
	InsertNodeByFrontToTail(&pHead3, 0);
	InsertNodeByFrontToTail(&pHead3, 3);

	printf("\npHead3: ");
	PrintNodeProntToTail(pHead3);

	printf("\n排序好的链表3：");
	BubbleSortNodeList(pHead3);
	PrintNodeProntToTail(pHead3);

	printf("\npHead1中间节点：%d\n", (SearchMIdNode(pHead1)->data));
	printf("\npNewHead中间节点：%d\n", (SearchMIdNode(pNewHead)->data));

	printf("\n查找倒数第K个节点: ");
	LastKNum = FindLastKNode(pNewHead, 2);
	if (LastKNum != NULL)
		printf("%d\n", LastKNum->data);
	else
		printf("没有找到\n");

	printf("\n删除倒数第K个节点\n");
	DeleteLastKNode(pNewHead, 3);

	/* 构造一个带环链表4 */
	InsertNodeByFrontToTail(&pHead4, 9);
	InsertNodeByFrontToTail(&pHead4, 8);
	InsertNodeByFrontToTail(&pHead4, 6);
	InsertNodeByFrontToTail(&pHead4, 5);
	InsertNodeByFrontToTail(&pHead4, 2);
	InsertNodeByFrontToTail(&pHead4, 1);
	InsertNodeByFrontToTail(&pHead4, 3);
	InsertNodeByFrontToTail(&pHead4, 0);
	InsertNodeByFrontToTail(&pHead4, 7);

	printf("\n打印链表4：\n");
	PrintNodeProntToTail(pHead4);

	printf("\n构造带环链表\n");
	GetCircleForList(pHead4);
	printf("构造OK\n");

	pCirclieNode = isHaveCircle(pHead4);
	if (pCirclieNode == NULL)
		printf("不带环\n");
	else
		printf("带环\n");

	printf("\n求环的长度：");
	//PrintNodeProntToTail(pHead4);

	CircleLength = GetCircleLength(pHead4);
	printf("%d", CircleLength);

	printf("\n求环的入口：");
	pIntoCircleNode = GetCircleIntoNode(pHead4);
	printf("%d\n", pIntoCircleNode->data);

	/* 构造一个链表，pHead5; 头插法构造单链表 */
	InsertNodeByFrontToTail(&pHead5, 9);
	InsertNodeByFrontToTail(&pHead5, 8);
	InsertNodeByFrontToTail(&pHead5, 7);
	InsertNodeByFrontToTail(&pHead5, 6);
	InsertNodeByFrontToTail(&pHead5, 5);
	InsertNodeByFrontToTail(&pHead5, 4);
	InsertNodeByFrontToTail(&pHead5, 3);
	InsertNodeByFrontToTail(&pHead5, 2);
	InsertNodeByFrontToTail(&pHead5, 1);

	printf("\n约瑟夫环问题:\n");
	/* 构造环 */
	GetCircleForJoseph(pHead5);
	printf("构造OK\n");

	//PrintNodeProntToTail(pHead5);

	/* 约瑟夫环 */
	GetJosephCircle(pHead5, 4);

	printf("构造链表6\n");

	InsertNodeByFrontToTail(&pHead6, 6);
	InsertNodeByFrontToTail(&pHead6, 5);
	InsertNodeByFrontToTail(&pHead6, 4);
	InsertNodeByFrontToTail(&pHead6, 3);
	InsertNodeByFrontToTail(&pHead6, 2);

	PrintNodeProntToTail(pHead6);
	printf("\n删除倒数第 3 个节点后，链表为：\n");
	DeleteLastKNode(pHead6, 3);

	PrintNodeProntToTail(pHead6);
	printf("\n");

	PrintNodeProntToTail(pHead6);
	printf("删除非尾节点 3 :\n");
	pDelNode = FindDataInNodeList(pHead6, 3);
	DeleteNotTailNode(pDelNode);
	PrintNodeProntToTail(pHead6);

	printf("\n非头节点 5 前的插入:\n");
	PrintNodeProntToTail(pHead6);

	pInsertNode = FindDataInNodeList(pHead6, 5);

	InsertNotHead(pInsertNode, 99);
	PrintNodeProntToTail(pHead6);
	printf("\n");
}

int main()
{
	test();
	return 0;
}
```