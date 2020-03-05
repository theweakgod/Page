---
title: "类的学习4"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream> 
#include<string> 
using namespace std; 
class MyArray{ 
public: 
	MyArray(int leng);  
	~MyArray();  
	void Input();   
	void Display(string); 
protected:  
	int *alist;  
	int length;  
};  
MyArray::MyArray(int leng){ 
	if(leng<=0)  
	{    
	cout<<"error length";     
	exit(1);
	}      
	alist=new int [leng];     
	length=leng;     
	if(alist==NULL)  {   
	cout<<"assign failure";  
	exit(1);
	}   
	cout<<"MyArray 类对象已创建。"<<endl;  
}  
MyArray::~MyArray(){ 
	delete[] alist;   
	cout<<"MyArray 类对象被撤销。"<<endl;  
}  
void MyArray::Display(string str){
	int i;  
	int *p=alist;   
	cout<<str<<length<<"个整数：";   
	for(i=0;i<length;i++,p++)
			cout<<*p<<" ";   
	cout<<endl;  
}  
void MyArray::Input(){ 
	cout<<"请从键盘输入"<<length<<"个整数：";  
	int i;  
	int *p=alist;   
	for(i=0;i<length;i++,p++)    
	cin>>*p;      
}
class SortArray:public MyArray{
public:
	SortArray(int i):MyArray(i){
		Input();
	}
	void sort(){
		int i,j,tmp;
		for(i=0;i<length-1;i++){
			tmp=*(alist+i);
			for(j=i+1;j<length;j++){
				if(*(alist+i)>*(alist+j)){
					tmp=*(alist+j);
					*(alist+j)=*(alist+i);
					*(alist+i)=tmp;
				}
			}
		}
	}
	void display(){
		Display("整数是");
	}
};
class ReArray:public MyArray{
public:
	ReArray(int i):MyArray(i){
		Input();
	}
	void daozhi(){
		int i,tmp;
		for(i=0;i<length/2;i++){
			tmp=*(alist+length-1-i);
			*(alist+length-i-1)=*(alist+i);
			*(alist+i)=tmp;
		}
	}
	void display(){
		Display("整数是");
	}
};
class Average:public MyArray{
	double a;
public:
	Average(int i):MyArray(i){
		Input();
	}
	void aver(){
		int i;
		a=0;
		for(i=0;i<length;i++){
			a=a+*(alist+i);
		}
		a=a/length;
	}
	void display(){
		cout<<"平均值是"<<a<<endl;
	}
};
int main(){  
	MyArray a(5);  
	a.Input();   
	a.Display("显示已输入的");
	SortArray S1(5);
	S1.sort();
	S1.display();
	ReArray R1(5);
	R1.daozhi();
	R1.display();
	Average A1(5);
	A1.aver();
	A1.display();
	return 0; 
}
```