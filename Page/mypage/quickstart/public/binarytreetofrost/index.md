# 二叉树转森林

二叉树转森林,左子树为孩子节点,右子树为孩子兄弟节点

``` C
#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>
typedef int AdjType;
typedef struct QNode{
	AdjType i;
	AdjType date;
	struct QNode *next;
}Q;
typedef struct Sqlist{
	Q *front;
	Q *rear;
}S;
void Init(S* p){
	p->rear=p->front=(Q *)malloc(sizeof( Q ));
	p->rear->next=NULL;
	printf("\n\t\t\t银行计划初始化成功!");
}
void insert(S *p,int i,int j){
	Q *q;
	printf("\n\t\t\t第%d位客户进入了银行他要用的时间是%d\t",i,j);
	q=(Q *)malloc(sizeof(Q));
	q->date=j;
	p->rear->next=q;	
	p->rear=q;
	q->i=i;
	p->rear->next=NULL;
}
void Delete(S *p,int i){
	Q *q;
	if(p->front==p->rear)
		return;
	q=p->front->next;
	if(q->date==0&&i==1){
		p->front=p->front->next;
		printf("\n\t\t\t第%d客户已经离开了银行\n",q->i);
	}
}
int fanhui(S *p){
	int i;
	Q *q;
	q=p->front;
	i=0;
	if(q==p->rear){
		return 0;
	}
	while(q!=p->rear){
		i++;
		return i;
	}
}
void jianyi(S *p){
	Q *q;
	if(p->front==p->rear)
		return ;
	q=p->front->next;
	if(q->date>0)
		q->date=q->date-1;
}
int shijianhe(S *p){
	int j;
	Q *q;
	j=0;
	q=p->front;
	do{
		q=q->next;
		j=q->date+j;
	}while(q!=p->rear);
	return j;
}
int min(int a,int b){
	if(a>b)
		return b;
	else
		return a;
}
void xianshi(S *p,int i){
	Q *q;
	if(p->front==p->rear)
		return ;
	q=p->front->next;
	printf("\t第%d位客户在%d窗口,数值是%d",q->i,i+1,q->date);
}
void main(){
	S a[4];
	int visited[4]={0};
	int b[4];
	int i,k,l,s;
	int flag;
	int j;
	int x;
	x=1;
	for(k=0;k<=3;k++){
		Init(&a[k]);
	}
	for(i=1;x!=0;i++){
		if(x==1){
			s=31;
			for(k=0;k<4;k++){
				b[k]=0;
			}
			j=rand()%30+1;
			flag=0;
			for(k=0;k<4;k++){
				b[k]=fanhui(&a[k]);
				if(fanhui(&a[k])==0){
					insert(&a[k],i,j);
					visited[k]=1;
					break;
				}
				b[k]=fanhui(&a[k]);
			}
			if(b[1]&&b[2]&&b[0]&&b[3]){
				for(k=0;k<4;k++){
					s=min(shijianhe(&a[k]),s);
				}
				for(k=0;k<4;k++){
					if(s==shijianhe(&a[k])){
						insert(&a[k],i,j);
						visited[k]=1;
						break;
					}
				}
			}
		}
		for(k=0;k<4;k++){
			printf("\n\t\t");
			xianshi(&a[k],k);
		}
		for(k=0;k<4;k++){
			jianyi(&a[k]);
		}
		for(k=0;k<4;k++){
			Delete(&a[k],visited[k]);
		}
		printf("\n\t\t\t请输入控制字0结束，1继续产生随机数，2不产生随机数");
		scanf("%d",&x);
	}
	printf("\n\t\t\t银行下班了");

}
```
