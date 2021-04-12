---
title: "图"
date: 2020-03-05T13:48:55+08:00

lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["图","数据结构"]
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
图!<br>
这个就牛逼了,增删改查.看代码

``` C

#include <stdio.h>
#include <stdlib.h>
#include<malloc.h>	
#define MAX 20                     /* 图的最多顶点数*/
#define INF 65535                  /*定义大于所有权值的一个数*/
typedef enum {DGN,UDGN} GraphKind; /*DGN包括有向图和有向网，UDGN包括无向图和无向网*/
typedef char VertexType;           /* 图的顶点类型 */
typedef int AdjType;            /* 图的边（或弧）的类型，如果是网，则权值非负 */
typedef char zifu;				
typedef int zheng;   
typedef struct list{
	VertexType date;
	struct list *next;
}lqlist;
typedef struct Queuelist{
	zheng date;						/*队列的数据类型*/
	struct Queuelist *next;			
}QNode, *Queue;
typedef struct queuelist{
	Queue front;
	Queue rear;
}S,*PQueue;
struct  
{	VertexType  vex;
	AdjType  lowcost;
}closeedge[MAX];
typedef struct
{	VertexType vexs[MAX+1];      /* 表示的顶点的一维向量 */
	AdjType edges[MAX+1][MAX+1]; 
	/* 表示图的边（或弧）的邻接矩阵，对于无权图用1或0表示顶点是否相邻，对于带权图，则为权值类型 */
	int n,e;                     /* n表示当前顶点数  e表示当前的边数（弧数） */
}MGraph;
              /*为了避免元素的位序与C语言数组的表示之间的错位，不使用C语言中数组的零元素*/

void InitQueue(PQueue p){
	p->front=p->rear=(Queue)malloc(sizeof(QNode));
	p->front->next=NULL;
}
void InsertQueue(PQueue p,zheng i){
	Queue q;
	q=(Queue)malloc(sizeof(QNode));
	q->date=i;
	q->next=NULL;
	if(p->rear==NULL){
		p->front=q;
		p->rear=q;
	}
	else{
		p->rear->next=q;
		p->rear=q;
	}
}
int DeleteQueue(PQueue p){
	QNode *q;
	q=p->front;
	if(p->front==p->rear){
		p->front=NULL;
		p->rear=NULL;
	}
	else{
		p->front=p->front->next;
		free(q);
	}
	return p->front->date;
}
int panduan(PQueue p){
	if(p->front==p->rear){
		return 1;
	}
	else{
		return 0;
	}
}
void CreateGN(MGraph* G,GraphKind gk)                                           /*构造图*/
{	int i,j,k;
	AdjType w;

	printf("\n\t\t请输入图的顶点数和边数(格式为：顶点数，边数)\n\t\t");
	scanf("%d,%d",&(G->n),&(G->e));getchar();                     /*输入图的顶点数和边数*/ 
	for(i=1;i<=G->n;i++)
		for(j=1;j<=G->n;j++)
			G->edges[i][j]=INF;                /*邻接矩阵的初始化,用INF表示边或弧的不存在*/
	printf("\t\t请输入各顶点的值（该值对应的序号为它输入时的次序）：\n\t\t");
	for(i=1;i<=G->n;i++)
	{	scanf("%c",&(G->vexs[i]));getchar();printf("\t\t"); }              /*构造顶点向量*/
	printf("请输入边信息，如果不是网，则用1表示,否则输入权值\n");
	printf("\t\t如果是网，则输入权值!\n");
	printf("\t\t输入格式为：端点1的序号，端点2的序号，1或权值\n\t\t");
	for(k=1;k<=G->e;k++)
	{	scanf("%d,%d,%d",&i,&j,&w); 
	                            /* i和j分别表示两个顶点在一维向量的位置，w表示边（或弧）的值，
	                                  如果两个顶点之间没有边，则权值w为一特定的预先给定的值*/
	    getchar();
		printf("\t\t");
		G->edges[i][j]=w;
		if (gk==2)
			G->edges[j][i]=w;
	}                                                                        /*构造邻接矩阵*/
	printf("\n\t\t输入的各个顶点为： ");
	for(i=1;i<=G->n;i++)
		printf("%c\t",G->vexs[i]);
	printf("\n\t\t输入的邻接矩阵为： \n");
	for(i=1;i<=G->n;i++)
	{	printf("\n\t\t");
		for(j=1;j<=G->n;j++)
			printf("%d\t",G->edges[i][j]);
	}
}

int FirstAdjVertex(MGraph* G,int i)
/*返回图(网)G的第i个顶点的第一个邻接顶点的值*/
{	int j;
	for(j=1;j<=G->n;j++)
		if (j!=i && G->edges[i][j]!=INF) break;
	if (j==G->n+1)
		return 0;
	else
		return j;

}

int NextAdjVertex(MGraph* G,int i,int j)
/*已知图(网)G的第j个顶点是第i个顶点的邻接顶点，求第i个顶点的相对于第j个顶点的下一个邻接顶点*/
{
	
    int k;
	for(k=1;k<=G->n;k++)
	{
		if (k!=i && k!=j && G->edges[i][k]!=INF) 
			break;
	}
	if (k==G->n+1){
			return 0;
		}
		else{
			return k;
		}
}


void DFSM(MGraph* G,int i,int* tag)       
/*采用数组表示法表示的无向图的从第i个顶点开始的深度优先搜索遍历*/
{	int j;
	printf("%c\t",G->vexs[i]);                                   /*采用打印函数访问每个结点*/
	tag[i]=1;
	for(j=1;j<=G->n;j++)
	  	if ((!tag[j]) && G->edges[i][j]!=INF)
			DFSM(G,j,tag);                                                       /*递归调用*/
}

void  BFSM(MGraph* G,int i,int* tag)
/*采用数组表示法表示的无向图的从第i 个顶点开始的广度优先搜索遍历*/
{
	S p;
	int k,j;
	int u;
	InitQueue(&p);
	InsertQueue(&p,i);
	tag[i]=1;
	u=i;
	printf("\n\t\t广度优先遍历搜索的结果是\n\t");
	while(!panduan(&p)){
		u=DeleteQueue(&p); 
		printf("\t%c",G->vexs[u]);
		for(j=1;j<=G->n;j++){
			if(!tag[j]&&G->edges[u][j]!=INF){
				tag[j]=1;
				InsertQueue(&p,j);
			}
		}
	}
}

void Prim(MGraph* G,  int i)
{		/*用Prim方法建立有n个顶点的邻接矩阵存储结构的网gm的最小生成树*/
        /*从序号为1的顶点出发；建立的最小生成树存于数组closevertex中*/
    int mincost;
	int j,k,m;
	int vexnum;
	vexnum=G->n;
	for (j=1;j<=G->n;j++)                                                   /*辅助数组初始化*/
		if (j!=i)
		{	closeedge[j].vex=G->vexs[i];
			closeedge[j].lowcost=G->edges[i][j];
		}
	closeedge[i].lowcost=0;                            /*从序号为i的顶点出发生成最小生成树*/
	vexnum--;
	while(vexnum!=0)                                   /*寻找当前最小权值的边的顶点*/
	{	mincost=INF;                                   /*INF为一个大于所有权值的数*/
		k=1;m=1;
		while (k<=G->n)                                /*求顶点集V-U与顶点集U之间的权值最小边*/
		{	if (closeedge[k].lowcost<mincost && closeedge[k].lowcost!=0)
      		{	mincost= closeedge[k].lowcost;
				m=k;
			}        
       		k++;
		}
		printf("\t\t当前最小边的端点的值为：%c----%c\n",G->vexs[m],closeedge[m].vex);
		closeedge[m].lowcost=0;
		vexnum--;
		for (j=1;j<=G->n;j++)                    /*修改其它顶点的边的权值和最小生成树顶点序号*/
			if (G->edges[m][j]<closeedge[j].lowcost)
			{	
				closeedge[j].vex=G->vexs[m];
				closeedge[j].lowcost=G->edges[m][j];
			}
	}
	//牛逼啊，兄嘚。
}


void ShortestPath_DIJ(MGraph* G, int i, int *P, AdjType *D)
{	/*用Dijkstra算法求有向网G的第i个顶点vi到其余顶点v的最短路径*/
	/* P[v]表示从源点到顶点v的最短路径上顶点v的前驱顶点*/
	/* D[v]表示从源点到顶点v的最短路径的长度*/
	/*常量INF为大于网中所有权值的值*/
	int   j, k,q,min,m,pre;
	int   final[MAX];            /*final[v]为TRUE当且仅当v∈S, ，即已经求得从vi到v的最短路径*/
	for (j=1;j<=G->n;j++)
	{	
		D[j]=G->edges[i][j];
		final[j]=0;    
	    if (D[j]<INF)   P[j]=i;                                  /*最短路径初始化*/
	    else  P[j]=0;
	}
	D[i]=0; final[i]=1;                                         /*初始化，vi顶点属于S集*/
	                           /*开始主循环，每次求得vi到某个v 顶点的最短路径，并加v到集S*/
	for(j=1; j<=G->n; j++)                                      /*其余G.vexnum-1个顶点*/
	{
		min=INF;                                        /*min为当前所知离vi顶点的最近距离*/
		for (k=1; k<=G->n; k++)
			if (!final[k] && D[k]<min)                    /*w顶点在V－S中*/
			{min=D[k];  m=k;  }
			final[m]=1;                                      /*离vi顶点最近的vm加入S集合*/
			for(k=1;k<=G->n;k++)                             /*更新当前最短路径*/
				if ((!final[k]) && ((min+G->edges[m][k])<D[k]))  /* 修改D[k]和P[k],k∈V-S */
				{
					D[k]=min+G->edges[m][k]; 
					P[k]=m;                                   /*顶点m是顶点k的前驱*/
				}
	}
	 printf("\n\t\t");
	 for(j=1;j<=G->n;j++)
	 {	printf("%d\t%d",D[j],j);                           /*打印结果*/
	    pre=P[j];
	    while(pre!=0)                                      /*继续寻找前驱顶点*/
	    {	printf("<---%d",pre);
	     		pre=P[pre];
	    }
		printf("\n\t\t");
	 }
}

main()
{	  MGraph G;
	  GraphKind gk;
	  VertexType a;
	  char choice;
	  int i,j=1,ch2,k;
	  int visited[MAX];                 /*定义数组存储访问标志*/
	  int P[MAX];                       /*记录单源点的最短路径*/
	  AdjType D[MAX];                   /*记录从源点到其余各顶点的最短路径的长度*/
	  while(j)
      {
      printf("\n");
      printf("\n");
      printf("\n");
      printf("\n");
      printf("\n\t\t图的数组表示\n");
      printf("\n\t\t**************************************************");
      printf("\n\t\t*            1--------图 的  初  始 化           *");
      printf("\n\t\t*            2--------求第一个邻接顶点           *");
      printf("\n\t\t*            3--------求第二个邻接顶点           *");
      printf("\n\t\t*            4--------图的深度优先遍历    　     *");
      printf("\n\t\t*            5--------图的广度优先遍历       　  *");
      printf("\n\t\t*            6--------Prim    算    法    　     *");
      printf("\n\t\t*            7--------单源点的最短路径   　      *");
      printf("\n\t\t*            0--------退            出           *");
      printf("\n\t\t**************************************************");
      printf("\n\t\t请选择菜单号: 0-7...");
      scanf("%c",&choice);
      getchar();
		if(choice=='1')
		{
			printf("\t\t请输入图的类型（有向还是无向）\n");
			printf("\t\t如果是有向图或有向网输入1，否则输入2\n\t\t");
			scanf("%d",&gk);getchar();		
			CreateGN(&G,gk);
		}
		else if (choice=='2')
		{	printf("\n\t\t请输入顶点的编号:");
			scanf("%d",&i);getchar();
			
			a=FirstAdjVertex(&G,i);
			if (a==0)
				printf("\t\t没有邻接顶点");
			else
			{	printf("\t\t顶点%c的第一个邻接顶点是: ",G.vexs[i]);
				printf("%c",G.vexs[a]); }
		}
		else if (choice=='3')
		{	
			printf("\n\t\t请输入顶点的编号:");
			scanf("%d",&i);getchar();
			printf("\n\t\t请输入顶点编号为：%d的第一个邻接顶点的编号:",i);
			scanf("%d",&k);getchar();

			a=NextAdjVertex(&G,i,k);
			if (a==0)
				printf("\t\t没有第二个邻接顶点");
			else
			{	printf("\t\t顶点%c的第二个邻接顶点是: ",G.vexs[i]);
				printf("%c",G.vexs[a]); }

		}
		else if (choice=='4')
		{	for(j=1;j<=MAX;j++)
				visited[j]=0;
			printf("\n\t\t请输入深度优先遍历的第一个顶点的编号(1-%d)",G.n);
			scanf("%d",&i);getchar();
			printf("\n\t\t");
			DFSM(&G,i,visited) ;
			printf("\n");
		}
		else if (choice=='5')
		{
			for(j=1;j<=MAX;j++)
				visited[j]=0;
			printf("\n\t\t请输入广度优先遍历搜索的起始顶点");
			scanf("%d",&i);
			getchar();
			BFSM(&G,i,visited);
		}
		else if (choice=='6')
		{	printf("\n\t\t请输入出发顶点的编号：");
			scanf("%d",&i);getchar();
			Prim(&G,i);
		}
		else if (choice=='7')
		{	printf("\n\t\t请输入源点的编号：");
			scanf("%d",&i);getchar();
			ShortestPath_DIJ(&G, i, P, D);
		}
		else if (choice=='0')
		{	j=0;
			printf("\t\t程序结束!\n");}
		else printf("\n\t\t输入错误! 请重新输入!\n");
   }
}


```