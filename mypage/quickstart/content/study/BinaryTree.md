---
title: "二叉树"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
typedef char CharType;
typedef struct GNode{
	CharType date;
	struct GNode *lchild,*rchild;
}G;
G *chushi(){
	G *p;
	CharType ch;
	scanf("%c",&ch);getchar();
	if(ch==' ')
		p=NULL;
	else
	{
		p=(G *)malloc(sizeof(G));
		p->date=ch;
		printf("\n\t\t\t请输入%c节点的左孩子",p->date);
		p->lchild=chushi();
		printf("\n\t\t\t请输入%c节点的右孩子",p->date);
		p->rchild=chushi();
	}
	return (p);
}
void xianxu(G *p){
	if(p!=NULL){
		xianxu(p->lchild);
		printf("%c",p->date);
		xianxu(p->rchild);
	}
}
void zhongxu(G *p){
	if(p!=NULL){
		zhongxu(p->lchild);
		printf("%c",p->date);
		zhongxu(p->rchild);
	}
}
void houxu(G *p){
	if(p!=NULL){
		zhongxu(p->lchild);
		zhongxu(p->rchild);
		printf("%c",p->date);
	}
}
void main(){
	G *p;
	p=chushi();
	xianxu(p);
	zhongxu(p);
	houxu(p);
}
```