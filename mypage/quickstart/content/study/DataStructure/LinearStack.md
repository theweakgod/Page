---
title: "线性栈"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
typedef int SElemType;
typedef struct SqStack{
	SElemType *base;
	SElemType *top;
	int stacksize;
}SStack;
void InitStack(SStack *p){
	p->base=p->top=(SStack *)malloc(sizeof(SStack));
	if(p->base==NULL){
		return;
	}
	else{
		p->base=NULL;
		p->stacksize=0;
	}
}
void DestoryStack(SStack *p){

	free(p->base);
	free(p->top);
}
void CLearStack(SStack *p){
	while(p->top!=p->base){
		p->top=p->top-1;
		p->stacksize=p->stacksize-1;
	}
}
int StackEmpty(SStack *p){
	if(p->base==p->top){
		return 1;
	}
	else{
		return 0;
	}

}
int StackLength(SStack *p){
	int i;
	SElemType *q;
	q=p->top;
	i=0;
	while(q==p->base){
		q=q-1;
		i++;
	}
	return i;
}
int Get(SStack *p){
	int i;
	if(p->top==p->base)
		return 0;
	else{
		i=*p->top;
		return i;
	}
}
void Push(SStack *p){
	if(p->top-p->base>=p->stacksize){
		p->base=(int *)realloc(p->base,(p->stacksize+10)*sizeof(int));
		if
	}
}


```