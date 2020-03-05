---
title: "类的学习7"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
#include<math.h>
using namespace std;
class CPoint{
	mutable double x,y;
public:
	void Print()const {
		cout<<"("<<x<<","<<y<<")"<<endl;
	}
	double GetX()const{
		return x;
	}
	double GetY()const {
		return y;
	}
	void SetX()const {
		cin>>x;
	}
	void SetY()const {
		cin>>y;
	}
	CPoint(double i=0,double j=0){
		x=i;
		y=j;
	}
	void display()const {
		cout<<"x is"<<x<<" y is"<<y<<endl;
	}
};
class CRectangle{
	CPoint p1,p2;
public:
	CRectangle(const CPoint &t1,const CPoint &t2){
		t1.SetX();
		t1.SetY();
		t2.SetX();
		t2.SetY();
		p1=t1;
		p2=t2;
	}
	void SetLPoint(const CPoint &t1){
		t1.SetX();
		t1.SetY();
		p1=t1;
	}
	void SetRPoint(const CPoint &t2){
		t2.SetX();
		t2.SetY();
		p2=t2;
	}
	void display(const CPoint &t1,const CPoint &t2){
		t1.display();
		t2.display();
	}
	double GetPerimeter(){
		double lenth1,lenth2,perimeter;
		lenth1=sqrt(pow(p1.GetX(),2)+pow(p1.GetY(),2));
		lenth2=sqrt(pow(p2.GetX(),2)+pow(p2.GetY(),2));
		perimeter=(lenth1+lenth2)*2;
		cout<<perimeter<<endl;
		return perimeter;
	}
	double GetArea(){
		double area1,area2,area;
		area=abs(p1.GetX()*p2.GetY()-p2.GetX()*p1.GetY());				//老师我这里没用矩形的面积，不知道。。这个矩形是哪个矩形，因为只有两个点，所以我自己用平
																			//行四边形的面积公式代替了。
		cout<<area<<endl;
		return area;
	}
};
void main(){
	CPoint p1,p2;
	CRectangle a_rectangle(p1,p2);
	a_rectangle.SetLPoint(p1);
	a_rectangle.SetRPoint(p2);
	a_rectangle.display(p1,p2);
	a_rectangle.GetPerimeter();
	a_rectangle.GetArea();
}
```