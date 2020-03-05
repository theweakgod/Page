---
title: "类的学习2"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
class area 
{     
protected:      
	double height;     
	double width;   
public:      
	area(double h,double w)     
	{      
		height=h;      
		width=w;     
	}      
	virtual double getarea()=0; 
};  
class rectangle:public area{
public:
	rectangle(double h,double w):area(h,w){
	}
	virtual double getarea(){
		return height*width;
	}
};
class isosceles:public area{
public:
	isosceles(double h,double w):area(h,w){
	}
	virtual double getarea(){
		return height*width/2;
	}
};
void main(){
	area *a1;
	rectangle r1(10.0,5.0);
	isosceles i1(4.0,6.0);
	a1=&r1;
	cout<<a1->getarea()<<endl;
	a1=&i1;
	cout<<a1->getarea()<<endl;
}
```