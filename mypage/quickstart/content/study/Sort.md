---
title: "排序"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include <stdio.h>
#include <conio.h>
#include <string.h>
#include <malloc.h>
#define Keytype int
typedef  struct {								 /* 定义元素类型 */
  Keytype key;									 /* 关键字定义 */
} ElemType;                
#define MAXELEMCOUNT 100						 /*定义元素最多元素个数*/
#define ENDVALUE     -1							 /*定义输入元素的结束值,假定关键字值不可能取-1 */
ElemType  elemarr[MAXELEMCOUNT];
int elemcount=0   ;                               /*实际元素个数*/
#define SHELL_ADD_COUNT 6                         /*希尔排序的增量值个数*/
int shell_add_arr[SHELL_ADD_COUNT],shell_add_len; /*增量数组与长度*/
/*显示主界面*/
 void PrintMenu()
 {
	    printf("\n\n\n\n\n");
	    printf("\t\t\t   --   各 类 排 序 综 合 演 示  --     \n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------插 入 类 排 序     *");
		printf("\n\t\t\t*       2-------交 换 类 排 序     *");
		printf("\n\t\t\t*       3-------选 择 类 排 序     *");
		printf("\n\t\t\t*       4-------归  并  排  序     *");
		printf("\n\t\t\t*       0-------退          出     *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t          请选择功能号(0--4)：");
 }

/* 待排序元素初始化*/
 void ElemInit()
{
    int i=1;
	ElemType elem;
	printf("\n\t 请注意! 系统假定的元素个数最多为 %d" ,MAXELEMCOUNT);
    printf("\n\t 请输入若干待排序元素的关键字值(整数),以%d结束.\n",ENDVALUE);
	elemcount=0;
	while (1)
	{
	scanf("%d",&elem.key);
	if (elem.key==ENDVALUE) 
	   break;
	else
	   elemarr[i++]=elem;   /*从第1号单元开始存放数据*/
    elemcount++;            /*数组长度*/
	}
}



/* 输出第i趟排序后数组元素*/
void output(ElemType Elem_arr[], int len ,int i)
/* len 为数组长度,i为趟数,i为0表示初始关键字序列 */
{
   int k=1;
   if (i==0 )
		printf("\n\t\t 关键字初始序列为:\n");
   else
		printf("\n\t\t 第 %d 趟排序结果为:\n",i);
   printf("\t\t");
   for (k=1;k<=len ;k++)
   {	printf (" %4d ",Elem_arr[k]); }
}


 /*显示插入类排序主界面*/

void PrintInsertMenu()
 {
	    printf("\n\n\n\n\n");
	    printf("\t\t\t--    插  入 类 排 序 综 合 演 示-- \n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------排序数据初始化     *");
		printf("\n\t\t\t*       2------- 直接插入排序      *");
		printf("\n\t\t\t*       3------- 折半插入排序      *");
		printf("\n\t\t\t*       4------- 希 尔 排 序       *");
		printf("\n\t\t\t*       0-------  返回主界面       *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t请选择功能号(0--4)：");
 }
/*直接插入排序*/
void DirectInsertSort (ElemType Elem_Arr[ ],int len) /* len为数组长度，即待排序的元素个数*/
{	int i, endp;
	for (i=2 ; i<=len ;i++)                   /*从第2个元素开始进行插入排序*/
	{		
		Elem_Arr[0]=Elem_Arr[i];            /*设置监视哨*/
		endp=i-1;      /*从当前已排好序的最后一个元素开始确定待插入元素的位置*/
		while ( Elem_Arr[0].key < Elem_Arr[endp].key )  /*确定第i个元素待插入的位置*/
		{  Elem_Arr[endp+1]=Elem_Arr[endp];   /*将元素后移一个位置*/
		   endp--;
		}   /*循环结束后，待插入元素的位置为 endp+1所指示的位置*/
		Elem_Arr[endp+1]=Elem_Arr[0];  /*将第i个元素放入待插入位置*/
		output(Elem_Arr,len,i-1);  /* 输出第i趟排序后数组元素*/
	}
}

void BinInsSort (ElemType Elem_Arr[ ],int len) /*折半插入排序*/
/* len为数组长度，即待排序的元素个数，元素从Elem_Arr[1]中开始存放*/
{	
	int i,j,k,m,x;
	int *p,*q,*t;
	i=1;
	k=len;
	j=(i+k)/2;
	p=Elem_Arr+i,q=Elem_Arr+k;
	printf("\n\t\t\t请输入你要输入的数据");
	scanf("%d",&x);
	if(Elem_Arr[1].key>=x){
		for(m=len;m>=1;m--){
				Elem_Arr[m+1]=Elem_Arr[m];
			}
			Elem_Arr[1].key=x;
			elemcount++;
			output(Elem_Arr,elemcount,0);  
			return;
	}
	if(Elem_Arr[len].key<=x){
		Elem_Arr[len+1].key=x;
		elemcount++;
		output(Elem_Arr,elemcount,0);  
		return;
	}
	for(1;p!=q;){
		p=Elem_Arr+i,q=Elem_Arr+k,t=Elem_Arr+j;
		if((*(t+1)>x)&&(*t<=x)){
			for(m=len;m>j;m--){
				Elem_Arr[m+1]=Elem_Arr[m];
			}
			Elem_Arr[j+1].key=x;
			elemcount++;
			output(Elem_Arr,elemcount,0);  
			return;
		}
		if(x>*t){
			i=j;
			j=(i+k)/2;
			continue ;
		}
		if(x<*t){
			k=j;
			j=(i+k)/2;
			continue ;
		}
	
		if(*t==x){
			j++;
		}

	}
}
/*希尔排序*/
/*一趟希尔排序*/
/*初始化希尔增量数组*/
void ShellAddArrInit()
{
	int i=1,addvar;
	printf("\n 请输入希尔排序数组的增量序列,以 %d 结束!",ENDVALUE);
	shell_add_len=0;
	while (1)
    {	scanf("%d",&addvar);
		if (addvar==ENDVALUE) 
			break;
		else
			shell_add_arr[i++]=addvar;
		shell_add_len++;
	}
 }
void ShellInsSort (ElemType Elem_Arr[ ],int len,int add) 
/* 对元素数组按增量add进行一趟希尔排序，len为数组长度，即待排序的元素个数，元素从*/
/* Elem_Arr[1]中开始存放*/
{	int i,j;
	for ( i=add+1 ; i<=len ; i++ )     /* i的初值为第一个子序列的第二个元素的位置*/
	{	Elem_Arr[0]=Elem_Arr[i];   /*将当前元素存放在Elem_Arr[0]中*/
		for (j=i-add ; j>0 && Elem_Arr[j].key>Elem_Arr[0].key ; j-=add)
		{	Elem_Arr[j+add]=Elem_Arr[j];  /*跳跃式前移*/
		}   /*经过此循后，j+add位置为元素Elem_Arr[i]待插入的位置*/
		Elem_Arr[j+add]=Elem_Arr[0];
  }
}
/* 所有元素进行希尔排序*/
void ShellAllSort (ElemType Elem_Arr[ ],int len,int add[ ],int add_len) 
/*add_len 为增量数组的长度*/
{	int k;
	for (k=1;k<=add_len;k++)
	{	ShellInsSort(Elem_Arr,len,add[k]);
		output(Elem_Arr, len ,k) ; /*输出第k趟排序结果*/
	}
}


/*插入类排序演示*/

void InsertSort( ) 
{
	char ins_func_choice;
	PrintInsertMenu();
	getchar();     /*读主界面的回车符*/
	ins_func_choice=getchar();
	while (ins_func_choice!='0')
	{
		switch (ins_func_choice)
		{
			case '1':
				ElemInit();   
				output(elemarr, elemcount ,0);   
				break;
			case '2' :	/*直接插入类排序*/
				DirectInsertSort(elemarr,elemcount);
				 break;
			case '3' :/*折半插入类排序*/
				BinInsSort(elemarr,elemcount);
				break;
			case '4' :	/*希尔排序*/ 	
				ShellAddArrInit();   /*调用增量初始化函数对增量数组进行初始化*/
				getchar();
				ShellAllSort (elemarr,elemcount, shell_add_arr, shell_add_len);
  				break;
	  
			case '0' :
				ins_func_choice='0';   break;
			default:
				printf( "\n 请输入正确的操作选项(0-3):");
		}
    getchar();   
    PrintInsertMenu();
	ins_func_choice=getchar();
	}
}
/*显示交换类排序主界面*/
void PrintSwapMenu()
 {
	    printf("\n\n\n\n\n");
	    printf("\t\t\t--    交  换 类 排 序 综 合 演 示  --\n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------排序数据初始化     *");
		printf("\n\t\t\t*       2-------冒  泡  排  序     *");
        printf("\n\t\t\t*       3-------冒泡排序(改进型)   *");
		printf("\n\t\t\t*       4-------快  速  排  序     *");
		printf("\n\t\t\t*       0-------返 回 主 界 面     *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t          请选择功能号(0--4)：");
 }


/*冒泡排序*/
void BubbleSort( ElemType Elem_Arr[ ], int len)  /* len为数组长度 */
{	int i,j;
	ElemType temp;
	for ( i=1 ; i<=len-1 ; i++ )    /* 外循环，i 表示趟数，总共需要len-1趟*/
	{	for ( j=1 ; j<=len-i ;j++ )  /*内循环，每趟排序中所需比较的次数*/
		if  ( Elem_Arr[j].key>Elem_Arr[j+1].key)  /*前面元素大于后面元素，则交换*/
        {	}
		output(Elem_Arr, len ,i); 
	}
}

/*冒泡排序改进型*/
void BubbleSortChg( ElemType Elem_Arr[ ], int len)  /* len为数组长度 */
{
	int i;
	int j;
	Elem_Arr[0].key=-1;
	for(i=1;i<=len;i++){
		for(j=1;j<=len-i;j++){
			if(Elem_Arr[j].key>Elem_Arr[j+1].key){
			Elem_Arr[0]=Elem_Arr[j] ; Elem_Arr[j]=Elem_Arr[j+1]; Elem_Arr[j+1]=Elem_Arr[0];
			}
		}
		output(Elem_Arr,len,i);
	}
}


/*快速排序*/
int quickpasscount=1;
int QuickSortPass(ElemType Elem_Arr[ ], int low, int high)
/*对元素数组Elem_Arr中从low 位置开始到 high部分的元素进行一趟快速排序。*/
/*返回值为枢轴（基准点）位置。*/
{	int i=low,j=high;
	ElemType base_var;
	base_var=Elem_Arr[low];
	while (i<j)
	{	while (Elem_Arr[j].key>=base_var.key  && j>i) j--;
		/*从右向左扫描，确定关键字小于base_var.key的元素的位置。*/
		if (i<j)  Elem_Arr[i++]=Elem_Arr[j];  /*元素移动*/
			while (Elem_Arr[i].key<=base_var.key  && j>i) i++;
		if (i<j)  Elem_Arr[j--]=Elem_Arr[i];  /*元素移动*/
	}
	Elem_Arr[i]=base_var ;
	output(Elem_Arr,elemcount,quickpasscount++); 
	return i;
}
/*对所有元素进行快速排序*/
void QuickSort(ElemType Elem_Arr[ ], int low, int high)
{	int base_point;
	if (low<high)
	{	base_point=QuickSortPass(Elem_Arr,low,high);
		QuickSort(Elem_Arr,low,base_point-1);  /*对左子序列进行递归快速排序*/
		QuickSort(Elem_Arr,base_point+1,high); /*对右子序列进行递归快速排序*/
	}
}

/*交换类排序主程序*/
void SwapSort( ) 
{
	char swap_func_choice;
	PrintSwapMenu();
	getchar();     /*读主界面的回车符*/
	swap_func_choice=getchar();
	while (swap_func_choice!='0')
	{
		 switch (swap_func_choice)
		 {
			case '1':
				ElemInit();   
				output(elemarr, elemcount ,0);   
				break;
			case '2' :	/*冒泡排序*/
			    BubbleSort(elemarr, elemcount) ; /* len为数组长度 */
				break;
			case '3' :	/*冒泡排序改进型*/
				BubbleSortChg(elemarr, elemcount);
				break;
			case '4' :/*快速排序*/    
				if (quickpasscount>1) quickpasscount=1;
					QuickSort(elemarr,1, elemcount);
				break;	  
			case '0' :
				swap_func_choice='0';   break;
			default:
				printf( "\n 请输入正确的操作选项(0-4):");
		}
    getchar();   
    PrintSwapMenu();
	swap_func_choice=getchar();
	}
}
/*显示选择类排序主界面*/
void PrintChooseMenu()
 {
	    printf("\n\n\n\n\n");
	    printf("\t\t\t--    选  择 类 排 序 综 合 演 示-- \n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------排序数据初始化     *");
		printf("\n\t\t\t*       2------- 简单选择排序      *");
		printf("\n\t\t\t*       3-------堆    排    序     *");
		printf("\n\t\t\t*       0-------返 回 主 界 面     *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t          请选择功能号(0--3)：");
 }

/*简单选择排序*/

void SingleSelectSort(ElemType Elem_Arr[ ],int len)
/* 数组中元素从Elem_Arr[1]开始存放 */
{	
	int i,j,k;
	ElemType tump,flag;
	for(i=1;i<=len-1;i++){
		k=i;
		tump.key=Elem_Arr[i].key;
		for(j=i+1;j<=len;j++){
			if(Elem_Arr[j].key<tump.key){
				k=j;
				tump.key=Elem_Arr[j].key;
			}
		}
		Elem_Arr[k].key=Elem_Arr[i].key;
		Elem_Arr[i].key=tump.key;
		output(elemarr, elemcount ,i);  
	}
}

/*堆排序 */
/*筛选算法*/
void sift(ElemType Elem_Arr[ ],int k,int l)
/* Elem_Arr[k..l]中除Elem_Arr[k]以外的所有元素的关键字满足大顶堆，本函数的功能是调整*/
/* Elem_Arr[k]，使整个元素序列成为大顶堆。*/
{	int i; 
	ElemType cur=Elem_Arr[k]; /*保存根结点元素*/
	for (i=2*k ; i<=l ; i*=2)  /*以第k个结点作为根结点*/
	{	if (i<l && Elem_Arr[i].key < Elem_Arr[i+1].key) 
		/*右子树存在，且右子树大于左子树，继续沿右子树“筛选”*/
		i=i+1;  /* i为左右子树中关键字较大元素的位置*/
		if (cur.key>=Elem_Arr[i].key)  
			break ;          /*已满足堆树定义，结束*/
		else
		{	Elem_Arr[k]=Elem_Arr[i];
			k=i;           /* k为变量cur要放入的位置*/
		}
   }
   Elem_Arr[k]=cur;  /*将元素Elem_Arr[k] 放入适当位置*/
}

/*堆排序算法*/

void HeapSort(ElemType Elem_Arr[ ],int len)
{	int  n, i ;
	ElemType temp;
	for (i=len/2 ; i>=1; i--)      /*建堆，从第n/2个元素开始向上层进行调整*/
		sift(Elem_Arr,i,len);
	n=len;
	for(i=n ; i>=2 ; i--)           /*调整堆，进行n-1次*/
	{	temp=Elem_Arr[1];            /*将堆顶与堆底交换*/
		Elem_Arr[1]=Elem_Arr[i];
		Elem_Arr[i]=temp;
		output(Elem_Arr,len,n-i+1); 
		sift(Elem_Arr,1,i-1);       /*将剩余的元素重新调整为堆*/
	}
}
/*选择类排序主程序*/
void ChooseSort( ) 
{
	char choose_func_choice;
	PrintChooseMenu();
	getchar();     /*读主界面的回车符*/
	choose_func_choice=getchar();
	while (choose_func_choice!='0')
	{
		switch (choose_func_choice)
		{
			case '1':
				ElemInit();   
				output(elemarr, elemcount ,0);   
				break;
			case '2' :	/*简单选择排序*/
				SingleSelectSort(elemarr, elemcount);
				break;
			case '3' :/*堆排序*/
			    HeapSort(elemarr,elemcount);
				break;
			case '0' :
				choose_func_choice='0';   break;
			default:
				printf( "\n 请输入正确的操作选项(0-3):");
		}
    getchar();   
    PrintChooseMenu();
	choose_func_choice=getchar();
	}
}


/*显示归并排序主界面*/
void PrintMergeMenu()
{
	    printf("\n\n\n\n\n");
	    printf("\t\t\t--    归  并 排 序 综 合 演 示-- \n");
		printf("\n\t\t\t************************************");
		printf("\n\t\t\t*       1-------排序数据初始化     *");
		printf("\n\t\t\t*       2-------归  并  排  序     *");
		printf("\n\t\t\t*       0-------返 回 主 界 面     *");
		printf("\n\t\t\t************************************\n");
		printf("\t\t\t请选择功能号(0--2)：");
 }

void Merge(ElemType  Src[ ], int low, int mid ,int high )
/* 已知Src[low‥mid]和Src[mid+1‥high]为两个有序子序列，本算法将它们合并成为一个新的有序列，并存入Src[low‥high]中 */
{	int i=low, j=mid+1,pos=0,k;
	 ElemType *base;
	 base=(ElemType *)malloc(sizeof(ElemType)*(high-low+1)) ; /*申请临时空间*/
	while (i<=mid && j<=high )   /* 当两个有序子表都没有结束时 */
		*(base+pos++)=(Src[i].key<=Src[j].key )? Src[i++]:Src[j++] ;
	while (i<=mid)        /*第1个有序子表未结束，将第1个有序子表剩余元素放入Dest中 */
		*(base+pos++)=Src[i++];
	while (j<=high)        /*第2个有序子表未结束，将第2个有序子表剩余元素放入Dest中 */
		*(base+pos++)=Src[j++];
	for (k=0,i=low;k<pos;k++,i++)
		Src[i]=*(base+k);
	free(base);
}
void MergeOnce(ElemType  Src[ ] , int LENGTH, int len ) 
/*LENGTH 为整个待排元素序列的长度，len 为该趟归并排序的有序子表长度，经过此算法，
将存放在Src[ ]中?LENGTH / len?  个长度为len 的有序子表合并成为?LENGTH / 2len?个长度为2len（最后一个可能例外）的有序子表 ,存放在Src[ ]中*/
{	int i;
	for (i=1 ; i+2*len-1<=LENGTH ;i=i+2*len)
		Merge(Src,i,i+len-1,i+2*len-1);
	if (i+len-1<LENGTH)
		Merge(Src,i,i+len-1,LENGTH);
}

void  MergeSort(ElemType  Src[ ] , int LENGTH)
{	int i;
	printf("lll"); 
	for (i=1; i<LENGTH;i*=2)
	{	MergeOnce(Src , LENGTH, i);
		output(Src,LENGTH,i);
	}
}
/*归并类排序主程序*/
void MergeSortMain( ) 
{
	char merge_func_choice;
	PrintMergeMenu();
	getchar();     /*读主界面的回车符*/
	merge_func_choice=getchar();
	while (merge_func_choice!='0')
	{
		switch (merge_func_choice)
		{
			case '1':
				ElemInit();   
				output(elemarr, elemcount ,0);   
				break;
			case '2' : /*归并排序*/
				MergeSort(elemarr,elemcount);
				break;
			case '0' :
				merge_func_choice='0';   break;
			default:
				printf( "\n 请输入正确的操作选项(0-2):");
	}
    getchar();   
    PrintMergeMenu();
	merge_func_choice=getchar();
	}
}

/*主函数*/
void main()
{
	char func_choice;
    PrintMenu();
	func_choice=getchar();
	putchar(func_choice);
	while (func_choice!='0')
	{
		switch (func_choice)
		{
			case '1' :
				InsertSort(); break;
			case '2' :
				SwapSort( ) ;   break;
			case '3' :
				ChooseSort( ) ;  break;
			case '4' :
				MergeSortMain( ) ; break;
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