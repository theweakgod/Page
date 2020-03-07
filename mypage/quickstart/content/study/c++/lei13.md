---
title: "类的学习13"
date: 2020-03-05T14:56:52+08:00
draft: false
---

```c++
#include<iostream>
using namespace std;
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
void main(){
	int year1,year2,month1,month2,date1,date2,sum1,sum2;
	int sum,a,b,flag;
	int i=0;
	flag=1;
	sum1=sum2=0;
	cout<<"请输入年月日";
	cin>>year1>>month1>>date1;
	cin>>year2>>month2>>date2;
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
	cout<<"天数为:"<<sum<<endl;
}

```