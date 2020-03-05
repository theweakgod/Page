---
title: "类的学习2"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
#define PAI 3.1415926535
class circle{
	double radius,lenth,area;
public:
	circle(){
		cin>>radius;
		lenth=radius*PAI;
		area=radius*radius*PAI;
	}
	circle(circle &c){
		radius=c.radius;
		lenth=c.lenth;
		area=c.area;
	}
	void show(){
		cout<<radius<<"  "<<lenth<<"  "<<area<<endl;
	}
};
void main(){
	circle C1;
	C1.show();
	circle C2(C1);
	C2.show();
}
```