---
title: "线性二叉树"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
#define MAX 100
typedef char CharType;
typedef struct btree{
	int		llist;
	CharType data;
	int		rlist;
}B;
typedef struct queueNode{
	int data;
	struct queueNode *next;
}Q;
typedef struct queuelist{
	Q *front;
	Q *rear;
}L;
B BiTree[MAX+1];
int count;
int c;
void createBiTree(B *T, int root){
	CharType e;
	scanf("%c", &e);getchar();
	if(e!='#'){
		if(e!='0'&&T[root].llist!=0&&T[root].rlist!=0){
			T[root].data = e;
			T[root].llist=2*root;
			printf("你现在输入的位置是：%d\t\t",T[root].llist);
			createBiTree(T, T[root].llist);
			T[root].rlist=2*root+1;
			printf("你现在输入的位置是：%d\t\t",T[root].rlist);
			createBiTree(T, T[root].rlist);
		}else{
			T[root].data='0';
			T[root].llist=0;
			T[root].rlist=0;
		}
	}
}
void Init(L* p){
	p->rear=p->front=(Q *)malloc(sizeof(Q));
	if(p->front->next==NULL)
		printf("\n\t\t\t分配内存失败");
	else
		printf("\n\t\t\t内存分配成功");
	p->rear->next=NULL;
}
void Insert(L *p,int x){
	Q *q;
	q=(Q *)malloc(sizeof(Q));
	q->data=x;
	p->rear->next=q;
	p->rear=q;
	q->next=NULL;
}
int Delete(L *p){
	Q *q;
	if(p->front==p->rear)
		return (0);
	else{
		q=p->front->next;
		p->front=p->front->next;
		return (q->data);
	}
}
void xianxu(B *s,int root,char *q,int i){
	if(s[root].llist==0&&s[root].rlist==0)
		return;
	else{
		q[i]=s[root].data;
		xianxu(s,s[root].llist,q,i++);
		xianxu(s,s[root].rlist,q,i++);
	}
}
void zhongxu(B *s,int root){
	if(s[root].llist==0&&s[root].rlist==0)
		return;
	else{
		zhongxu(s,s[root].llist);
		printf("%c",s[root].data);
		zhongxu(s,s[root].rlist);
	}
}
void houxu(B *s,int root){
	if(s[root].llist==0&&s[root].rlist==0)
		return;
	else{
		houxu(s,s[root].llist);
		houxu(s,s[root].rlist);
		printf("%c",s[root].data);
	}
}
int yezi(B *s,int root){
	int m,l;
	if(s[root].llist==0&&s[root].rlist==0){
		return 0;
	}
	else{
		m=yezi(s,s[root].llist);
		l=yezi(s,s[root].rlist);
		if(m==0&&l==0)
			count++;
		return count;
	}
}
int jiedian(B *s,int root){
	if(s[root].llist==0||s[root].rlist==0){
		return 0;
	}
	else{		
		count++;
		jiedian(s,s[root].llist);
		jiedian(s,s[root].rlist);
		return count;
	}	
}
int deep(B *s,int root){
	int m,l;
	m=0;
	l=0;
	if(s[root].llist==0||s[root].rlist==0){
		return 0;
	}
	else{
		m=deep(s,s[root].llist);
		l=deep(s,s[root].rlist);
		if(m>l)
			return m+1;
		else
			return l+1;
	}	
}
void shenduyouxian(B *s,int root,int *tag){
	if((s[root].data=='0')&&tag[root/2])
		return ;
	else{
		printf("%c\t",s[root].data);
		*(tag+root)=1;
		shenduyouxian(s,s[root].llist,tag);
		shenduyouxian(s,s[root].rlist,tag);
		return ;
	}
}
void guangdubianli(B *s,int root,L *p,int *tag){
	int i;
	i=Delete(p);
	if(s[i].data!='0'&&s[i].data>0&&tag[i]==0){
		tag[i]=1;
		printf("%c\t",s[i].data);
	}
	if(s[root].data!='0'&&s[root].data>0){
		Insert(p,s[root].llist);
		Insert(p,s[root].rlist);
	}
	guangdubianli(s,i,p,tag);
}
void guangduyouxian(B *s){
	int i,m,flag;
	m=jiedian(s,1);
	flag=0;
	printf("\n");
	for(i=1;i<MAX;i++){
		if(flag+1==m||s[i].data<0)
			break;
		if(s[i].data!='0'){
			flag++;
			printf("%c\t",s[i].data);
		}
	}
}
void main(){
	B tree[MAX+1];
	L p;
	char s[MAX];
	int i,root,pos;
	int visited[MAX+1];
	for(i=0;i<MAX;i++)
		visited[i]=0;
	count=0;
	root=1;
	printf("你现在输入的位置是：%d\t\t",root);
	createBiTree(tree,root);
	i=0;
	xianxu(tree,root,s,i);
	printf("\n");
	zhongxu(tree,root);
	printf("\n");
	houxu(tree,root);
	printf("\n%d",yezi(tree,root));
	count=0;
	printf("\n%d",jiedian(tree,root));
	printf("\n%d",deep(tree,root));
	printf("\n");
	shenduyouxian(tree,root,visited);
	guangduyouxian(tree);
	i=1;
	for(i=0;i<MAX;i++)
		visited[i]=0;
	Init(&p);
	Insert(&p,1);
	printf("\n\t\t");
	guangdubianli(tree,i,&p,visited);	
}

```