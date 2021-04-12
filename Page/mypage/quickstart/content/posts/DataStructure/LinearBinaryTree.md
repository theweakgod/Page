---
title: "不知道做的啥玩意,有点久远了"
date: 2020-03-05T13:48:55+08:00

lastmod: 2020-11-20T18:31:26+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["不知道做的啥玩意,有点久远了","数据结构"]
categories: ["c语言","数据结构"]
featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  # ...
math:
  enable: true
  # ...
mapbox:
  accessToken: ""
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---

``` C
#include<stdio.h>
#define N 1100
int max(int a,int b){
	if(a<b){
		return b;
	}
	else{
		return a;
	}
}
int min(int a,int b){
	if(a<b){
	return a;
	}
	else{
	return b;
	}
}

int main(){
	char arr[N];
	char ass[N];
	int a[N];
	int b[N];
	int sum[N+1];
	int s;
	int q;
	int x,y,d,f,o,j,k;
	int l,m,n;
	scanf("%d",&n) ; 
	 sum[0]=-199;
	for(int i=0 ; i<n ; i++ ) {
		int g=0;
		scanf("%s",&arr);
		scanf("%s",&ass);
		for(d=0;d<N;d++){
			x=arr[d];
			if(x==0){
				break;
			}
		}
		for(f=0;f<N;f++){
			y=ass[f];
			if(y==0){
				break;
			}
		}
		for(o=0;o<d;o++){
			s=arr[o];
			if(s<=57&&s>=48){
				a[o]=arr[o]-48;
			}
		}
		for(j=0;j<f;j++){
			q=ass[j];
			if(q<=57&&q>=48){
				b[j]=ass[j]-48;
			}
		}
		x=max(d,f);
		y=min(d,f);
		for(k=x;k>=0;k--){
			if(k==0&&g==1){
				sum[k]=1;
			}
			else{
				if(k>x-y){
					if(d<f){
						sum[k]=a[y+k-1-x]+b[k-1]+g;
					}
					else{
						sum[k]=a[k-1]+b[y+k-1-x]+g;	
					}
					g=0;
					if(sum[k]>=10){
						g=1;
						sum[k]=sum[k]-10;
					}
				}
				else{
					if(d>f){
						sum[k]=a[k-1]+g;
						g=0;
					}
					else{
						sum[k]=b[k-1]+g;
						g=0;
					}
					
				}
			}
		}
		
		for(l=0;l<=x;l++){
			if(sum[0]==0){
				 sum[0]=-199;}
			if(sum[l]<10&&sum[l]>=0){
			printf("%d",sum[l]);	
			}
		}
		printf("\n");
	}
	return 0;
}

```