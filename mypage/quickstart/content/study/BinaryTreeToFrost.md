---
title: "二叉树转化成树"
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
CN *Init2(){
	CN *p;
	char x;
	scanf("%c",&x);getchar();
	if(x!=' '){
		p=(CN *)malloc(sizeof(CN));
		p->data=x;
		printf("要不要给%c结点创造子节点",p->data);
		p->firstchild=Init2();
		printf("要不要给%c结点创造兄弟",p->data);
		p->nextsibling=Init2();
	}
	else{
		p=NULL;
	}
	return p;
}
void xianxu(CN *p){
	if(p==NULL){
		return ;
	}
	else{
		printf("%c\t",p->data);
		xianxu(p->firstchild);
		xianxu(p->nextsibling);
	}
}
void zhongxu(CN *p){
	if(p->firstchild==NULL&&p->nextsibling==NULL)
		return ;
	else{
		zhongxu(p->firstchild);	
		printf("%c\t",p->data);
		zhongxu(p->nextsibling);
	}
}
void huanyuan(CSTr *q,int root,CN *p,CST *l){
	if(p==NULL){
		return ;
	}
	else{
		if(p->firstchild!=NULL){
			root++;
			q->nodes[root].firstchild=(CST *)malloc(sizeof(CST));
			q->nodes[root].data=p->firstchild->data;
			q->nodes[root].firstchild->next=NULL;
			q->nodes[root-1].firstchild->child=root;
			huanyuan(q,root,p->firstchild,q->nodes[root-1].firstchild);
		}
		else{
			root++;
			l->next=(CST *)malloc(sizeof(CST));
			l->next->next=NULL;
		}
		if(p->nextsibling!=NULL){
			CST *n;
			root++;
			l->next=n=(CST *)malloc(sizeof(CST));
			q->nodes[root].firstchild=(CST *)malloc(sizeof(CST));
			q->nodes[root].data=p->nextsibling->data;
			q->nodes[root].firstchild->next=NULL;
			n->child=root;
			n->next=NULL;
			l->next=n;
			l=n;
			huanyuan(q,root,p->nextsibling,n);
		}
		else{ 
			root++;
			l->next=(CST *)malloc(sizeof(CST)); 	
			l->next->next=NULL;
		}
	}	
}
int flag=0;
void xianxu2(CSTr *p,int root,int *tag){
	if(p->nodes[root].firstchild->next==NULL&&tag[root]==1){
		printf("lhlh");
		return ;
	}
	else{
		CST *q;
		if(flag!=1){
			printf("%c\t",p->nodes[0].data);
			flag=1;	
		}
		q=p->nodes[root].firstchild;
		while(q->next!=NULL&&tag[root]!=1){
			printf("%c\t",p->nodes[q->child].data);
			xianxu2(p,q->child,tag);
			tag[q->child]=1;
			q=q->next;
		}
	}
}
void zhongxu2(CSTr *p,int root,int *tag){
	if(p->nodes[root].firstchild->next==NULL&&tag[root]==1){
		printf("lhlh");
		return ;
	}
	else{
		CST *q;
		if(flag!=1){
			printf("%c\t",p->nodes[0].data);
			flag=1;	
		}
		q=p->nodes[root].firstchild;
		while(q->next!=NULL&&tag[root]!=1){
			zhongxu2(p,q->child,tag);
			printf("%c\t",p->nodes[q->child].data);
			tag[q->child]=1;
			q=q->next;
		}
	}
}
void main(){
	CSTr p[3];
	CN q[3];
	CST s;
	int visited[MAX]={0};
	int j,i=0;
	q[0]=*Init2();
	p->nodes[0].firstchild=(CST *)malloc(sizeof(CST));
	p->nodes[0].data=(q+0)->data;
	huanyuan(p,0,q,&s);
	i=0;
	xianxu2(p,i,visited);
	flag=0;
	printf("ADDD");
	zhongxu2(p,i,visited);
}


```