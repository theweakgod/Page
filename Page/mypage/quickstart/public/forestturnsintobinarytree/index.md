# 森林转成二叉树

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
