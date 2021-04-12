# 排序


各种排序!这些内容还没有复刻,是邱桃荣老师做的.方便以后学习
``` C
#include <stdio.h>
#include <conio.h>
#include <string.h>
#include <malloc.h>
#define Keytype int        /*关键字类型定义*/
#define MAX_LIST_LEN 100   /*定义线性表的最大长度*/
typedef  struct {                    /*定义元素类型 */
	Keytype key;                     /*关键字定义*/
} ElemType;               
typedef struct {                     /*查找表顺序存储结构定义*/
	ElemType elem[MAX_LIST_LEN+1];   /*elem[0]元素当作工作单元*/
	int length;                      /* 查找表长度*/
}Seq_Table;
Seq_Table seqtbl;
typedef struct  NODE{                       /*查找表链式存储结构定义*/
	ElemType elem;                        /*其中ElemType 定义同顺序存储结构*/
	struct  NODE *next;
}LINK_NODE;
#define ENDVALUE  -1
typedef struct BINNODE{                       /*二叉排序树定义*/
	Keytype key;                              /*关键字值*/
	struct BINNODE *lchild ,*rchild;          /*左右指针*/ 
} BSTNode, *BSTree;

/*显示主界面*/
void PrintMenu()
{	    printf("\n\n\n\n\n");
	    printf("\t\t\t    --  各 类 查 找  综 合 演 示  --  \n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------静 态 查 找        *");
		printf("\n\t\t\t*       2-------动 态 查 找        *");
		printf("\n\t\t\t*       0-------退       出        *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t请选择功能号(0--2)：");
}

/* 查找表初始化*/
 void ElemInit()
{
    int i=1;
	ElemType elem;
	printf("\n 请注意! 假定系统查找表的最大长度为 %d" ,MAX_LIST_LEN);
        printf("\n 请输入若干( <%d )查找表初始元素的关键字值(整数),以%d结束.\n",MAX_LIST_LEN,ENDVALUE);
        seqtbl.length=0;    /*初始化查找表长度*/
	while (1)
	{
	scanf("%d",&elem.key);
	if (elem.key==ENDVALUE) 
	   break;
	else
	   seqtbl.elem[i++].key=elem.key;   /*从第1号单元开始存放数据*/
        seqtbl.length++;     
	}
}

/* 输出查找表的所有元素*/
void output(Seq_Table seqtbl1)
{
        int k;
	printf ("\n\t 查找表的地址单元:  ");
        for(k=1;k<=seqtbl1.length ;k++)
		printf("%4d",k);
   	printf ("\n\t 元素关键字序列为:  ");
	for(k=1;k<=seqtbl1.length ;k++)
		printf("%4d",seqtbl1.elem[k]);
}

/*数据输入界面*/
int input()
{
	int x;
	while(1)
	{
	printf("\n 请输入你要查找元素的关键字值(整型):");
        scanf("%d",&x);
	getchar();
	if (!((x>=-32768) && (x<=32767))) 
		printf("\n 关键字输入无效,请重新输入!");
	else
		break;
	}
	return x;
}

 /*显示静态查找主界面*/

void PrintStaticMenu()
{
	        printf("\n\n\n\n\n");
	        printf("\t\t\t     --   静 态 查 找 综 合 演 示   --     \n");
		printf("\n\t\t\t*****************************************");
		printf("\n\t\t\t*       1------- 查找表初始化           *");
		printf("\n\t\t\t*       2------- 顺 序 查  找           *");
        printf("\n\t\t\t*       3------- 顺 序 查  找(设监视哨) *");
		printf("\n\t\t\t*       4------- 折 半 查  找           *");
		printf("\n\t\t\t*       0------- 返 回主界 面           *");
		printf("\n\t\t\t*****************************************\n");
		printf("\t\t\t          请选择功能号(0--4)：");
 }

/*顺序查找 未设置监视哨情形*/
int SeqSearch(Seq_Table  Seq_Tbl, Keytype  Sea_Key)
  /*Seq_Tbl为查找表，Sea_Key 为待查找的关键字.*/
{
	int  pos, len;
	pos=1; len=Seq_Tbl.length; 
	/*当没有到达查找表尾且没有找到时循环;*/
	while (pos<=len  &&  Seq_Tbl.elem[pos].key!=Sea_Key)  pos++; 
	if  (pos>len )    /*循环结束是因为没有找到此元素*/
		return 0;
	else         /*找到此元素 */
		return pos;
}

/*顺序查找 设置监视哨情形*/

int SeqSearchSetMonitor(Seq_Table  Seq_Tbl, Keytype  Sea_Key)
{
	int i;
	Seq_Tbl.elem[0].key=Sea_Key;
	for(i=MAX_LIST_LEN;i!=0&&Seq_Tbl.elem[0].key==-1;i--){
		if(Seq_Tbl.elem[0].key==Sea_Key){
			break;
		}
	}
	return i;
}

/*冒泡排序*/
void BubbleSort( Seq_Table  *Seq_Tbl )  
{	int i,j;
	ElemType temp;
	for ( i=1 ; i<=Seq_Tbl->length-1 ; i++ )    /* 外循环，i 表示趟数，总共需要len-1趟*/
	{	for ( j=1 ; j<=Seq_Tbl->length-i ;j++ )  /*内循环，每趟排序中所需比较的次数*/
		if  ( Seq_Tbl->elem[j].key>Seq_Tbl->elem[j+1].key)  /*前面元素大于后面元素，则交换*/
        {	temp=Seq_Tbl->elem[j] ; Seq_Tbl->elem[j]=Seq_Tbl->elem[j+1]; Seq_Tbl->elem[j+1]=temp;}
	}
}

/*折半查找*/

int  BinarySearch(Seq_Table  Seq_Tbl , Keytype  Sea_Key)
/*Seq_Tbl 为查找表，Sea_Key 为待查找的关键字*/
{
	int mid, low, high;
	low=1; high=Seq_Tbl.length;
	while (low<=high)
	{	mid=(low+high)/2;
		if  (Seq_Tbl.elem[mid].key<Sea_Key)   /*若存在，则必在右半区*/
			low=mid+1;
		else
			if (Seq_Tbl.elem[mid].key>Sea_Key)  /*若存在，则必在左半区*/
				high=mid-1;
			else                             /*等于待查找的关键字，查找结束*/
			break;
	}
	if  (low>high)                         /*没有找到*/
		return 0;
	else 
		return mid;
}

/*静态查找主程序*/
void StaticSearch( ) 
{
	char static_func_choice;
	PrintStaticMenu();
	getchar();     /*读主界面的回车符*/
	static_func_choice=getchar();
	while (static_func_choice!='0')
	{
		switch (static_func_choice)
		{
			case '1':
				ElemInit();   
				break;
			case '2':	/*顺序查找*/
			{
			   	int retval,seakey;
				seakey=input();
				retval=SeqSearch(seqtbl, seakey);
				output(seqtbl);
				printf("\n\t 你要查找的关键字为: %d",seakey);
				if (retval>0) 
					printf("\n\t 查找成功,关键字为 %d 的元素位于第 %d 个位置!",seakey,retval);
				else
					printf("\n\t 对不起,关键字为 %d 的元素不存在.",seakey);
				break;
			}
			case '3' :	/*设置监视哨情形*/
				int retval,seakey;
				seakey=input();
				retval=SeqSearchSetMonitor(seqtbl, seakey);
				output(seqtbl);
				printf("\n\t 你要查找的关键字为: %d",seakey);
				if (retval>0) 
					printf("\n\t 查找成功,关键字为 %d 的元素位于第 %d 个位置!",seakey,retval);
				else
					printf("\n\t 对不起,关键字为 %d 的元素不存在.",seakey);
				break;
			case '4' :    /*折半查找*/ 
		    {
			   	int retval,seakey;
				seakey=input();
				printf("\n 请注意!折半查找要求是有序表,若元素顺序无序,则首先将排序!\n");
				BubbleSort(&seqtbl);  /*调用冒泡排序法使查找表成为有序表*/
				retval=BinarySearch(seqtbl, seakey);
				output(seqtbl);
				printf("\n\t 你要查找的关键字为: %d",seakey);
				if (retval>0) 
					printf("\n\t 查找成功,关键字为 %d 的元素位于第 %d 个位置!",seakey,retval);
				else
					printf("\n\t 对不起,关键字为 %d 的元素不存在.",seakey);
				break;
			} 
			default:
			printf( "\n 请输入正确的操作选项(0-4):");
		}
	getchar();   
	PrintStaticMenu();
	static_func_choice=getchar();
	}
}

/*显示动态查找主界面*/
void PrintDynamicMenu()
{
	        printf("\n\n\n\n\n");
	        printf("\t\t\t      --  动 态 查 找 综 合 演 示  -- \n");
		printf("\n\t\t\t****************************************");
		printf("\n\t\t\t*       1------- 二叉排序树初始化      *");
		printf("\n\t\t\t*       2------- 二叉排序树插入        *");
        printf("\n\t\t\t*       3------- 二叉排序树删除        *");
		printf("\n\t\t\t*       4------- 二叉排序树查找        *");
		printf("\n\t\t\t*       0-------   返回主界面          *");
		printf("\n\t\t\t****************************************\n");
		printf("\t\t\t          请选择功能号(0--4)：");
 }

/*二叉排序树的结点插入*/

 void InsertBST(BSTree  bstree, Keytype Ins_key)
{
	BSTree  s ,p,q;
	q=s=bstree;                             /* s指向二叉排序树的根结点*/
	while (s)                             /*找到要插入的结点位置*/
	{	if (s->key>Ins_key) 
			{	q=s;s=s->lchild; }                   /*在左子树查找*/
		 else
			if (s->key<Ins_key)              /*在右子树查找*/
			{	q=s;s=s->rchild; }
		else                           /*存在关键字等于Ins_key的结点*/
			{	printf("\n插入失败，树中已经存在此关键字的结点。\n");
				return ;
			}
    }
    p=(BSTree)malloc(sizeof(BSTNode));     /*生成一个关键字为Ins_key的结点p*/
		p->key=Ins_key;
		p->lchild=NULL;
    p->rchild=NULL;
	if (q->key>Ins_key) 
		q->lchild=p;                        /*将结点p插入左子树*/
	else
		q->rchild=p;                        /*将结点p插入右子树*/
}
/*二叉排序树的更新建立*/

BSTree  CreateBST()               /*返回二叉排序树的头指针*/
{	Keytype Ins_key; 
	BSTree  p,head=NULL;
	int nodeseq=1;  /*结点序号*/
	printf("\n\t 请输入若干元素结点的关键字值,以 %d 结束!\n",ENDVALUE);
	scanf("%d",&Ins_key) ;                 /*假定关键字类型为整型*/
	while (Ins_key!=ENDVALUE)
	{
		if (nodeseq==1) /*第1个结点单独处理*/
		{	p=(BSTree)malloc(sizeof(BSTNode));     /*生成一个关键字为Ins_key的结点p*/
			p->key=Ins_key;
			p->lchild=NULL;
			p->rchild=NULL;
			head=p;
			nodeseq++;
		}
		else
 			InsertBST(head,Ins_key);  	/*调用插入函数*/
		scanf("%d",&Ins_key);
   }
   getchar();
   return head;
}

/*中序二叉排序树的中序遍历输出*/

void OutPutBintree( BSTree  p)
{
	if (p!=NULL)
	{ 
		OutPutBintree(p->lchild );
		printf("%4d",p->key);
		OutPutBintree(p->rchild );
	}
}

/*二叉排序树中结点的删除*/

BSTree  DelBSTNode(BSTree  bstree, Keytype Del_key)
{
	BSTree  s ,p,q,f;           /*s 指向要删除结点的父结点，p指向要删除的结点*/
	p=bstree; s=NULL;                   /* p指向二叉排序树的根结点*/
	while (p)                             /*查找要删除的结点*/
	{	if (p->key==Del_key)                /*找到，则结束查找过程*/
			break;                   
		else
		{	s=p;                          /*将其父结点保存在s中 */  
			if (p->key<Del_key)           /*往右子树继续查找*/
				p=p->rchild;    
			else                          /*往左子树继续查找*/
				p=p->lchild;    
        }
  }   
  if(p)                             
  {
      if (p->lchild)  /* p有左子树,查找左子树最右下方的结点 */
	  {		f=p; 
			q=p->lchild ;
			while (q->rchild)  /*查找最右下方结点*/
			{	f=q; q=q->rchild ;}
			if (f==p) /*q即为最右下方结点*/
				f->lchild=q->lchild ;  /**/ 
			else
				f->rchild =q->lchild ;  /*将左子树链接到父结点的右子树*/
			p->key=q->key ;
			free(q);
	  }
      else   /*要删除的结点无左子树*/
	  {
			if (s==NULL) /*删除结点为根结点*/
				bstree=p->rchild ; 
	  		else
				if (s->lchild==p)  /*删除结点为父结点的左孩子*/
					s->lchild=p->rchild;
			else
				s->rchild=p->rchild;  
		       
			free (p);
	  }
	}
    else                               /*没有找到要删除的结点*/
    {	printf( "\n\t 关键字为期 %d 的结点不存在!", Del_key); }
	return  bstree;
}

/*二叉排序树查找*/
void SearchBST(BSTree  bstree, Keytype Sea_Key)
{
	BSTree p,q;
	p=q=bstree;
	while(p->key!=Sea_Key){
		printf("\t%d",p->key);
		if(p->key>Sea_Key){
			q=p;
			p=p->lchild;
		}
		else{
			q=p;
			p=p->rchild;
		}
		if(p->lchild==NULL&&p->rchild==NULL)
			break;
	}
	if(p->key==Sea_Key){
		if(q->lchild==p)
			printf("你要查找的元素%d是在%d的左边",Sea_Key,q->key);
		else
			printf("你要查找的元素%d是在%d的右边",Sea_Key,q->key);
	}
	else{
		printf("没有该元素");
	}
}




/*动态查找主程序*/

void DynamicSearch( ) 
{
	Keytype Ins_key,Sea_Key;
	BSTree p,root=NULL;
	char dynamic_func_choice;
	PrintDynamicMenu();
	getchar();     /*读主界面的回车符*/
	dynamic_func_choice=getchar();
	while (dynamic_func_choice!='0')
	{
		switch (dynamic_func_choice)
		{
			case '1':
				root=NULL;
				root=CreateBST(); 
				printf("\n\t当前二叉排序树的中序遍历为:\n");
				OutPutBintree(root);
				break;
			case '2':
				printf("\n\t 请输入要插入元素的关键字:");
				scanf("%d",&Ins_key);
				printf("\n\t插入之前的元素的关键字序列为:\n");
				OutPutBintree(root);
				if (!root)    /*第1个结点*/
				{	p=(BSTree)malloc(sizeof(BSTNode));     /*生成一个关键字为Ins_key的结点p*/
					p->key=Ins_key;
					p->lchild=NULL;
					p->rchild=NULL;
					root=p;
				}
				else
				{
					InsertBST(root,Ins_key);	
				}	
				printf("\n\t插入关键字 %d 之后的元素的关键字序列为:\n",Ins_key);
				OutPutBintree(root);
				break;
			case '3' :
 			{	Keytype Del_key;
				printf("\n\t请输入要删除元素的关键字: ");
				scanf("%d",&Del_key);
				printf(" \n\t删除之前元素关键字中序遍历序列为:\n");
				printf("\t");
				OutPutBintree(root);
				root=DelBSTNode(root, Del_key);
				printf(" \n\t删除之后元素关键字中序遍历序列为:\n");
				printf("\t");
				OutPutBintree(root);
  				break;
			}
			case '4' :
			{
				Keytype key;
				scanf("%d",&key);
				SearchBST(root,key);
				break;
				
				}
			default:
				printf( "\n 请输入正确的操作选项(0-4):");
	}
	getchar();   
	PrintDynamicMenu();
	dynamic_func_choice=getchar();
}

}
/*主函数*/
void main()
{	char func_choice;
    PrintMenu();
	func_choice=getchar();
	while (func_choice!='0')
	{
		switch (func_choice)
		{
		case '1' :
			StaticSearch( ) ;
			break;
		case '2' :
			DynamicSearch();
			break;
		case '0' :
			func_choice='0';   break;
		default:
			printf( "\n 请输入正确的操作选项(0-4):");
		}
    PrintMenu();
	getchar();
	func_choice=getchar();
	}
 }
```
