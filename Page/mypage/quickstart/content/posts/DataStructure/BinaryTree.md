---
title: "二叉树"
date: 2020-03-05T13:48:55+08:00
lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["二叉树","数据结构"]
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

``` C
#include<stdio.h>
#include<stdlib.h>
typedef char AdjType;
typedef struct Btree{
	AdjType date;
	struct Btree *lchild,*rchild;
}btree;
int count;
int i;
btree *Init(){
	btree *p;
	char x;
	scanf("%c",&x);getchar();
	if(x ==' '){
		p=NULL;	
	}
	else{
		p=(btree *)malloc(sizeof(btree));
		p->date=x;
		printf("\n\t\t\t请输入一个非#的字符,当前访问到%c的左子女",p->date);
		p->lchild=Init();
		printf("\n\t\t\t请输入一个非#的字符,当前访问到%c的右子女",p->date);
		p->rchild=Init();
	}
	return p;
}
void xianxu(btree *p){
	if(p==NULL)
		return;
	printf("%c",p->date);
	xianxu(p->lchild);
	xianxu(p->rchild);
}
void zhongxu(btree *p){
	if(p==NULL)
		return;
	zhongxu(p->lchild);
	printf("%c",p->date);
	zhongxu(p->rchild);
}
void houxu(btree *p){
	if(p==NULL)
		return;
	houxu(p->lchild);
	houxu(p->rchild);
	printf("%c",p->date);
}
int yezi(btree *p){
	if(p!=NULL){
		if(p->lchild==NULL&&p->rchild==NULL){
			count++;
			return count;
		}
		else{
			yezi(p->lchild);
			yezi(p->rchild);
		}
	}
	else{
		return i;
	}
}
int bNodecount(btree *p){
	if(p!=NULL){
		count++;
		bNodecount(p->lchild);
		bNodecount(p->rchild);
		return count;
	}
}
int Treedeep(btree *p){
	int ldeep,rdeep;
	if(p==NULL)
		return 0;
	ldeep=Treedeep(p->lchild);
	rdeep=Treedeep(p->rchild);
	if(ldeep>rdeep)
		return (ldeep+1);
	else
		return (rdeep+1);
}
void main(){
	int i;
	btree *p;
	char choice;
	choice='1';
	while(choice!='0'){
		count=0;
		printf("\n\n\n\n");
		printf("\t\t\t-二叉树的基本运算--\n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------建 二  叉 树       *");
		printf("\n\t\t\t*       2-------先 序  遍 历       *");
		printf("\n\t\t\t*       3-------中 序  遍 历       *");
		printf("\n\t\t\t*       4-------后 序  遍 历       *");
		printf("\n\t\t\t*       5-------统计  叶子数       *");
		printf("\n\t\t\t*       6-------统计  结点数       *");
		printf("\n\t\t\t*       7-------求二叉树深度       *");
		printf("\n\t\t\t*       0-------退        出       *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t 请选择菜单号(0--7): ");
		scanf("%c",&choice);getchar();
		if(choice=='1'){
			p=Init();
		}
		if(choice=='2'){
			xianxu(p);
		}
		if(choice=='3')
			zhongxu(p);
		if(choice=='4')
			houxu(p);
		if(choice=='5'){
			i=0;
			i=yezi(p);
			printf("%d",i);
		}
		if(choice=='6'){
			i=0;
			i=bNodecount(p);
			printf("%d",i);
		}
		if(choice=='7'){
			i=0;
			i=Treedeep(p);
			printf("%d",i);
		}
	}
}
```