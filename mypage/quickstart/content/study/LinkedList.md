---
title: "链表"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
typedef int AdjType;
typedef char CharType;
typedef struct Node{
	CharType date;
	struct Node *next;
}PNode;
PNode *head;
void chushi(){
	PNode *p,*q;
	char x;
	head=(PNode *)malloc(sizeof(PNode));
	head->next=NULL;
	q=head;
	scanf("%c",&x);
	getchar();
	while(x != '#'){
		p=(PNode *)malloc(sizeof(PNode));
		p->date=x;
		p->next=q->next;
		q->next =p;
		q=p;
		scanf("%c",&x);getchar();
	}
}
void Insert(int i){
	PNode *p,*q;
	AdjType j;
	CharType x;
	q=(PNode *)malloc(sizeof(PNode));
	p=head;
	j=0;
	scanf("%c",&x);getchar();
	while (j!=i){	
		p=p->next;
		j++;
	}
	q->next=p->next;
	p->next=q;
	q->date=x;
}
void Delete(int i){
	PNode *p,*q;
	AdjType j;
	p=head;
	j=0;
	q=(PNode *)malloc(sizeof(PNode));
	while(j!=i){
		q=p;
		p=p->next;
		j++;
	}
	q->next=p->next;
}
int Len(){
	PNode *q;
	int j;
	q=head;
	j=0;
	while(q->next!=NULL){
		j++;
		q=q->next;
	}
	return j;
}
void Clear(){
	PNode *q,*p;
	p=head;
	p=p->next;
	while(p->next!=NULL){
		q=p;
		p=p->next;
	}
}
void show(){
	PNode *p;
	p=head;
	while(p->next!=NULL){
		p=p->next;
		printf("%c",p->date);
	}
	printf("\n");
}

void main(){
	int i;
	chushi();
	show();
	scanf("%d",&i);getchar();
	Insert(i);
	show();
	Delete(i);
	show();
	printf("\n\t\t\t%d",Len());
	Clear();
	show();
	printf("\nyiqingchule");
}


```