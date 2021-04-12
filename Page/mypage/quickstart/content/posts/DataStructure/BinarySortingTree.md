---
title: "二叉排序树"
date: 2020-03-05T13:48:55+08:00
lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["二叉排序树","数据结构"]
categories: ["c语言","数据结构"]
featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  # ...
math:
  enable: true
  # ...
mapbox:
  accessToken: ""
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---
二叉排序树:<br>
一棵空树，或者是具有下列性质的二叉树：<br>
（1）若左子树不空，则左子树上所有结点的值均小于它的根结点的值；<br>
（2）若右子树不空，则右子树上所有结点的值均大于它的根结点的值；<br>
（3）左、右子树也分别为二叉排序树；<br>
（4）没有键值相等的结点。<br>

``` C
#include<stdio.h>
#include<malloc.h>
#include<string.h>
#define MAX 10
typedef struct BtreeNode{
	char	data[MAX];
	int i;
	struct BtreeNode *lchild,*rchild;
}BN;
int bijiao(char a[MAX],char b[MAX]){
	if(strlen(a)>strlen(b))
		return 1;
	if(strlen(a)<strlen(b))
		return -1;
	if(strlen(a)==strlen(b)){
		int i;
		i=strcmp(a,b);
		return i;
	}
}
void InsertBtree(BN *p,char key[MAX]){
	BN *q,*s,*l;
	q=s=p;
	int m;
	while(s!=NULL){
		m=bijiao(s->data,key);
		if(m>0)
		{q=s;s=s->lchild;}
		else{
				q=s;s=s->rchild;
		}
	}
	l=(BN *)malloc(sizeof(BN));
	l->lchild=NULL;
	l->rchild=NULL;
	strcpy(l->data,key);
	if(m>0){
		q->lchild=l;
	}
	else{
		q->rchild=l;
	}
}
BN *InitBtree(){
	char x[MAX];
	int flag;
	BN *p,*head;
	head=NULL;
	scanf("%s",&x);
	flag=0;
	while(x[0]!='$'){
		if(flag==0){
			p=(BN *)malloc(sizeof(BN));
			p->lchild=NULL;
			p->rchild=NULL;
			head=p;
			strcpy(p->data,x);
			flag=1;
		}
		else{
			InsertBtree(head,x);
		}
		scanf("%s",&x);
	}
	return p;
}
void show(BN *p)
{
	if (p!=NULL)
	{ 
		show(p->lchild);
		printf("%s\t",p->data);
		show(p->rchild );
	}
}
void Delete(BN *p,char x[MAX]){
	BN *s,*q,*n;
	int m;
	q=s=p;
	m=bijiao(p->data,x);
	if(m==0){
		p->lchild=NULL;
		p->rchild=NULL;
		return;
	}
	while(s!=NULL){
		m=bijiao(s->data,x);
		if(m>0)
		{q=s;s=s->lchild;}
		if(m<0)
		{q=s;s=s->rchild;}
		if(m==0)
			break;
	}
	if(s!=NULL){
		if(s->lchild!=NULL){
			n=s->lchild;
			while(n->rchild!=NULL)
				n=n->rchild;
			if(q->lchild==s){
				n->rchild=s->rchild;
				q->lchild=s->lchild;
			}
			else{
				n->rchild=s->rchild;
				q->rchild=s->lchild;
			}
		}
		else{
			if(q->lchild==s)
				q->lchild=s->rchild;
			else
				q->rchild=s->rchild;
		}
		if(s->lchild==NULL&&s->rchild==NULL){
			if(q->lchild==s)
				q->lchild=NULL;
			if(q->rchild==s)
				q->rchild=NULL;
		}
	}
	else{
		printf("没有这个数据");
	}
}
void main(){
	BN *p;
	BN *l;
	char x[MAX];
	p=InitBtree();
	show(p);
	scanf("%s",&x);
	Delete(p,x);
	show(p);
}
}
```