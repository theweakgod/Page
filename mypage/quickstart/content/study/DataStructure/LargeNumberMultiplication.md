---
title: "大数乘法"
date: 2020-03-05T13:48:55+08:00
draft: false
---

``` C
#include<stdio.h>
#define N 200
int main(){
	char arr[N];
	char ass[N];
	int a[N];
	int b[N];
	int sum[2*N];
	int mul[N][N];
	int ii,j,k,ij,jj,ji;
	int mm,nn,tt,qqq,s,blog,gt;
	int ff;
	int flag=0;
	int x,y;
	scanf("%s",arr);
	scanf("%s",ass);
	qqq=0;
	s=0;
	for(j=0;j<N;j++){
		if(arr[j]==0){
			break;
		}
	}
	for(k=0;k<N;k++){
		if(ass[k]==0){
			break;
		}
	}
	for(ii=0;ii<N;ii=ii+1){
		if(arr[ii]<=57&&arr[ii]>=48){
			x=arr[ii];
			a[ii]=x-48;
		}
	}
	for(ij=0;ij<N;ij=ij+1){
		if(ass[ij]<=57&&ass[ij]>=48){
			y=ass[ij];
			b[ij]=y-48;
			}
	}
	for(jj=j-1;jj>=0;jj--){
		for(ji=k-1;ji>=0;ji--){
			mul[jj][ji]=a[jj]*b[ji];
		}
	}
	for(tt=j+k-1;tt>=0;tt--){
		sum[tt]=0;
	}
	for(tt=j+k-1;tt>=0;tt--){
		for(mm=j-1;mm>=0;mm--){
			for(nn=k-1;nn>=0;nn--){
				blog=mm+nn;
				if(mul[mm][nn]>=0){
					if(blog==tt){
						sum[tt]=sum[tt]+mul[mm][nn];
					}
					if(sum[tt+1]>9){
						sum[tt]=sum[tt]+sum[tt+1]/10;
						sum[tt+1]=sum[tt+1]%10;
					}
				}
			}
		}
	}
	for(ff=0;ff<j+k-1;ff++){
		if(sum[ff]>=0){
				printf("%d",sum[ff]);
		}
	}
	return 0;
}




```