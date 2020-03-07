---
title: "队列"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>
typedef int AdjType;
typedef char CharType;
typedef struct queue{
	CharType date;
	struct queue *next;
}QNode;
typedef struct Qlist{
	QNode *front;
	QNode *rear;
}List;

void InitQueue(List* p){
	p->rear=p->front=(QNode *)malloc(sizeof(QNode));
	if(p->front->next==NULL)
		printf("\n\t\t\t分配内存失败");
	else
		printf("\n\t\t\t内存分配成功");
	p->rear->next=NULL;
}
void Insert(List* p){
	QNode *q;
	CharType x;
	printf("\n\t\t\t请输入队列的值:");
	printf("\n\t\t\t输入以#结束\n");
	printf("\n\t\t\t");
	scanf("%c",&x);getchar();
	while(x!='#'){
		q=(QNode *)malloc(sizeof(QNode));
		q->date=x;
		p->rear->next=q;
		p->rear=q;
		printf("\n\t\t\t");
		scanf("%c",&x);getchar();
	}
	p->rear->next=NULL;
	free(q);
}
void Delete(List *q){
	QNode *p;
	p=q->front;
	p=p->next;
	printf("\n\t\t\t%c\n",p->date);
	q->front=q->front->next;
	free(p);
}
void Destroy(List *p){
	while(p->front->next!=NULL){
		p->front=p->front->next;
		printf("\n\t\t\t正在摧毁%c",p->front->date);
	}
}
int Len(List *p){
	QNode *q;
	AdjType j;
	j=0;
	q=p->front;
	while(q->next!=NULL)	{
		j++;
		q=q->next;
	}
	printf("\n\t\t\t长度是%d",j);
	free(q);
	return j;
}
void show(List* q){
	QNode *p;
	p=q->front;
	printf("\n\t\t\t");
	while(p->next!=NULL){
		p=p->next;
		printf("%c\t",p->date);
	}	
	free(q);
}
void main(){
	List q;
	char choice;
	int z;
	z=1;
	while(z){
		printf("\n\n\n\n");
		printf("\t\t\t--线 性 链 表--\n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------初始化队列         *");
		printf("\n\t\t\t*       2-------输入队列的值       *");
		printf("\n\t\t\t*       3-------显示队列           *");
		printf("\n\t\t\t*       4-------出      列         *");
		printf("\n\t\t\t*       5-------显示队列长度       *");
		printf("\n\t\t\t*       6-------摧毁队列           *");
		printf("\n\t\t\t*       0-------退      出         *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t请选择菜单号(0--7)：");
		scanf("%c",&choice);getchar();
		switch(choice){
		case '1':InitQueue(&q);break;
		case '2':Insert(&q);break;
		case '3':show(&q);break;
		case '4':Delete(&q);break;
		case '5':Len(&q);break;
		case '6':Destroy(&q);break;
		default:z=0;break;
		}
	}
}

```