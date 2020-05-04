## 顺序存储链表结构体
```c
#ifndef _SEQLIST_H_
#define _SEQLIST_H_

typedef struct _tag_SeqList	/* 头节点，记录表的信息 */
{
	int capacity;	/* 表容量 */
	int length;		/* 表长度 */
	int *node;		/* node[capacity],为指针素组 char **node; */
}TSeqList;

typedef void SeqList;
typedef void SeqListNode;

SeqList *SeqList_Create(int capacity);	/* 创建顺序表 */
void SeqList_Destory(SeqList *list);		/* 销毁顺序表 */
void SeqList_Clear(SeqList *list);			/* 清空顺序表 */
int SeqList_Length(SeqList *list);			/* 获取顺序表长度 */
int SeqList_Capacity(SeqList *list);		/* 获取顺序表容量 */
/* 在pos位置插入元素 */
int SeqList_Insert(SeqList *list,SeqListNode *node,int pos);	
/* 获取pos位置的元素 */
SeqList *SeqList_Get(SeqList *list,int pos);	
/* 删除pos位置的元素 */
SeqList *SeqList_Delete(SeqList *list, int pos);
#endif
```

## 顺序存储链表实现
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include "SeqList.h"
/* 创建顺序表,返回值为SeqList*类型，即顺序表的地址 */
SeqList *SeqList_Create(int capacity)	
{
	int ret;
	TSeqList *temp=NULL;
	temp=(TSeqList*)malloc(sizeof(TSeqList)); /* 为头节点分配地址 */
	if(temp==NULL)
	{
		ret=1;
		printf("func SeqList_Create() error:%d\n",ret);
		return NULL;
	}
	memset(temp, 0, sizeof(TSeqList));
	temp->capacity=capacity;
	temp->length=0;
	/* 分配一个指针数组	char**malloc(sizeof(char*)*capacity) */
	temp->node=(int*)malloc(sizeof(void*)*capacity);	
	if(temp->node==NULL)
	{
		ret=2;
		printf("func SeqList_Create() error:%d\n",ret);
		return NULL;
	}
	return temp;
}

int SeqList_Capacity(SeqList *list) /* 求顺序表容量 */
{
	TSeqList *temp=NULL;
	if(list==NULL)
	{
		return;
	}
	temp=(TSeqList *)list;
	return temp->capacity;
}

int SeqList_Length(SeqList *list) /* 获取顺序表长度 */
{
	TSeqList *temp=NULL;
	if(list==NULL)
	{
		return;
	}
	temp=(TSeqList *)list;
	return temp->length;
}
/* 插入元素 */
int SeqList_Insert(SeqList *list,SeqListNode *node,int pos)
{
	int i;
	TSeqList *temp=NULL;
	if(list==NULL || node==NULL)	/* 健壮性判断 */
	{
		return -1;
	}
	temp=(TSeqList *)list;
	if(temp->length >= temp->capacity)	/* 如果顺序表已满 */
	{
		return -2;
	}
	/* 如果给出的pos位置在线性表长度之后，即中间有空余 */
	if(pos >temp->length)		
	{
		pos=temp->length;		/* 就修正到最后一个元素后面 */
	}
	/* 将插入元素后的元素依次后移 */
	for(i=temp->length;i>pos;i--)	
	{
		temp->node[i]=temp->node[i-1];
	}
	/* 腾出的位置插入新元素 */
	temp->node[i]=(SeqListNode *)node; 
	temp->length++;				/* 插入成功后，长度加1 */
	return 0;
}

SeqList *SeqList_Delete(SeqList *list, int pos) /* 删除元素 */
{
	int i;
	TSeqList *tlist=NULL;
	SeqListNode *temp=NULL;
	tlist=(TSeqList*)list;
	if(list==NULL || pos<0 || pos>=tlist->capacity)
	{
		printf("SeqList_Detele() error\n");
		return NULL;
	}
	temp=(SeqListNode*)tlist->node[pos];	/* 要删除的元素 */
	for(i=pos+1;i<tlist->length;i++)
	{
		tlist->node[i-1]=tlist->node[i];
	}
	tlist->length--;
	return temp;
}

SeqList *SeqList_Get(SeqList *list, int pos) /* 查找元素 */
{
	TSeqList *tlist=NULL;
	SeqListNode *temp=NULL;
	tlist=(TSeqList *)list;
	if(list==NULL || pos<0 || pos>=tlist->capacity)
	{
		printf("SeqList_Get() error\n");
		return NULL;
	}
	/* 将表中pos位置的结点指针赋给temp */
	temp=(SeqListNode *)tlist->node[pos];	
	return temp;
}

void SeqList_Clear(SeqList *list)  /* 清空顺序表 */
{
	TSeqList *temp=NULL;
	if(list==NULL)
	{
		return;
	}
	temp=(SeqList*)list;
	temp->length=0;
	memset(temp->node, 0, (temp->capacity*sizeof(void*)));
	return;
}

void SeqList_Destory(SeqList *list) /* 销毁顺序表 */
{
	TSeqList *temp=NULL;
	if(list==NULL)
	{
		return;
	}
	temp=(TSeqList *)list;
	if(temp->node != NULL)
	{
		free(temp->node);	/* 先释放头节点中的指针数组 */
	}
	free(temp);		/* 在释放头节点 */
	return;
}
```

## Unit Test
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include "SeqList.h"

typedef struct _Teacher
{
	char name[32];
	int age;
}Teacher;

int main()
{
	int ret =0;
	int len =0;
	int i		=0;

	SeqList *list=NULL;
	Teacher t1={
		"loci",
		32
	};
	Teacher t2={
		"coco",
		23
	};
	Teacher t3={
		"lili",
		21
	};
	Teacher t4={
		"fgfg",
		45
	};
	Teacher t5={
		"ktkt",
		28
	};

	/* 创建顺序表 */
	list=SeqList_Create(10);

	/* 头插法 */
	ret=SeqList_Insert(list,(SeqListNode*)&t1,0);	
	ret=SeqList_Insert(list,(SeqListNode*)&t2,0);
	ret=SeqList_Insert(list,(SeqListNode*)&t3,0);
	ret=SeqList_Insert(list,(SeqListNode*)&t4,0);
	ret=SeqList_Insert(list,(SeqListNode*)&t5,0);

	printf("顺序表容量:%d\n",SeqList_Capacity(list));
	printf("顺序表长度:%d\n",SeqList_Length(list));
	len=SeqList_Length(list);

	printf("遍历顺序表：\n");
	for(i=0;i<len; i++)
	{
		Teacher *temp=(Teacher *)SeqList_Get(list,i);	
		if(temp==NULL)
		{
			printf("func SeqList_Get() error\n");
			return;
		}
		printf("teachr name:%s\tage:%d\n",temp->name,temp->age);
	}

	printf("销毁顺序表：\n");
	while(SeqList_Length(list)>0)
	{
		/* 删除头部元素 */
		Teacher *temp=(Teacher *)SeqList_Delete(list,0);	
		if(temp==NULL)
		{
			printf("func SeqList_Delete error\n");
			return;
		}
		printf("teachr name:%s\tage:%d\n",temp->name,temp->age);
	}
	SeqList_Destory(list);
	system("pause");
	return 0;
}
```