---
title: "类的学习12"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
#include<string>
#include<math.h>
using namespace std;
class Person{
	string name;
	string id;
	string phonenumber;
public:
	Person(){
		string name1,id1,phonenumber1;
		cout<<"please input his name:";
		cin>>name1;
		cout<<"please input "<<name1<<"'s id:";
		cin>>id1;
		cout<<"please input "<<name1<<"'s phonenumber:";
		cin>>phonenumber1;
		name=name1;
		id=id1;
		phonenumber=phonenumber1;
	}
	string get_name(){
		return name;
	}
	void get_MESSAGE(){
		cout<<name<<endl;
		cout<<name<<"'s id is"<<id<<endl;
		cout<<name<<"'s phonenumber is "<<phonenumber<<endl;
	}
};
class student:public Person{
	const string grade;
public:
	student(string s):Person(),grade(s){
	}
	void display(){
		get_MESSAGE();
		cout<<get_name()<<"'s grade is "<<grade<<endl;
	}
}; 
class MyDate{
	int year,month,day;
public:
	MyDate(){
		cout<<"please input year month day"<<endl;
		cin>>year;
		cin>>month;
		cin>>day;
	}
	MyDate(int x,int y,int z){
		year=x;
		month=y;
		day=z;
	}
	void date_display(){
		cout<<year<<"-";
		cout<<month<<"-";
		cout<<day<<endl;
	}
	int diffYear(MyDate &p){
		int i,j;
		if(p.year-year==0){
			if(p.month>month&&p.day>=day)
			return (abs(p.month-month));
			if(p.month<month&&day>=p.day)
				return (abs(p.month-month));
			return (abs(p.month-month)-1);
		}
		if(p.year-year>0){
			i=p.year-year-1;
			j=12-p.month+month;
			if(p.day>=day)
				return (i*12+j);
			else
			return (i*12+j-1);
		}
		else{							//偷个懒，直接else了。
			i=year-p.year-1;
			j=12-month+p.month;
			if(day>=p.day)
				return (i*12+j);
			else
				return (i*12+j-1);
		}
	}
};
class Employee:public Person{
	string office;
	double salary;
	MyDate dateHired;
public:
	Employee():Person(){
		cout<<"please input "<<get_name()<<"'s office:";
		cin>>office;
		cout<<"please input "<<get_name()<<"'s salary:";
		cin>>salary;
	}
	string get_office(){
		return office;
	}
	void Employee_display(){
		get_MESSAGE();
		cout<<get_name<<"'s office is"<<office<<endl;
		cout<<get_name()<<"'s Hired date is ";
		dateHired.date_display();
		cout<<endl;
	}
	double get_salary(){
		return salary;
	}
	int diffYear_Employee(MyDate &p){
		return dateHired.diffYear(p);
	}
};
class Faculty:public Employee{
	const int rank;
public:
	Faculty(int s):Employee(),rank(s){
	}
	void display(){
		Employee_display();
	}
};
class Staff:public Employee{
	string position;
	double BasicWages;
	const double Allowance;
public:
	Staff(double s):Employee(),Allowance(s){
		cout<<"please input "<<get_name()<<"'s position";
		cin>>position;
		BasicWages=get_salary();
	}
	void display(){
		cout<<get_name()<<"'s position is"<<position;
		Employee_display();
	}
	void display_salary(){
		int i;
		MyDate B1(2010,1,1);
		i=diffYear_Employee(B1);
		cout<<get_name()<<"'s salary is"<<BasicWages+Allowance*i<<endl;
	}
};
void main(){
	string s;
	double j;
	int i;
	cout<<"please input this student's grade"<<endl;
	cin>>s;
	student st1(s);
	st1.display();
	Employee E1;
	E1.Employee_display();
	string m;
	cout<<"please input this Faculty's position,this string belong in 'Professor AssociateProfessor AssistantProfessor'"<<endl;
	cin>>m;
	if(m=="Professor"){
		Faculty asd(3);
		asd.display();
	}
	if(m=="AssociateProfessor"){
		Faculty asd(2);
		asd.display();
	}
	if(m=="AssistantProfessor"){
		Faculty asd(1);
		asd.display();										//这里只能在if语句内进行调用display函数，估计是编译器问题，我的编译器只能这么写
	}
	cout<<"please input this staff's Allowance"<<endl;
	cin>>j;
	Staff staff1(j);
	staff1.display();
	staff1.display_salary();
}

```