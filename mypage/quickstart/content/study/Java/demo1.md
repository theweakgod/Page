---
title: "学习记录"
date: 2020-03-02T16:56:36+08:00
draft: false
---
##### 学习记录

```java


import java.util.Scanner;                           //插入scanner库
public class test1{
    public static void main(String[] args) {
        Scanner reader=new Scanner(System.in);
        String s=reader.next();                     //输入字符串
        zhuanhua ZH = new zhuanhua(s);              //zhuanhua类的初始化,调用构造函数
        ZH.change();
    }
}
class zhuanhua{
    int dot;
    int sum;
    char []MoneyThatIWillEarn=new char[100];
    public zhuanhua(String s){                             //构造函数,字符串初始化
        for(int j=0;j<s.length();j++){
            MoneyThatIWillEarn[j]=s.charAt(j);
        }
        sum=s.length();                             //sum为字符串长度
        dot=0;                                      //dot表示小数点
    }
    public void change(){
        int flag;
        int cout;
        int cout1;
        int a;
        int b;
        for(int j=0;j<sum;j++){
            if((MoneyThatIWillEarn[j]<='9'&&MoneyThatIWillEarn[j]>'0')||MoneyThatIWillEarn[j]=='.'){
                if(MoneyThatIWillEarn[j]=='.'){
                    dot=j;                                  //把dot初始化
                }
            }
        }
        if(dot==0){
            dot=sum;                                        //如果没有小数点,就令dot等于长度
        }
        cout1=0;                                        //cout1为字符串的位置
        a=0;                                            //a是用来控制输出0 
        if(dot%8==0)b=0;                //如果dot为8的整数倍,t小于dot/8会多运行一次
        else b=-1;                      //否则便小于dot/8+1
        for(int t=0;t<((int)dot/8)-b;t++){
            flag=8;
            if(t==0){
                if(dot%8!=0)flag=dot%8;
            }
            cout=0;
            for(int s=0;s<flag;s++){
                if(t!=0&&s==0&&a!=0){
                    System.out.print("亿");         //如果t不等于0就输出亿单位,因为必定有8个数字在后面.
                }
                if(MoneyThatIWillEarn[cout1]=='1'){
                    System.out.print("壹");         //对应输出壹,下同
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='2'){
                    System.out.print("贰");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='3'){
                    System.out.print("叁");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='4'){
                    System.out.print("肆");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='5'){
                    System.out.print("伍");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='6'){
                    System.out.print("陆");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='7'){
                    System.out.print("柒");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='8'){
                    System.out.print("捌");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='9'){
                    System.out.print("玖");
                    a++;
                }
                if(MoneyThatIWillEarn[cout1]=='0'){         
                    if(cout1<dot&&MoneyThatIWillEarn[cout1+1]!='0'){
                        a++;                                //控制0的输出
                    }
                    if(a>=2){
                        if((s==7&&t==((int)dot/8)-b-1)||(((int)dot/8)-b-1==0&&s==flag-1)){
                            System.out.print("元");
                        }                               //控制0的输出,顺便控制前缀0
                        System.out.print("零");
                        a=0;
                    }
                }
                else{
                    if(t==0){
                        cout=s+8-flag;
                    }
                    if(t!=0){
                        cout=s;
                    }
                    if(cout==0){
                        System.out.print("千万");
                    }
                    if(cout==1){
                        System.out.print("百万");
                    }
                    if(cout==2){
                        System.out.print("十万");
                    }
                    if(cout==3){
                        System.out.print("万");
                    }
                    if(cout==4){
                        System.out.print("仟");
                    }
                    if(cout==5){
                        System.out.print("佰");
                    }
                    if(cout==6){
                        System.out.print("拾");
                    }
                }
                if((s==7&&t==((int)dot/8)-b-1)||(((int)dot/8)-b-1==0&&s==flag-1)){
                    if(a!=0&&MoneyThatIWillEarn[cout1]!='0')
                    System.out.print("元");
                }
                cout1++;
            }
        }//dot之前的都转换!
        if(dot==sum){
            System.out.println("整");
        }
        for(int t=dot+1;t<dot+5&&t<sum;t++){
            if(MoneyThatIWillEarn[t]=='1'){
                System.out.print("壹");
            }
            if(MoneyThatIWillEarn[t]=='2'){
                System.out.print("贰");
            }
            if(MoneyThatIWillEarn[t]=='3'){
                System.out.print("叁");
            }
            if(MoneyThatIWillEarn[t]=='4'){
                System.out.print("肆");
            }
            if(MoneyThatIWillEarn[t]=='5'){
                System.out.print("伍");
            }
            if(MoneyThatIWillEarn[t]=='6'){
                System.out.print("陆");
            }
            if(MoneyThatIWillEarn[t]=='7'){
                System.out.print("柒");
            }
            if(MoneyThatIWillEarn[t]=='8'){
                System.out.print("捌");
            }
            if(MoneyThatIWillEarn[t]=='9'){
                System.out.print("玖");
            }
            if(MoneyThatIWillEarn[t]=='0'){
                if(t<sum&&MoneyThatIWillEarn[t+1]!='0'){
                    System.out.print("零");            //控制0的输出,顺便控制后缀
                }
            }
            else{
                if(t==dot+1){
                    System.out.print("角");
                }
                if(t==dot+2){
                    System.out.print("分");
                }
                if(t==dot+3){
                    System.out.print("豪");
                }
                if(t==dot+4){
                    System.out.print("厘");
                }
            }
        }
        System.out.println();
    }
}
```

