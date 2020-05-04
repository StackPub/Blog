## 链式存储链表结构体
```c
#ifndef _LIST_H_
#define _LIST_H_
struct Node		/* 结点 */
{
	int data;		/* 数据域 */ 
	struct Node *next;	/* 指向下一个结点的指针 */
};

struct Header	/* 头结点 */
{
	int length;		/* 记录链表大小 */
	struct Node *next;
};

typedef struct Node List;
typedef struct Header pHead;

pHead *createList();		/* 创建链表 */
int isEmpty(pHead *l);	/* 判断链表是否为空 */
int Insert(pHead *l,int pos,int val);		/* 插入元素 */
List *Delete(pHead *l, int ele);		/* 删除元素 */
List *find(pHead *l,int ele);		/* 查找某个元素是否存在 */
int Size(pHead *l);			/* 获取链表大小 */
void Destory(pHead *l);		/* 销毁链表 */
void print(pHead *l);		/* 打印链表 */

#endif
```
## 链式存储链表实现
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include "link_list.h"

pHead *createList()	/* pHead是struct Header的别名，是头结点类型 */
{
	pHead *ph=(pHead*)malloc(sizeof(pHead));	
	ph->length=0;								
	ph->next=NULL;
	return ph;					
}

int Size(pHead *ph) /* 获取链表大小 */
{
	if(ph==NULL)
	{
		printf("ph==NULL\n");
		return 0;
	}
	return ph->length;
}

int Insert(pHead *ph,int pos, int val)
{
	/* 先做健壮性判断 */
	if(ph==NULL || pos<0 || pos >ph->length)
	{
		printf("error\n");
		return 0;
	}
	List* pval=(List *)malloc(sizeof(List));
	pval->data=val;
	List *pCur=ph->next;
	if(pos==0)
	{
		ph->next=pval;
		pval->next=pCur;
	}
	else
	{
		int i;
		for(i=1;i<pos;i++)
		{
			pCur=pCur->next;
		}
		pval->next=pCur->next;
		pCur->next=pval;
	}
	ph->length++;
	return 1;
}

List *find(pHead *ph,int val) /* 查找某个元素 */
{
	if(ph==NULL)
	{
		printf("error\n");
		return NULL;
	}
	
	List *pTmp=ph->next; /* 遍历链表来查找元素 */
	do
	{
		if(pTmp->data==val)
		{
			return pTmp;
		}
		pTmp=pTmp->next;
	}while(pTmp->next!=NULL);
	printf("no find %d data\n",val);
	return NULL;
}

List *Delete(pHead *ph,int val) /* 删除元素 */
{
	if(ph==NULL)
	{
		printf("ph==NULL\n");
		return NULL;
	}
	List *pval=find(ph,val);
	if(pval==NULL)
	{
		printf("not find %d\n",val);
	}

	/* 遍历链表找到要删除的结点，并找出其前驱及后继结点 */
	List *pRe=ph->next;		
	List *pCur=NULL;
	if(pRe->data==val)	/* 第一个结点 */
	{
		ph->next=pRe->next;
		ph->length--;
		return pRe;
	}
	else	
	{
		int i;
		for(i=0;i<ph->length;i++)
		{
			pCur=pRe->next;
			if(pCur->data==val)
			{
				pRe->next=pCur->next;
				ph->length--;
				return pCur;
			}
			pRe=pRe->next;
		}
	}
}

void Destory(pHead *ph) /* 销毁链表 */
{
	List *pCur=ph->next;
	List *pTmp;
	if(ph==NULL)
	{
		printf("error\n");
	}
	while(pCur->next != NULL)
	{
		pTmp=pCur->next;
		free(pCur);
		pCur=pTmp;
	}
	ph->length=0;
	ph->next=NULL;
}

void print(pHead *ph) /* 遍历打印链表 */
{
	if(ph==NULL)
	{
		printf("ph==NULL\n");
	}
	List *pTmp=ph->next;
	while(pTmp!=NULL)
	{
		printf("%d	",pTmp->data);
		pTmp=pTmp->next;
	}
	printf("\n");
}

```
## Unit Test
```c
#define _CRT_SECURE_NO_WARNINGA
#include "link_list.h"
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

int main()
{
	int ret;
	pHead *ph=createList();
	if(ph==NULL)
	{
		printf("error\n");
	}
	int arr[10]={1,2,3,4,5,6,7,8,9,10};
	int i;
	for(i=0;i<10;i++)
	{
		Insert(ph, 0, arr[i]);
	}
	printf("link length %d\n",Size(ph));

	printf("print link_list ele:\n");
	print(ph);
	printf("please input delete link ele:\n");
	int num;
	scanf("%d",&num);
	Delete(ph,num);
	printf("delete over,print link\n");
	print(ph);

	ret=find(ph,3);
	if(ret)
	{
		printf("get!\n");
	}
	else
	{
		printf("no!\n");
	}
	system("pause");
	return 0;
}
```