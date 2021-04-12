---
title: "AVL平衡树"
date: 2020-03-05T13:48:55+08:00

lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["平衡二叉树","数据结构"]
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
豁,这个就有意思了,这个是平衡二叉树,是在二叉排序树的基础上保持他很胖(可以想象越吃的多,越保持身体各方面维生素的均衡).能够增加查找的效率,但建立比较耗时.

因为严蔚敏老师那本教材没有源码,所以有了写出来的想法,本人有点愚钝,写的很慢,但过程很有意思,当然B和B+树和红黑树更难,但我只写出了这个代码,不深究上述树是因为我太菜了(I'm too vegetable!)
话不多说,介绍这个过程:

<figure>
    <img src="/images/AVL1.jpg"/>
</figure>

平衡二叉树定义:它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
二叉查找树退化成链表的问题，把插入，查找，删除的时间复杂度最好情况和最坏情况都维持在O(logN)。但是频繁旋转会使插入和删除牺牲掉O(logN)左右的时间，不过相对二叉查找树来说，时间上稳定了很多。

平衡二叉树不平衡的情形：

把需要重新平衡的结点叫做α，由于任意两个结点最多只有两个儿子，因此高度不平衡时，α结点的两颗子树的高度相差2.容易看出，这种不平衡可能出现在下面4中情况中：

1.对α的左儿子的左子树进行一次插入

2.对α的左儿子的右子树进行一次插入

3.对α的右儿子的左子树进行一次插入

4.对α的右儿子的右子树进行一次插入


<figure>
    <img src="/images/AVL2.jpg"/>
</figure>

出现这种情况,就需要对这个插入的节点进行左右旋、右左旋、左左旋、右右旋来使得满足平衡二叉树的定义。

左左旋：
<figure>
    <img src="/images/L1.jpg"/>
</figure>

右左：

<figure>
    <img src="/images/L2.jpg"/>
</figure>


``` C
#include<stdio.h>
#include<malloc.h>
#include<string.h>
#define MAX 10
typedef struct BtreeNode{
	char data[MAX];
	int i;
	struct BtreeNode *Lchild,*Rchild;
}BN;
int bijiao(char a[MAX],char b[MAX]){		//新定义一个函数，有字符串长度的比较
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
void biaoshi(BN *p){						//打印出这个二叉树的平衡因子
	if(p==NULL)
		return;
	else{
		biaoshi(p->Lchild);
		printf("\t%d",p->i);
		biaoshi(p->Rchild);
	}
}
int deep(BN *p){							//求该结点的深度
	if(p==NULL)
		return 0;
	else{
		int i,j;
		i=deep(p->Lchild);
		j=deep(p->Rchild);
		if(i>j)
			return (i+1);
		else
			return (j+1);
	}
}
void pingheng(BN *p){						//使每个结点的元素p->i赋当前结构体的值
	if(p==NULL){
		return ;
	}
	else{
		int i,j;
		pingheng(p->Lchild);
		pingheng(p->Rchild);
		i=deep(p->Lchild);
		j=deep(p->Rchild);
		p->i=i-j;
	}
}
BN *xunzhao(BN *p,int *i,int *c){			//寻找离插入结点最近的危机结点
	if(p==NULL)
		return p;
	else{
		BN *q;
		q=NULL;
		if(*c!=1)
		q=xunzhao(p->Lchild,i,c);
		if(*c!=1)
		q=xunzhao(p->Rchild,i,c);
		if(*c!=1){
			if(p->i==2&&p->Lchild->i==1){
				*i=1;
				*c=1;
				return p;
			}
			if(p->i==2&&p->Lchild->i==-1){
				*i=3;
				*c=1;
				return p;
			}
			if(p->i==-2&&p->Rchild->i==-1){
				*i=2;
				*c=1;
				return p;
			}
			if(p->i==-2&&p->Rchild->i==1){
				*i=4;
				*c=1;
				return p;
			}
		}
		if(q==NULL)
			return p;
		else
			return q;
	}
}
BN *pinghenghua(BN *p,int i){					//在生成危机结点之后，将这个位置平衡化。
	BN *s,*l;
	BN *m;
	BN *a,*j,*v;
	int b;
	int k;
	int g;
	int flag;
	g=0;
	a=j=p;
	k=0;
	flag=-1;
	m=xunzhao(p,&k,&g);							//m为找到的危机结点，如果没有危机结点就会以k返回一个0值
	while(a!=NULL&&b!=0){
		b=bijiao(a->data,m->data);
		if(b>0)
		{j=a;a=a->Lchild;flag=1;}
		if(b<0)
		{j=a;a=a->Rchild;flag=2;}
	}
	if(k==1){
		s=m->Lchild;
		m->Lchild=s->Rchild;
		s->Rchild=m;
		if(j==m)
			return s;
		if(flag==1)
			j->Lchild=s;
		if(flag==2)
			j->Rchild=s;
		return p;
	}
	if(k==2){
		s=m->Rchild;
		m->Rchild=s->Lchild;
		s->Lchild=m;
		if(j==m)
			return s;
		if(flag==1)
			j->Lchild=s;
		if(flag==2)
			j->Rchild=s;
		return p;
	}
	if(k==3){
		s=m->Lchild;
		l=s->Rchild;
		s->Rchild=l->Lchild;
		m->Lchild=l->Rchild;
		l->Lchild=s;
		l->Rchild=m;
		if(j==m)
			return l;
		if(flag==1)
			j->Lchild=l;
		if(flag==2)
			j->Rchild=l;
		return p;
	}
	if(k==4){
		s=m->Rchild;
		l=s->Lchild;
		s->Lchild=l->Rchild;
		m->Rchild=l->Lchild;
		l->Rchild=s;
		l->Lchild=m;
		if(j==m)
			return l;
		if(flag==1)
			j->Lchild=l;
		if(flag==2)
			j->Rchild=l;
		return p;
	}
	return p;
}
BN *InsertBtree(BN *p,char key[MAX]){
	BN *q,*s,*l,*n,*t,*o;
	o=t=n=q=s=p;
	int m;
	int i=0;
	while(s!=NULL){
		m=bijiao(s->data,key);
		if(m>0)
		{q=s;s=s->Lchild;}
		if(m<0){q=s;s=s->Rchild;}
		if(m==0){
			q=s;s=s->Lchild;
		}
		i++;
	}
	l=(BN *)malloc(sizeof(BN));
	l->Lchild=NULL;
	l->Rchild=NULL;
	strcpy(l->data,key);
	if(m>0){
		q->Lchild=l;
	}
	else{
		q->Rchild=l;
	}
	pingheng(p);
	p=pinghenghua(p,i);
	pingheng(p);
	return p;
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
			p->Lchild=NULL;
			p->Rchild=NULL;
			head=p;
			strcpy(p->data,x);
			flag=1;
			pingheng(head);
		}
		else{
			head=InsertBtree(head,x);
		}
		scanf("%s",&x);
	}
	return head;
}
void show(BN *p){
	if(p==NULL)
		return ;
	else{
		show(p->Lchild);
		printf("%s\t",p->data);
		show(p->Rchild);
	}
}
BN *Dxunzhao(BN *p,int *i,int *c){			//寻找离插入结点最近的危机结点
	if(p==NULL)
		return p;
	else{
		BN *q;
		q=NULL;
		if(*c!=1)
		q=Dxunzhao(p->Lchild,i,c);
		if(*c!=1)
		q=Dxunzhao(p->Rchild,i,c);
		if(*c!=1){
			if((p->i==2&&p->Lchild->i==1)){
				*i=1;
				*c=1;
				return p;
			}
			if((p->i==2&&p->Lchild->i==-1)||(p->i==2&&p->Lchild->i==0)){
				*i=3;
				*c=1;
				return p;
			}
			if(p->i==-2&&p->Rchild->i==-1){
				*i=2;
				*c=1;
				return p;
			}
			if((p->i==-2&&p->Rchild->i==1)||(p->i==-2&&p->Rchild->i==0)){
				*i=4;
				*c=1;
				return p;
			}
		}
		if(q==NULL)
			return p;
		else
			return q;
	}
}
BN *Dpinghenghua(BN *p,int i){
	BN *s,*l;
	BN *m;
	BN *a,*j,*v;
	int b;
	int k;
	int g;
	int flag;
	g=0;
	a=j=p;
	k=0;
	flag=-1;
	m=Dxunzhao(p,&k,&g);
	while(a!=NULL&&b!=0){
		b=bijiao(a->data,m->data);
		if(b>0)
		{j=a;a=a->Lchild;flag=1;}
		if(b<0)
		{j=a;a=a->Rchild;flag=2;}
	}
	if(k==1){
		s=m->Lchild;
		m->Lchild=s->Rchild;
		s->Rchild=m;
		if(j==m)
			return s;
		if(flag==1)
			j->Lchild=s;
		if(flag==2)
			j->Rchild=s;
		return p;
	}
	if(k==2){
		s=m->Rchild;
		m->Rchild=s->Lchild;
		s->Lchild=m;
		if(j==m)
			return s;
		if(flag==1)
			j->Lchild=s;
		if(flag==2)
			j->Rchild=s;
		return p;
	}
	if(k==3){
		s=m->Lchild;
		l=s->Rchild;
		s->Rchild=l->Lchild;
		m->Lchild=l->Rchild;
		l->Lchild=s;
		l->Rchild=m;
		if(j==m)
			return l;
		if(flag==1)
			j->Lchild=l;
		if(flag==2)
			j->Rchild=l;
		return p;
	}
	if(k==4){
		s=m->Rchild;
		l=s->Lchild;
		s->Lchild=l->Rchild;
		m->Rchild=l->Lchild;
		l->Rchild=s;
		l->Lchild=m;
		if(j==m)
			return l;
		if(flag==1)
			j->Lchild=l;
		if(flag==2)
			j->Rchild=l;
		return p;
	}
	return p;
}
BN *zuomax(BN *p){
	BN *q,*g,*s,*t;
	char x[MAX];
	int i=0;
	q=s=p->Lchild;
	while(s!=NULL){
		q=s;s=s->Rchild;
	}
	if(q->Lchild!=NULL){
		strcpy(x,q->Lchild->data);
		strcpy(q->Lchild->data,q->data);
		strcpy(q->data,x);
		q=q->Lchild;
	}
	strcpy(p->data,q->data);
	g=p;
	s=p->Lchild;
	while(s!=q){
		if(s->Rchild!=NULL){
			g=s;s=s->Rchild;}
		else{
		g=s;s=s->Lchild;
		}
	}
	if(g->Lchild==q)
		g->Lchild=NULL;
	else
		g->Rchild=NULL;
	return p;
}
BN *youmin(BN *p){
	BN *q,*g,*s;
	char x[MAX];
	q=s=p->Rchild;
	while(s!=NULL){
		q=s;s=s->Lchild;
	}
	if(q->Rchild!=NULL){
		strcpy(x,q->Rchild->data);
		strcpy(q->Rchild->data,q->data);
		strcpy(q->data,x);
		q=q->Rchild;
	}
	strcpy(p->data,q->data);
	g=p;
	s=p->Rchild;
	while(s!=q){
		if(s->Lchild!=NULL){
			g=s;s=s->Lchild;
		}
		else{
			g=s;s=s->Lchild;
		}
	}
	if(g->Lchild==q)
		g->Lchild=NULL;
	else
		g->Rchild=NULL;
	return p;
}
BN *Delete(BN *p,char x[MAX]){
	BN *q,*s,*z;
	int m;
	int i=0;
	q=s=p;
	while(s!=NULL){
		m=bijiao(s->data,x);
		if(m>0)
		{q=s;s=s->Lchild;}
		if(m<0)
		{q=s;s=s->Rchild;}
		if(m==0)
			break;
	}
	if(s!=NULL){
		if(s->Lchild==NULL&&s->Rchild==NULL){
			if(q->Lchild==s)
				q->Lchild=NULL;
			if(q->Rchild==s)
				q->Rchild=NULL;
		}
		else{
			if(s->i==1||s->i==0){
				s=zuomax(s);
			}
			if(s->i==-1){
				s=youmin(s);
			}
		}
	}
	else{
		printf("没有这个结点值");
		return p;
	}
	pingheng(p);
	p=Dpinghenghua(p,i);
	pingheng(p);
	return p;
}
void main(){
	BN *p;
	char x[MAX];
	p=InitBtree();
	printf("\n该平衡树的中序遍历是：");
	show(p);
	printf("\n该平衡树各结点的平衡因子为");
	biaoshi(p);
	while(1){
	scanf("%s",&x);
	p=Delete(p,x);
	printf("\n该平衡树的中序遍历是：");
	show(p);
	printf("\n该平衡树各结点的平衡因子为");
	biaoshi(p);
	}
}

```


[摘自平衡二叉树详解](https://www.cnblogs.com/zhangbaochong/p/5164994.html)