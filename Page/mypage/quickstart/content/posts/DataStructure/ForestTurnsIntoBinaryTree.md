---
title: "森林转成二叉树"
date: 2020-03-05T13:48:55+08:00

lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["森林转成二叉树","数据结构"]
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
#include<malloc.h>
typedef struct CSNode{
	char data;
	struct CSNode *firstchild,*nextsibling;
}CSN;
CSN *Init(){
	CSN *p;
	char x;
	scanf("%c",&x);getchar();
	if(x!=' '){
		p=(CSN *)malloc(sizeof(CSN));
		p->data=x;
		printf("要不要给%c结点创造子节点",p->data);
		p->firstchild=Init();
		printf("要不要给%c结点创造兄弟",p->data);
		p->nextsibling=Init();
	}
	else{
		p=NULL;
	}
	return p;
}
void xianxu(CSNode *p){
	if(p==NULL)
		return ;
	else{
		printf("%c\t",p->data);
		if(p->firstchild!=NULL)
			xianxu(p->firstchild);
		if(p->nextsibling!=NULL)
			xianxu(p->nextsibling);
	}
}
void zhongxu(CSNode *p){
	if(p==NULL)
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
	CSN *p;
	p=Init();
	xianxu(p);
	printf("\n");
	zhongxu(p);
}
```