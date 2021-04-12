---
title: "线性栈"
date: 2020-03-05T13:48:55+08:00

lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["线性栈","数据结构"]
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

通过链表实现栈结构

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