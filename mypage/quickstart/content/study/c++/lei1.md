---
title: "类的学习1"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
class Account{
	int id;
	double balance;
	double annualInterestRate;
public:
	Account( ){
		cin>>id;
		cin>>balance;
		cin>>annualInterestRate;
	}
	double getMonthlyInterestRate(){
		return annualInterestRate/12;
	}
	void withdraw(){
		int x;
		cin>>x;
		if(balance<x){
			cout<<"Wake up, you don't have that much money."<<endl;
		}else{
			balance=balance-x;
			cout<<"Successful withdrawal"<<endl;
		}
	}
	void deposit(){
		int x;
		cin>>x;
		balance=balance+x;
		cout<<"Save money successfully"<<endl;
	}
	void display(){
		cout<<"Your balance is "<<balance<<" dollar"<<endl;
		cout<<"Your monthly interest rate is "<<annualInterestRate<<endl;
	}
};
void main(){
	Account A1;
	A1.withdraw();
	A1.deposit();
	A1.display();
}
```