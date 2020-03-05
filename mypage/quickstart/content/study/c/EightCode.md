---
title: "A*算法解决8数码问题"
date: 2020-03-05T14:56:50+08:00
draft: flase
---

```c
#include<stdio.h>
#include<malloc.h>
#include<stdlib.h>
#define Lenth 3
char arr[Lenth*Lenth];
char end[Lenth*Lenth];
typedef struct EightNode{
	char arr[Lenth*Lenth];					//8数码
	struct EightNode *parents;		//指针指向上个结点
	int site;						//位置
	int number;						//F(N)
	int ceng;						//当前层数
}EN;
typedef struct QueueNode{
	EN *smallEightNode;
	struct QueueNode *next;
}QN;
void swap(char *a1,char *b1){
	char t;
	t=*a1;
	*a1=*b1;
	*b1=t;
}
int up(char *a,int *Fan){
	if(*Fan<Lenth){
		return 0;
	}
	else{
		swap(a+*Fan,a-Lenth+*Fan);
		*Fan=*Fan-Lenth;
	}
	return 1;
}
int down(char *a,int *Fan){
	if(*Fan>Lenth*Lenth-Lenth-1){
		return 0;
	}
	else{
		swap(a+*Fan,a+*Fan+Lenth);
		*Fan=*Fan+Lenth;
	}
	return 1;
}
int left(char *a,int *Fan){
	if(*Fan%Lenth==0){
		return 0;
	}
	else{
		swap(a+*Fan,a+*Fan-1);
		*Fan=*Fan-1;
	}
	return 1;
}
int right(char *a,int *Fan){
	if(*Fan%Lenth==Lenth-1){
		return 0;
	}
	else{
		swap(a+*Fan,a+*Fan+1);
		*Fan=*Fan+1;
	}
	return 1;
}
void equator(char *p1,char *p2){
	int i;
	for(i=0;i<Lenth*Lenth;i++){
		*(p1+i)=*(p2+i);
	}
}
int adept(char *a){
	int i;
	int sum=0;
	for(i=0;i<Lenth*Lenth;i++){
		if(*(a+i)==end[i]){
			sum++;
		}
	}
	return sum;
}
QN *open;
QN *close;
int k;
QN *Delete(){
	QN *p,*q;
	p=close;
	q=close->next;
	p->next=q->next;
	return q;
}
void CloseInsert(QN *L){
	QN *p,*q;
	QN *s;
	s=(QN *)malloc(sizeof(QN));
	s->smallEightNode=(EN *)malloc(sizeof(EN));
	equator(s->smallEightNode->arr,L->smallEightNode->arr);
	s->smallEightNode->ceng=L->smallEightNode->ceng;
	s->smallEightNode->site=L->smallEightNode->site;
	s->smallEightNode->number=L->smallEightNode->number;
	s->smallEightNode->parents=L->smallEightNode->parents;
	s->next=NULL;
	p=close;
	q=close->next;
	if(q==NULL){
		p->next=s;
		s->next=q;
		return ;
	}
	for(;q!=NULL&&q->smallEightNode->number<=s->smallEightNode->number;){
		p=p->next;
		q=q->next;
	}
	p->next=s;
	s->next=q;
	return;
}
int CloseIsUsed(QN *L){					//看close表中有没有该结点，如果有，以number小的为准
	QN *q,*p;
	int i,po;
	p=close;
	q=close->next;
	while(q!=NULL){
		po=0;
		for(i=0;i<Lenth*Lenth;i++)
		if(*(q->smallEightNode->arr+i)!=*(L->smallEightNode->arr+i))
			po=1;
		if(po==0){
			break;}
		p=p->next;
		q=q->next;
	}
	if(po==0){
		if(L->smallEightNode->number<q->smallEightNode->number){
			p->next=q->next;
			CloseInsert(L);
		}
		return 0;
	}
	else{
		return 1;
	}
}
void OpenInsert(QN *L){
	QN *p,*q;
	QN *s;
	s=(QN *)malloc(sizeof(QN));
	s->smallEightNode=(EN *)malloc(sizeof(EN));
	equator(s->smallEightNode->arr,L->smallEightNode->arr);
	s->smallEightNode->ceng=L->smallEightNode->ceng;
	s->smallEightNode->site=L->smallEightNode->site;
	s->smallEightNode->number=L->smallEightNode->number;
	s->next=NULL;
	p=open;
	q=open->next;
	if(q==NULL){
		p->next=s;
		s->next=q;
		return ;
	}
	for(;q!=NULL&&p->smallEightNode->number<=s->smallEightNode->number;){
		p=p->next;
		q=q->next;
	}
	p->next=s;
	s->next=q;
	return;
}
int OpenIsused(QN *L){
	QN *p,*q;
	int i,po;
	p=open;
	q=open->next;
	while(q!=NULL){
		po=0;
		for(i=0;i<Lenth*Lenth;i++)
		if(*(q->smallEightNode->arr+i)!=*(L->smallEightNode->arr+i))
			po=1;
		if(po==0){
			break;}
		p=p->next;
		q=q->next;
	}
	if(po==0){
		if(L->smallEightNode->number>q->smallEightNode->number){
			p->next=q->next;
			OpenInsert(L);
		}
		return 0;
	}
	else{
		return 1;
	}
}
int display(EN *g,int i){
	int k;
	if(g->parents==NULL){
		printf("step:%d\n",i);
		printf("********************************************************\n");
		for(k=0;k<Lenth*Lenth;k++){
			printf("\t%c",*(g->arr+k));
			if(k%Lenth==Lenth-1)printf("\n");
		}
		printf("********************************************************\n");
		return 0;
	}
	else{
		i=display(g->parents,i)+1;
		printf("step:%d\n",i);
		printf("********************************************************\n");
		for(k=0;k<Lenth*Lenth;k++){
			printf("\t%c",*(g->arr+k));
			if(k%Lenth==Lenth-1)printf("\n");
		}
		printf("********************************************************\n");
		return i;
	}
}
int IsEmpty(QN *L){
	if(L->next==NULL)return 1;
	return 0;
}
void Init(int site){
	int i,j,k,flag;
	int step;
	QN *q,p[4],*h,*g;
	int search[4],po,min;
	QN *openp,*openq;
	QN *head;
	step=0;
	i=1;
	head=(QN *)malloc(sizeof(QN));
	open=(QN *)malloc(sizeof(QN));
	close=(QN *)malloc(sizeof(QN));
	open->smallEightNode=(EN *)malloc(sizeof(EN));
	close->smallEightNode=(EN *)malloc(sizeof(EN));
	head->smallEightNode=(EN *)malloc(sizeof(EN));
	p[0].smallEightNode=(EN *)malloc(sizeof(EN));
	p[1].smallEightNode=(EN *)malloc(sizeof(EN));
	p[2].smallEightNode=(EN *)malloc(sizeof(EN));
	p[3].smallEightNode=(EN *)malloc(sizeof(EN));
	equator(head->smallEightNode->arr,arr);
	head->next=NULL;
	head->smallEightNode->site=site;
	head->smallEightNode->ceng=1;
	head->smallEightNode->number=Lenth*Lenth-adept(head->smallEightNode->arr)+i;
	open->next=NULL;
	close->next=NULL;
	head->smallEightNode->parents=NULL;
	CloseInsert(head);
	for(;i<40000;i++){
			g=Delete();
			if(!OpenIsused(g))continue;
			po=0;
			for(k=0;k<Lenth*Lenth;k++){
				if(*(g->smallEightNode->arr+k)!=*(end+k)){
					po=1;
				}
			}
			if(po==0){
				printf("找到了！！！\n");
				display(g->smallEightNode,step);
				return ;
			}
			for(k=0;k<4;k++){
				p[k].next=NULL;
				equator(p[k].smallEightNode->arr,g->smallEightNode->arr);
				p[k].smallEightNode->number=100000;
				p[k].smallEightNode->site=g->smallEightNode->site;
				p[k].smallEightNode->ceng=g->smallEightNode->ceng+1;
				search[k]=0;
			}
			if(up(p[0].smallEightNode->arr,&p[0].smallEightNode->site)){
				search[0]=1;
				p[0].smallEightNode->number=Lenth*Lenth-adept(p[0].smallEightNode->arr)+g->smallEightNode->ceng+1;
			}
			if(down(p[1].smallEightNode->arr,&p[1].smallEightNode->site)){
				search[1]=1;
				p[1].smallEightNode->number=Lenth*Lenth-adept(p[1].smallEightNode->arr)+g->smallEightNode->ceng+1;
			}
			if(left(p[2].smallEightNode->arr,&p[2].smallEightNode->site)){
				search[2]=1;
				p[2].smallEightNode->number=Lenth*Lenth-adept(p[2].smallEightNode->arr)+g->smallEightNode->ceng+1;
			}
			if(right(p[3].smallEightNode->arr,&p[3].smallEightNode->site)){
				search[3]=1;
				p[3].smallEightNode->number=Lenth*Lenth-adept(p[3].smallEightNode->arr)+g->smallEightNode->ceng+1;
			}
		for(k=0;k<4;k++){
				if(search[k]==1){
					p[k].smallEightNode->parents=g->smallEightNode;
					if(OpenIsused(p+k)){
						if(CloseIsUsed(p+k)){
							CloseInsert(p+k,g);
						}
					}
				}
			}
		OpenInsert(g);
	}
	printf("\n没有搜索到结果！\n");
}
void main(){
	int i,k;
	k=0;
	printf("请输入初始图形：\n");
	for(i=0;i<Lenth*Lenth;i++){
		scanf("%c",arr+i);getchar();
	}
	printf("请输入目标图形：\n");
	for(i=0;i<Lenth*Lenth;i++){
		scanf("%c",end+i);getchar();
	}
	for(i=0;i<Lenth*Lenth;i++){
		printf("%c\t",*(end+i));
		if(*(arr+i)==' ')k=i;
		if(i%Lenth==Lenth-1)printf("\n");
	}
	Init(k);
	system("pause");
}
```