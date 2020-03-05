---
title: "类的学习6"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
#include<math.h>
using namespace std;
class CPoint{
	float x,y;
public:
	CPoint(float x=0,float y=0){}
	void set(){
		int i,j;
		cout<<"please input x,y"<<endl;
		cin>>i;
		cin>>j;
		x=i;
		y=j;
	}
	void setX(float i){
		x=i;
	}
	void setY(float i){
		y=i;
	}
	float getX(){
		return x;
	}
	float getY(){
		return y;
	}
	void display(){
		cout<<"X is"<<x<<". Y is"<<y<<endl;
	}
};
class CLine{
	CPoint Point1,Point2;
	float len,k;
public:
	void set(){
		cout<<"please input the first point's position"<<endl;
		Point1.set();
		cout<<"please input the second point's position"<<endl;
		Point2.set();
	}
	void get_Line(){
		Point1.display();
		Point2.display();
	}
	void caculator_len(){
		len=sqrt(pow((Point1.getX()-Point2.getX()),2)+pow((Point1.getY()-Point2.getY()),2));
	}
	void caculator_k(){
		k=(Point1.getY()-Point2.getY())/(Point1.getX()-Point2.getX ());
	}
	void display(){
		cout<<"k is "<<k<<". len is "<<len<<endl;
	}
};
class CCircle{
	CPoint Point;
	float r,area;
public:
	void set(){
		int i;
		cout<<"please input the point's position"<<endl;
		Point.set();
		cout<<"please input the circle's r"<<endl;
		cin>>i;
		r=i;
	}
	void calculator_area(){
		area=3.1415926*(pow(r,2));
	}
	void display(){
		Point.display();
		cout<<"r is"<<r<<". area is"<<area<<endl;
	}
};
void main(){
	CLine line;
	CCircle circle;
	line.set();
	line.get_Line();
	line.caculator_k();
	line.caculator_len();
	line.display();
	circle.set();
	circle.calculator_area();
	circle.display();
}
```