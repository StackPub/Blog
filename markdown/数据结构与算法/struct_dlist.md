## 双向链表结构体
```c
#ifndef _DLIST_H_
#define _DLIST_H_

struct Node;
typedef struct Head *pHead;
typedef struct Node *pNode;

struct Head /* 定义头结点 */
{
	int length;
	pNode next;

};

struct Node /* 定义数据节点 */
{
	int data;
	pNode pre;
	pNode next;
};

pHead DlistCreate();
int getLength(pHead ph);
int IsEmpty(pHead ph);
int DlistInsert(pHead ph, int pos, int val);
pNode DlistDelete(pHead ph, int val);
pNode DlistFind(pHead ph, int val);
void DlistDestory(pHead ph);
void printFront(pHead ph);
void printLast(pHead ph);
#endif
```
## 双向链表实现
```c
#include<stdio.h>
#include<stdlib.h>
#include "dlist.h"

pHead DlistCreate()
{
	pHead ph = (pHead)malloc(sizeof(struct Head));
	if(ph == NULL)
	{
		printf("malloc ph error\n");
	}
	ph->length = 0;
	ph->next = NULL;
	return ph;
}

int getLength(pHead ph)
{
	if(ph == NULL)
	{
		printf("error\n");
	}
	return ph->length;
}

int IsEmpty(pHead ph)
{
	if(ph == NULL)
	{
		printf("error\n");
	}
	if(ph->length == 0)
	{
		return 1;
	}
	else
	{
		return 0;
	}
}

int DlistInsert(pHead ph, int pos, int val)
{
	pNode pval = NULL;
	if(ph == NULL||pos < 0||pos > ph->length)
	{
		printf("error\n");
	}
	/* 如果参数无误，为元素分配结点空间 */
	pval =(pNode)malloc(sizeof(struct Node));
	if(pval == NULL)
	{
			return 0;
	}
	pval->data=val;
	/* 判断在哪个位置插入，先判断链表是否为空 */
	if(IsEmpty(ph))
	{
		ph->next=pval;		/* 直接将结点插入头结点后 */
		pval->next=NULL;
		pval->pre=NULL;		/* 第一个结点不回指头结点 */
	}
	else
	{
		pNode pCur=ph->next;
		/* 与链表为空的区别在与链表不空且在0位置插入 */
		if(pos==0)		/* 在第一个位置(头结点后)插入 */
		{
			ph->next=pval;
			pval->pre=NULL;
			pval->next=pCur;
			pCur->pre=pval;
		}
		else
		{
			int i;
			for(i=1; i<pos; i++) /* i!=0因为pos=0做了特殊的处理 */
			{
				pCur=pCur->next;
			}
			/* 循环结束，此时pCur指向的是要插入的位置 */
			pval->next=pCur->next;
			pCur->next->pre=pval;
			pval->pre=pCur;
			pCur->next=pval;
		}
	}
	ph->length++;
	return 1;
}

pNode DlistDelete(pHead ph, int val)
{
	if(ph==NULL || ph->length==0)
	{
		printf("error\n");
	}
	pNode pval=DlistFind(ph,val);
	if(pval==NULL)
	{
		return NULL;
	}
	printf("Delete it\n");
	pNode pRe=pval->pre;
	pNode pNext=pval->next;

	pRe->next=pNext;
	pNext->pre=pRe;
	return pval;
}

pNode DlistFind(pHead ph, int val)
{
	if(ph==NULL)
	{
		printf("error\n");
	}
	pNode pTmp=ph->next;
	do
	{
		if(pTmp->data==val)
		{
			printf("get it\n");
			return pTmp;
		}
		pTmp=pTmp->next;

	}while(pTmp->next!=NULL);
	printf("not find\n");
	return NULL;
}

void DlistDestory(pHead ph)
{
	pNode pCur=ph->next;
	pNode pTmp;
	if(ph==NULL)
	{
		printf("error\n");
	}
	while(pCur->next!=NULL)
	{
		pTmp=pCur->next;
		free(pCur);
		pCur=pTmp;
	}
	ph->length=0;
	ph->next=NULL;
}

void printFront(pHead ph) /* 从前打印 */
{
	if(ph==NULL)
	{
		printf("error\n");
	}
	pNode pTmp=ph->next;
	while(pTmp!=NULL)
	{
		printf("%d ",pTmp->data);
		pTmp=pTmp->next;
	}
	printf("\n");
}

void printLast(pHead ph) /* 从后打印 */
{
	if(ph==NULL)
	{
		printf("error\n");
	}
	pNode pTmp=ph->next;
	while(pTmp->next!=NULL)
	{
		pTmp=pTmp->next;
	}
	int i;
	for(i=--ph->length;i>=0;i--)
	{
		printf("%d ",pTmp->data);
		pTmp=pTmp->pre;
	}
	printf("\n");
}
```
## Unit Test
```c
#define _CRT_SECURE_NO_WARNINGS
#include "dlist.h"
#include<stdio.h>
#include<stdlib.h>
int main()
{
	pHead ph = NULL;
	ph=DlistCreate();

	int num;
	printf("请输入要插入的元素，输入0结束:\n");
	while(1)
	{
		scanf("%d",&num);
		if(num==0)
		{
			break;
		}
		DlistInsert(ph,0,num);
	}
	printf("双链表的长度:%d\n",getLength(ph));
	printFront(ph);
	DlistInsert(ph,3,99);
	printFront(ph);
	printLast(ph);

	int val;
	printf("请输入要查找的元素:\n");
	scanf("%d",&val);
	DlistFind(ph,val);

	int del;
	printf("请输入要删除的元素:\n");
	scanf("%d",&del);
	DlistDelete(ph,del);
	printFront(ph);

	DlistDestory(ph);
	printf("destory scuuess!length:%d\n",ph->length);
	system("pause");
	return 0;
}
```