---
title: "树转森林,森林转树"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
typedef char TElemType;
#define	MAX	100
typedef struct PTNode{
	TElemType	data;
	int			parent;
}P;
typedef struct{
	P nodes[MAX];
	int	r,n;
}PTree;
void Init(PTree *p){
	char x;
	int i,j;
	i=1;
	p->r=p->n=0;
	p->nodes[0].parent=-1;
	p->n++;
	printf("\n\t\t\t输入规则，如果输入#号直接结束输入，如果输入空格也就是 你输入的结点的双亲会跳到下一个位置");
	printf("\n\t\t\t请输入根的值；");
	scanf("%c",&x);getchar();
	p->nodes[0].data=x;
	printf("\n\t\t\t请输入你要输入的结点值");
	scanf("%c",&x);getchar();
	while(x!='#'){
		if(x!=' '){
		p->nodes[i].data=x;
		p->nodes[i].parent=p->r;
		printf("\n\t\t\t结点值是：%c",p->nodes[i].data);
		i++;
		p->n++;
		}
		else{
			p->r++;
		}
		if(p->r>i){
			printf("\n\t\t\t你输入的位置已经越界\n");
			break;
		}
		printf("\n\t\t\t你输入的该结点的双亲是：%d",p->r);
		printf("\n\t\t\t(#号决定是否结束，空格决定是否换双亲)\n\t\t\t请输入下一个值:");
		scanf("%c",&x);getchar();
	}
}
void Insert(PTree *p){
	char x;
	int j;
	scanf("%c",&x);getchar();
	scanf("%d",&j);
	p->nodes[++p->n].data=x;
	p->nodes[p->n].parent=j;
	printf("%c,%d",p->nodes[p->n].data,p->nodes[p->n].parent);
}
int xunzhao(PTree *p){
	char x;
	int i;
	i=0;
	scanf("%c",&x);getchar();
	if(p->nodes[0].data==x)
		return 0;
	while(p->nodes[i].data!=x&&i<=p->n){
		i++;
	if(p->nodes[i].data==x){
		return i;
	}
	}
	return -1;
}
void diandaofu(PTree *p,int i){
	if(p->nodes[i].parent==-1){
		printf("%c",p->nodes[i].data);
		return;
	}
	printf("%c",p->nodes[i].data);
	diandaofu(p,p->nodes[i].parent);
}
void Delete(PTree *p){
	char x;
	int j,i;
	i=0;
	i=xunzhao(p);
	for(j=i;j<p->n;j++){
		p->nodes[j-1].data=p->nodes[j].data;
		p->nodes[j-1].parent=p->nodes[i].parent;
	}
	p->n--;
	printf("%c",p->nodes[i].data);
}
void main(){
	PTree p;
	int visited[MAX]={0};
	int i;
	Init(&p);
	//Insert(&p);
	//i=xunzhao(&p);
	//diandaofu(&p,i);
	Delete(&p);
}

```