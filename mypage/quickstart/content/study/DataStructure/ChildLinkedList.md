---
title: "孩子链表"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
#define MAX 100
typedef struct CTNode{
	int child;
	struct CTNode *next;
}CNode;
typedef struct C{
	char data;
	CNode *firstchild;
}CTBox;
typedef struct c{
	CTBox	nodes[MAX];
	int	n,r;
}CTree;
void Init(CTree *p){
	char x;
	int i,j,m,h;
	CNode *s,*q;
	printf("\n\t\t\t请按下列规则输入，分字符和数字，字符决定该结点的值，数字决定这个结点是否为该结点位置的孩子");
	printf("\n\t\t\t数字为-1时跳到下一个结点,知道跳完所有结点才能结束");
	printf("\n\t\t\t请输入字符");
	scanf("%c",&x);getchar();
	i=0;
	h=-1;
	p->r=i;
	p->n=i;
	while(j!=-1||p->r!=i+1){
		p->nodes[i].data=x;
		if(h!=p->r){
			h=p->r;
			p->nodes[p->r].firstchild=(CNode *)malloc(sizeof(CNode));
			s=p->nodes[p->r].firstchild; 
		}
		printf("\n\t\t\t请输入数字,决定是否成为%c的孩子",p->nodes[p->r].data);
		scanf("%d",&j);getchar();
		if(j!=-1){
			q=(CNode *)malloc(sizeof(CNode));
			s->child=i+1;
			s->next=q;
			q->next=NULL;
			s=q;
			p->n++;
			printf("\n\t\t\t请输入该结点的值");
			scanf("%c",&x);getchar();
			i++;
		}
		else{
			s->next=NULL;
			p->r++;
		}
	}
}
int flag=0;
void xianxu(CTree *p,int root,int *tag){
	if(p->nodes[root].firstchild->next==NULL&&tag[root]==1){
		return ;
	}
	else{
		CNode *q;
		if(flag!=1){
			printf("%c\t",p->nodes[0].data);
			flag=1;
		}
		q=p->nodes[root].firstchild;
		while(q->next!=NULL&&tag[root]!=1){
			printf("%c\t",p->nodes[q->child].data);
			xianxu(p,q->child,tag);
			tag[q->child]=1;
			q=q->next;
		}
	}
}
int count;
int max(int a,int b){
	if(a>b)
		return a;
	else
		return b;
}
void deep(CTree *p,int root,int *tag,int i){
	if(p->nodes[root].firstchild->next==NULL&&tag[root]==1){
		return ;
	}
	else{
		CNode *q;
		q=p->nodes[root].firstchild;
		while(q->next!=NULL&&tag[q->child]!=1){	
			if(tag[root]!=1){
				i++;
			}
			deep(p,q->child,tag,i);
			count=max(count,i);
			tag[q->child]=1;
			q=q->next;
			i=1;
		}
	}
}
void main(){
	CTree p;
	int visited[MAX]={0};
	int i,j;
	i=0;
	count=0;
	Init(&p);
//	printf("\n");
	xianxu(&p,i,visited);
//	for(j=0;j<MAX;j++)
//		visited[j]=0;
//	printf("\n");
//	i=0;
//	j=1;
//	deep(&p,i,visited,j);
//	printf("%d",count);
}

```