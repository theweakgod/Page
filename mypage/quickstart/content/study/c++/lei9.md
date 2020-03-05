---
title: "类的学习9"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
class DATE{
	int date,month,year;
	int year1,month1,date1,year2,month2,date2;
public:
	DATE(){
		cin>>year>>month>>date;
	}
	DATE(int x,int y,int z){
		date=z;month=y;year=x;
	}
	void legal(){
		int flag=0;
		if(month<=12&&month>=1){
			switch (month){
				case 1:case 3:case 5:case 7:case 8: case 10: case 12:if(date<=31&&date>=1)flag=1;break;
				case 4:case 6:case 9:case 11:if(date<=30&&date>=1)flag=1;break;
				case 2:if(year%4==0&&(year%100!=0||year%400==0)){
						   if(date<=29&&date>=1)flag=1;
					   }else{
						   if(date<=28&&date>=1)flag=1;
					   }
			}
		}
		if(flag==0){
			cout<<"The position you entered is not valid."<<endl;
		}
		else{
			cout<<"The location you entered is legitimate"<<endl;
		}
	}
	void FZ(DATE &D1){
		year2=D1.year;month2=D1.month;date2=D1.date;
		year1=year;date1=date;month1=month;
	}
	void show(){
		cout<<year1<<" "<<month1<<" "<<date1<<" "<<year2<<" "<<month2<<" "<<date2<<endl;
	}
	int guibing(int month,int date){
		int sum1=0;
		switch (month){
			case 1:sum1=365-date;break;
			case 2:sum1=365-31-date;break;
			case 3:sum1=365-59-date;break;
			case 4:sum1=365-90-date;break;
			case 5:sum1=365-120-date;break;
			case 6:sum1=365-151-date;break;
			case 7:sum1=365-181-date;break;
			case 8:sum1=365-212-date;break;
			case 9:sum1=365-243-date;break;
			case 10:sum1=365-273-date;break;
			case 11:sum1=365-304-date;break;
			case 12:sum1=365-334-date;break;
		}
		return sum1;
	}
	void This_Year_FirstDay(){
		year1=year;month1=1;date1=1;
		year2=year;month2=month;date2=date;
	}
	int COUT(){
		int sum,a,b,flag;
		int i=0;
		int sum1,sum2;
		flag=1;
		sum1=sum2=0;
		a=year1/4;
		b=year2/4;
		if(year1%4==0){
			if(month1<=2)
				sum1++;
		}
		if(year2%4==0){
			if(month2>3)
				sum2++;
		}
		sum1+=guibing(month1,date1);
		sum2+=365-guibing(month2,date2);		
		if(b-a==0){
			if(year2-year1==1){
				sum=sum2+sum1;
			}
			else{
				sum=(year2-year1-1)*365+sum1+sum2;
			}
		}
		else{
			if(year2%4-year1%4==1){
				sum=sum1+sum2+(b-a)*1461;
			}
			else{
				flag=year2%4-year1%4;
				sum=(flag-1)*365+sum1+sum2+(b-a)*1461;
			}
		}
		flag=0;
		for(i=year1;i<=year2;i++){
			if(i%100==0&&i%400!=0)
				flag=flag+1;
		}
		sum=sum-flag;
		return sum;
	}
};
void main(){
	DATE D2;
	DATE D1;
	D2.This_Year_FirstDay();
	D1.This_Year_FirstDay();
	cout<<"You have already experienced"<<D2.COUT()<<" days of the year"<<endl;
	cout<<"You have already experienced"<<D1.COUT()<<" days of the year"<<endl;
	D1.legal();
	D2.legal();
	D2.FZ(D1);
	D2.show();
	cout<<D2.COUT()<<endl;
}
```