---
title: "N皇后问题"
date: 2020-03-05T14:56:50+08:00
lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["N皇后问题","人工智能"]
categories: ["c语言","人工智能"]
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
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

<figure>
    <img src="/images/Nqueen.png"/>
    <figcaption>
        <h4>N皇后问题棋盘</h4>
    </figcaption>
</figure>

我们要做的就是把这种结果求出来,通过回朔法的方式把这个树建出来,在求解的过程中做剪枝,不用暴力破解这个答案,因为即使是8*8的所有解都是编译器难以接受的.
算法是我自己写的,没有优化.很久之前写的,更新博客的时候发现有这个就加进来了.

```c
#include<stdio.h>
#include<malloc.h>
#define Lenth 8
typedef struct QueenNode{
	char arr[Lenth*Lenth];			//定义棋盘
	int	 jugde[Lenth*Lenth];		//判断棋盘是否可插入用
	struct QueenNode *parents;
	int i;
	struct QueenNode *next;
}QN;
typedef struct QueueNode{
	QN *smallQueenNode;
	struct QueueNode *next;
}AQN;
typedef struct Queue{
	AQN *front;
	AQN *base;
}Q;
Q *queue;
void equator(QN *p,QN *q){			//使得两个结点的arr和judge相等
	int i;
	for(i=0;i<Lenth*Lenth;i++){
		*(q->arr+i)=*(p->arr+i);
		*(q->jugde+i)=*(p->jugde+i);
	}
}
int Insert(QN *L,int target){
	int i,lock1,lock2;
	int flag,number,floor,plant;
	flag=0;
	for(i=(L->i)*Lenth;i<(L->i+1)*Lenth;i++){
		if(*(L->jugde+i)==-1||*(L->jugde+i)==1)continue;
		if(*(L->jugde+i)==0&&(i%Lenth)==target){
			*(L->arr+i)='#';					//#为下在这个棋盘
			*(L->jugde+i)=1;
			flag=1;								//flag为信号，表示棋盘插入成功
			break;
		}
	}
	if(flag==0)return 0;
	number=i%Lenth;								//number为i所在行的第几位
	floor=i/Lenth;								//floor为插入点所在第几列
	plant=i;									//plant记录i在数组中的位置
	lock1=lock2=0;								//防止斜线不可插入越界
	if(flag==1){
		for(i=L->i*Lenth;i<Lenth*Lenth;i++){
			if(i==plant)continue;
			if(i%Lenth==number){				//横行不可插入
				*(L->jugde+i)=-1;
			}
			if(i/Lenth==floor){					//竖行不可插入
				*(L->jugde+i)=-1;
			}
			if(i==plant+(i/Lenth-floor)*(Lenth+1)){		//右斜线不可插入
				if(lock1==1)continue;
				if(i%Lenth==Lenth-1)lock1=1;
				*(L->jugde+i)=-1;
			}
			if(i==plant+(i/Lenth-floor)*(Lenth-1)){		//左斜线不可插入
				if(lock2==1)continue;
				if(i%Lenth==0)lock2=1;
				*(L->jugde+i)=-1;
			}
		}
	}
	return 1;
}
int display(QN *L,int i){								//通过递归显示数组
	int k;
	if(L->parents==NULL){
		return 0;
	}
	else{
		i=display(L->parents,i)+1;
		printf("step:%d\n",i);
		printf("************************************\n");
		printf("\t1\t2\t3\t4\t5\t6\n");
		for(k=0;k<Lenth*Lenth;k++){
			printf("\t%c",*(L->arr+k));
			if(k%Lenth==Lenth-1)printf("\n");
		}
		printf("************************************\n");
		return i;
	}
}
QN *Delete(){										//从队列中删除第一个节点并且通过返回值返回
	QN *L;
	L=queue->front->next->smallQueenNode;
	queue->front=queue->front->next;
	return L;
}
void queueInsert(QN *L){							//往队列里插入一个结点
	AQN *T;
	T=(AQN *)malloc(sizeof(AQN));
	T->smallQueenNode=(QN *)malloc(sizeof(QN));
	equator(L,T->smallQueenNode);
	T->smallQueenNode->i=L->i;
	T->smallQueenNode->parents=L->parents;
	queue->base->next=T;
	queue->base=queue->base->next;
	queue->base->next=NULL;
}
void fenpei(QN *L){
	L=(QN *)malloc(sizeof(QN));
}
void Init(){										
	int i,j,k;
	QN *reselution,*reselution2,*t;
	QN *head,*g,p[Lenth],*s;
	queue=(Q *)malloc(sizeof(Q));
	head=(QN *)malloc(sizeof(QN));
	reselution=(QN *)malloc(sizeof(QN));
	queue->front=queue->base=(AQN *)malloc(sizeof(AQN));
	queue->front->smallQueenNode=(QN *)malloc(sizeof(QN));
	queue->base->smallQueenNode=(QN *)malloc(sizeof(QN));
	g=(QN *)malloc(sizeof(QN));
	s=(QN *)malloc(sizeof(QN));
	queue->front->next=NULL;
	queue->base->next=NULL;
	reselution->next=NULL;
	head->i=0;
	head->parents=NULL;
	for(i=0;i<Lenth*Lenth;i++){				//初始化棋盘，空的为*
		*(head->arr+i)='*';
		*(head->jugde+i)=0;
	}
	queueInsert(head);
	reselution2=reselution->next;
	for(;queue->front->next!=NULL;){		//循环体当队列为空终止循环.
		s=Delete();							//读取队列队首结点
		if(s->i==Lenth){					//如果到最后一行就做一个链表存储这些
			s->next=reselution2;
			reselution->next=s;
			reselution2=reselution->next;
		}
		if(s->i>Lenth)break;				//到了lenth就不做了，中断
		for(k=0;k<Lenth;k++){
			equator(s,g);
			g->i=s->i;
			g->parents=s->parents;
			g->next=s->next;
			if(!Insert(g,k))continue;
			fenpei(p+k);
		 	p[k].next=NULL;
		 	equator(g,p+k);
		 	p[k].i=g->i+1;
		 	p[k].parents=s;
		 	queueInsert(p+k);
		}
	}
	for(t=reselution->next,i=1;t!=NULL;t=t->next,i++){
		printf("第%d种解题步骤是：\n",i);								//依照链表输出
		display(t,0);													//通过parents指针，递归显示步骤
	}
}
void main(){
	Init();
}

```