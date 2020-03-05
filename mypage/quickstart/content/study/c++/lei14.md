---
title: "类的学习14"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
class Student{
	float score;
public:
	static float total_score;
	static int count;
	void account(){
		cin>>score;
		count=count+1;
		total_score=total_score+score;
	}
	static float sum(){
		return total_score;
	}
	static float average(){
		return total_score/count;
	}
};
float Student::total_score=0;
int Student::count=0;
void main(){
	Student St[5];
	int i;
	for(i=0;i<5;i++){
		St[i].account();
	}
	cout<<St[4].average()<<endl;
	cout<<St[4].sum()<<endl;
}

```