title: 用C写了个一个支持长数字输入输出计算器
id: 153
categories:
  - 未分类
date: 2015-02-26 15:35:12
tags:
---

市面上竟然没找到一个支持几千位数字输入的计算器软件，只好自己动手搞了一个。

目前暂时只支持非负整数的加减法运算，我毕竟还是很菜，代码又丑又烂。
```
#include<iostream>
#include<stdio.h>
using namespace std;
int main(){
	int a[10001],b[10001],o,oo,i,j;
	char c[20002],d;
	cout<<"Input equation like '1+1', only support + and - ."<<endl;
	gets(c);
	for(i=0;c[i+1]<'9'&&c[i+1]>'0'&&i<20001;i++){
		a[i]=c[i]-'0';
	}
	if(i==20001||i==0){cout<<"INPUT ERROR "; return 0;}
	i++;a[i]='/0';o=i;
	cin>>d;
	for(i=o,j=0;c[i+1]!='0'&&i<20002;i++,j++){
		b[j]=c[i]-'0';
	}
	if(i==20002){cout<<"INPUT ERROR "; return 0;}
	oo=j+1;
	if(d!='+'||d!='-'){cout<<"INPUT ERROR "; return 0;}
	if(d=='+'){
		if(o>=oo){
			for(i=o-1,j=oo-1;j>=0;i--,j--){
				a[i]+=b[j];
				if(a[i]>9&&i!=0){a[i]-=10;a[i-1]++;}
			}
			for(i=0;i<o;i++){cout<<a[i];}
		}
		else{
	     	for(i=o-1,j=oo-1;i>=0;i--,j--){
				b[j]+=a[i];
				if(b[j]>9&&j!=0){b[j]-=10;b[j-1]++;}
			}
			for(i=0;i<oo;i++){cout<<b[i];}
		}

	}
	else{
		if(o>=oo){
			for(i=o-1,j=oo-1;j>=0;i--,j--){
				a[i]-=b[j];
				if(a[i]<0&&i!=0){a[i]+=10;a[i-1]--;}
			}
			for(i=0;i<o;i++){cout<<a[i];}
		}
		else{
			cout<<"INPUT ERROR "; return 0;
		}

	} 

}
```
&nbsp;