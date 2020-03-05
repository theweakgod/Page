---
title: "A*算法解决8数码问题"
date: 2020-03-05T14:56:50+08:00
draft: flase
---

```c
#include<stdio.h>
#include<malloc.h>
char arr[9];
char end[9];
char skip[9];
int Fan;
typedef struct EightNode{
	char arr[9];
	struct EightNode *parents;
	struct EightNode *OL;
	struct EightNode *OR;
	struct EightNode *OU;
	struct EightNode *OD;
	int site;
	int number;
}EN;
void swap(char *a1,char *b1){
	char t;
	t=*a1;
	*a1=*b1;
	*b1=t;
}
int up(char *a){
	if(Fan<3){
		return 0;
	}
	else{
		swap(a+Fan,a-3+Fan);
	}
	return 1;
}
int down(char *a){
	if(Fan>5){
		return 0;
	}
	else{
		swap(a+Fan,a+Fan+3);
	}
	return 1;
}
int left(char *a){
	if(Fan%3==0){
		return 0;
	}
	else{
		swap(a+Fan,a+Fan-1);
	}
	return 1;
}
int right(char *a){
	if(Fan%3==2){
		return 0;
	}
	else{
		swap(a+Fan,a+Fan+1);
	}
	return 1;
}
void equator(char *p1,char *p2){
	int i;
	for(i=0;i<9;i++){
		*(p1+i)=*(p2+i);
	}
}
EN *head;
void Init(int site){
	int i,j,k,t,tmp,po,flag;
	int z,judg;
	int a,b;
	EN *NNN;
	EN *q,*p,*monitor;
	a=b=0;
	if(arr[0]==end[0]&&arr[1]==end[1]&&arr[2]==end[2])a=1;
	z=1;
	tmp=1;
	printf("请输入一个最大层数");
	scanf("%d",&t);getchar();
	head=(EN *)malloc(sizeof(EN));
	head->number=1;
	p=monitor=NNN=(EN *)malloc(sizeof(EN));
	NNN->OD=NNN->OL=NNN->OR=NNN->OU=NNN->parents=NULL;
	equator(NNN->arr,skip);
	head->site=site;
	p=head;
	head->OU=head->OD=head->OL=head->OR=NNN;
	head->parents=NULL;
	equator(head->arr,arr);
	while(1){
		judg=0;
		printf("\n");
		flag=0;
		po=0;
		Fan=p->site;
		if(a==1&&Fan<5){
			p->OU=NULL;
		}
		if(p->OU==NNN&&flag==0){
			q=(EN *)malloc(sizeof(EN));
			equator(q->arr,p->arr);
			if(up(q->arr)==0){
				printf("不被允许的操作，向上越界");
				p->OU=NULL;
				free(q);
			}
			else{
				Fan=Fan-3;
				q->site=Fan;
				printf("成功向上移动");
				q->OD=q->OL=q->OR=q->OU=q->parents=NNN;
				q->parents=p;
				q->number=q->parents->number+1;
				p->OU=q;
				p=q;
				judg=1;
			}
			flag=1;
			printf("\n");
		}
		if(p->OD==NNN&&flag==0){
			q=(EN *)malloc(sizeof(EN));
			equator(q->arr,p->arr);
			if(down(q->arr)==0){
				printf("不被允许的操作，向下越界");
				p->OD=NULL;
				free(q);
			}
			else{
				Fan=Fan+3;
				q->site=Fan;
				printf("成功向下移动");
				q->OD=q->OL=q->OR=q->OU=q->parents=NNN;
				q->parents=p;
				q->number=q->parents->number+1;
				p->OD=q;
				p=q;
				judg=2;
			}
			flag=1;
		}
		if(p->OL==NNN&&flag==0){
			q=(EN *)malloc(sizeof(EN));
			equator(q->arr,p->arr);
			if(left(q->arr)==0){
				printf("不被允许的操作，向左越界");
				p->OL=NULL;
				free(q);
			}
			else{
				Fan=Fan-1;
				q->site=Fan;
				printf("成功向左移动");
				q->OD=q->OL=q->OR=q->OU=q->parents=NNN;
				q->parents=p;
				q->number=q->parents->number+1;
				p->OL=q;
				p=q;
				judg=3;
			}
			flag=1;
		}
		if(p->OR==NNN&&flag==0){
			q=(EN *)malloc(sizeof(EN));
			equator(q->arr,p->arr);
			if(right(q->arr)==0){
				printf("不被允许的操作，向右越界");
				p->OR=NULL;
				free(q);
			}
			else{
				Fan=Fan+1;
				q->site=Fan;
				printf("成功向右移动");
				q->OD=q->OL=q->OR=q->OU=q->parents=NNN;
				q->parents=p;
				q->number=q->parents->number+1;
				p->OR=q;
				p=q;
				judg=4;
			}
			flag=1;
		}
		printf("\n");
		for(k=0;k<9;k++){	
			if(end[k]!=p->arr[k]){
				po=1;
				break;
			}
			printf("%c,%c\t",p->arr[k],end[k]);
			if(k%3==2){
				printf("\n");
			}
		}
		printf("这个矩阵层数是：%d\n",p->number);
		if(po==0){
			printf("\n找到了！");
			break;
		}
		if(p==head&&head->OD!=NNN&&head->OL!=NNN&&head->OR!=NNN&&head->OU!=NNN){
			printf("\n没找到！");
			break;
		}
		if(p->OD!=NNN&&p->OL!=NNN&&p->OR!=NNN&&p->OU!=NNN){
			printf("全为空");
			monitor->OD=p;
			p=p->parents;
			monitor->OD=NULL;
		}
		if(p->number>t){
			p=p->parents;
			if(judg==1){
				p->OU=NULL;
			}
			if(judg==2){
				p->OD=NULL;
			}
			if(judg==3){
				p->OL=NULL;
			}
			if(judg==4){
				p->OR=NULL;
			}
		}
	}
}
void main(){
	int i,k;
	for(i=0;i<9;i++){
		scanf("%c",arr+i);getchar();
		if(arr[i]==' ')k=i;
	}
	for(i=0;i<9;i++){
		scanf("%c",end+i);getchar();
	}
	Init(k);
}
```