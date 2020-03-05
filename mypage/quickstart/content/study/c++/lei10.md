---
title: "类的学习10"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
class Fan{
	int speed;
	double radius;
	char on[4];
	char color[10];
public:
	Fan(){
		cout<<"Please enter the speed, radius, switch status and color in sequence";
		cin>>speed>>radius;
		cin>>on;
		cin>>color;
	}
	void speed_change(){
		int x;
		cout<<"Please enter the speed you want to change";
		cin>>x;
		speed=x;
	}
	void speed_display(){
		cout<<"The speed of the fan is "<<speed<<endl;
	}
	void radius_display(){
		cout<<"The radius of the fan is "<<radius<<endl;
	}
	void on_display(){
		cout<<"The switch of the fan is "<<on<<endl;
	}
	void color_display(){
		cout<<"The color of the fan is "<<color<<endl;
	}
	void radius_change(){
		double x;
		cout<<"Please enter the radius you want to change";
		cin>>x;
		radius=x;
	}
	void on_change(){
		char x[4];
		cout<<"Please enter the on you want to change";
		cin>>x;
		strcpy(on,x);
		cout<<on<<endl;
	}
	void color_change(){
		char x[10];
		cout<<"Please enter the color you want to change";
		cin>>x;
		strcpy(color,x);
		cout<<color<<endl;
	}
	void display(){
		cout<<"radius: "<<radius<<" speed: "<<speed<<" switch: "<<on<<" color: "<<color<<endl;
	}
};
void main(){
	Fan F1;
	Fan F2;
	F1.display();
	F2.display();
	F1.color_display();
	F1.color_change();
	F1.display();
}
```