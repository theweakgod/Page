---
title: "大数加法"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#include<malloc.h>
#include<string.h>
#define MAX 10
#define M	100
typedef struct HTNode{
	char data[MAX];
	int i;
	struct HTNode *Lchild,*Rchild;
}Huffman;
typedef struct ListNode{
	int		data;
	struct ListNode *next;
}LN;
LN *head;
void INit(){
	LN *q,*p;
	int x;
	head=(LN *)malloc(sizeof(LN));
	p=head;
	head->next=NULL;
	scanf("%d",&x);getchar();
	while(x!=0){
		q=(LN *)malloc(sizeof(LN));
		q->next=NULL;
		q->data=x;
		p->next=q;
		p=q;
		scanf("%d",&x);getchar();
	}
}
void Insert(int x){
	LN *q,*s;
	q=head;
	while(q->next!=NULL){
		q=q->next;
	}
	s=(LN *)malloc(sizeof(LN));
	q->next=s;
	s->data=x;
	s->next=NULL;
}
void Delete(LN *p,int i){
	LN *q,*s;
	q=s=head;
	if(s->next==NULL)
		return ;
	while(s->data!=i&&s!=NULL){
		q=s;
		s=s->next;
	}
	if(s!=NULL)
	q->next=s->next;
}
int min(LN *p){
	LN *q,*s;
	int i;
	q=head;
	i=q->next->data;
	while(q->next!=NULL){
		q=q->next;
		if(q->data<=i){
			i=q->data;
			printf("{%d}",i);
		}
	}
	return i;
}
Huffman *InitHuffman( ){
	Huffman *L,*R,*p,*s,*f,*t,*z[M];
	LN *q;
	int l,r,i,j,flag,k;
	flag=-1;
	i=k=0;
	j=0;
	INit( );
	q=head;
	l=min(head);
	z[i]=(Huffman *)malloc(sizeof(Huffman));
	s=(Huffman *)malloc(sizeof(Huffman));
	z[i]->Lchild=z[i]->Rchild=s->Lchild=s->Rchild=NULL;
	z[i]->i=0;
	s->i=l;
	Delete(head,l);
	r=min(head);
	Delete(head,r);
	while(1){
		for(j=0;j<=i;j++){
			if(z[j]->i==l||z[j]->i==r){
				flag=1;
				printf("flag==1");
				break;
			}
		}
		for(k=0;k<=i;k++){
			if(k==j)
				continue;
			if(z[k]->i==l||z[k]->i==r){
				flag=2;
				printf("flag==2");
				break;
			}
		}
		if(flag==2){
			f=(Huffman *)malloc(sizeof(Huffman));
			f->Lchild=NULL;
			f->Rchild=NULL;
			f->Lchild=z[k];
			f->Rchild=z[j];
			f->i=f->Lchild->i+f->Rchild->i;
			t=z[k]=f;
			flag=0;
		}
		else{
			if(flag==1){
				f=(Huffman *)malloc(sizeof(Huffman));
				f->Lchild=NULL;
				f->Rchild=NULL;
				f->Lchild=p;
				f->Rchild=z[j];
				f->i=f->Lchild->i+f->Rchild->i;
				t=p=f;
				flag=0;
			}
			else{
				if(s->i==l||s->i==r){
					p=(Huffman *)malloc(sizeof(Huffman));
					p->Rchild=NULL;
					printf("%d",s->i);
					p->Lchild=s;
					p->Rchild=(Huffman *)malloc(sizeof(Huffman));
					p->Rchild->Lchild=p->Rchild->Rchild=NULL;
					if(s->i==l)
						p->Rchild->i=r;
					else
						p->Rchild->i=l;
					p->i=p->Lchild->i+p->Rchild->i;
					t=s=p;
				}
				else{
					i++;
					R=(Huffman *)malloc(sizeof(Huffman));
					L=(Huffman *)malloc(sizeof(Huffman));
					z[i]=(Huffman *)malloc(sizeof(Huffman));
					R->Lchild=R->Rchild=L->Lchild=L->Rchild=z[i]->Lchild=z[i]->Rchild=NULL;
					L->i=l;
					R->i=r;
					z[i]->Lchild=L;
					z[i]->Rchild=R;
					z[i]->i=z[i]->Lchild->i+z[i]->Rchild->i;
					t=z[i];
				}
			}
		}
		Insert(t->i);
		if(head->next->next!=NULL){
			l=min(head);
			Delete(head,l);
			r=min(head);
			Delete(head,r);
		}
		else{
			break;
		}
	}
	return p;
}
void xianxu(Huffman *p){
	if(p==NULL)
		return ;
	else{
		printf("%d\t",p->i);
		xianxu(p->Lchild);
		xianxu(p->Rchild);
	}
}
void main(){
	Huffman *p;
	p=InitHuffman();
	xianxu(p);
}

```