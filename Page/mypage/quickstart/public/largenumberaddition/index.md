# 大数加法

采用进位的方式达到1100百位字符串数字相加<br>
做的没啥用,效率太低,傅里叶变换的那种暂时看不懂.

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
