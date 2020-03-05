---
title: "类的学习8"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
class CStereoShape{
public:
	virtual double GetArea()=0;
	virtual double GetVolume()=0;
};
class CCube:public CStereoShape{
	double Long,Width,Height;
public:
	CCube(double L,double W,double H){
		Long=L;
		Width=W;
		Height=H;
	}
	void Set_Parameter(){
		double L,W,H;
		cout<<"please input the long of cube:";
		cin>>L;
		cout<<endl<<"please input the width of cube:";
		cin>>W;
		cout<<"please input the height of cube:";
		cin>>H;
		Long=L;
		Width=W;
		Height=H;
	}
	virtual double GetArea(){
		return (2*Long*Width+2*Long*Height+2*Height*Width);
	}
	virtual double GetVolume(){
		return Long*Width*Height;
	}
};
class CSphere:public CStereoShape{
	double circuls;
public:
	CSphere(double r){
		circuls=r;
	}
	void Set_Parameter(){
		double L;
		cout<<"please input the r of CSphere:";
		cin>>L;
		circuls=L;
	}
	virtual double GetArea(){
		return (4*3.1415926535*circuls*circuls);
	}
	virtual double GetVolume(){
		return 4*3.1415926535*circuls*circuls*circuls/3;
	}
};
void main(){
	CStereoShape *CS;
	CCube C1(3.0,4.0,5.0);
	CSphere S1(7.0);
	C1.Set_Parameter();
	S1.Set_Parameter();
	CS=&C1;
	cout<<"cube's area is"<<CS->GetArea()<<endl;
	cout<<"cube's volume is"<<CS->GetVolume()<<endl;
	CS=&S1;
	cout<<"Sphere's area is"<<CS->GetArea()<<endl;
	cout<<"Sphere's volume is"<<CS->GetVolume()<<endl;
}
```