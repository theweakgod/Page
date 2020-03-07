---
title: "森林转化成二叉树实现"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
#define MAX 100
typedef struct CNode{
	char data;
	struct CNode *firstchild,*nextsibling;
}CN;
typedef struct CSTNode{
	int child;
	struct CSTNode *next;
}CST;
typedef struct CSTD{
	char data;
	CST *firstchild;
}CD;
typedef struct CSTree{
	CD nodes[MAX];
	int n,r;
}CSTr;
void Init(CSTr *p){
	char x;
	int h,i,j;
	CST *s,*q;
	scanf("%c",&x);getchar();
	i=p->r=p->n=0;
	h=-1;
	while(j!=-1||p->r!=i+1){
		p->nodes[i].data=x;
		if(h!=p->r){
			h=p->r;
			p->nodes[p->r].firstchild=(CST *)malloc(sizeof(CST));
			p->nodes[p->r].firstchild->next=NULL;
			s=p->nodes[p->r].firstchild;
		}
		printf("\n\t\t\t请输入数字,决定是否成为%c的孩子",p->nodes[p->r].data);
		scanf("%d",&j);getchar();
		if(j!=-1){
			s->child=i+1;
			q=(CST *)malloc(sizeof(CST));
			s->next=q;
			q->next=NULL;
			s=q;
			i++;
			p->n++;
			scanf("%c",&x);getchar();
		}
		else{
			p->r++;
			s->next=NULL;
		}
	}
}
void zhuanhua(CSTr *p,int root,int *tag,CN *q){
	if(p->nodes[root].firstchild->next==NULL&&tag[root]==1){
		q->firstchild=NULL;
		q->nextsibling=NULL;
		return ;
	}
	else{
		CST *f;
		CN *s;
		CN *l;
		s=(CN *)malloc(sizeof(CN));
		s->firstchild=s->nextsibling=NULL;
		q->firstchild=s;
		q->nextsibling=NULL;
		l=s;
		f=p->nodes[root].firstchild;
		while(f->next!=NULL&&tag[root]!=1){
			s->data=p->nodes[f->child].data;
			zhuanhua(p,f->child,tag,s);
			tag[f->child]=1;
			f=f->next;
			if(f->next!=NULL){
				s=(CN *)malloc(sizeof(CN));
				s->firstchild=s->nextsibling=NULL;
				l->nextsibling=s;
				l=s;
			}
		}
	}
}
void xianxu(CN *p){
	if(p->firstchild==NULL&&p->nextsibling==NULL){
		return ;
	}
	else{
		printf("%c\t",p->data);
		if(p->firstchild!=NULL)
		xianxu(p->firstchild);
		if(p->nextsibling!=NULL)
		xianxu(p->nextsibling);
	}
}
void zhongxu(CN *p){
	if(p->firstchild==NULL&&p->nextsibling==NULL)
		return ;
	else{
		if(p->firstchild!=NULL)
		zhongxu(p->firstchild);	
		printf("%c\t",p->data);
		if(p->nextsibling!=NULL)
		zhongxu(p->nextsibling);
	}
}
void main(){
	CSTr p[3];
	CN q[3];
	int visited[MAX]={0};
	int j,i=0;
	for(j=0;j<3;j++)
	Init(p+j);
	if(p->nodes[0].firstchild->next->next==NULL)
		printf("SSS112");
	for(j=0;j<3;j++){
	(q+j)->data=(p+j)->nodes[0].data;
	}
	for(j=0;j<3;j++){
		zhuanhua(p+j,i,visited,q+j);
		for(i=0;i<MAX;i++){
			visited[i]=0;
		}
		i=0;
	}
	for(j=0;j<2;j++){
		(q+j)->nextsibling=(q+j+1);
		(q+j+1)->nextsibling=NULL;
	}
	xianxu(q);
	printf("asd");
	zhongxu(q);
	printf("RRR");
}

```