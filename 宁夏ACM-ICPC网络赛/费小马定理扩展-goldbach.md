# Goldbach

## Description:

Goldbach's conjecture is one of the oldest and best-known unsolved problems in number theory and all of mathematics. It states:

Every even integer greater than 2 can be expressed as the sum of two primes.

The actual verification of the Goldbach conjecture shows that even numbers below at least 1e14 can be expressed as a sum of two prime numbers. 

Many times, there are more than one way to represent even numbers as two prime numbers. 

For example, 18=5+13=7+11, 64=3+61=5+59=11+53=17+47=23+41, etc.

Now this problem is asking you to divide a postive even integer n (2<n<2^63) into two prime numbers.

Although a certain scope of the problem has not been strictly proved the correctness of Goldbach's conjecture, we still hope that you can solve it. 

If you find that an even number of Goldbach conjectures are not true, then this question will be wrong, but we would like to congratulate you on solving this math problem that has plagued humanity for hundreds of years.

# Input:

The first line of input is a T means the number of the cases.

Next T lines, each line is a postive even integer n (2<n<2^63).

# Output:

The output is also T lines, each line is two number we asked for.

T is about 100.

本题答案不唯一，符合要求的答案均正确

## 样例输入
1  
8
# 样例输出
3 5

# 题目大意

哥德巴赫猜想  
求一个偶数n(2-2^63)可以分解成两个素数之和。
输出 某一个对素数，这对素数之和等于n。

# 思路 
分解这个n 成为a,b。a为较小的素数非常小，b为较大的素数非常大；  
所以只需要判定 a,b是不是素数即可。  
期初考虑使用欧拉筛，但是2^63次方会让我筛到老都筛成TLE。  
再次考虑 两个素数的判定。  
普通方法判定素数是比较快的 但是局限在素数比较小的范围。大数的判定就非常慢。  
所以这里就用到一个算法：费小马定理的加强版millar rabin，复杂度是log^3n。

首先先学一下这一个[知识点(戳我)](https://blog.csdn.net/zengaming/article/details/46932641)：快速积，快速幂

    int FastMul(int a,int b)//快速积 a*b  
    {  
    	int ans=0;  
    	while(b)  
    	{  
    	if(b&1)  
    	  ans=ans+a;  
    	a=a+a;  
    	b>>=1;  
    	}  
    	return ans;  
    }  

    int FastExp(int a,int b)//快速幂 a^b  
    {  
    	int ans=1;  
    	while(b)  
    	{  
    	if(b&1)  
     	 ans=ans*a;  
    	a*=a;  
    	b>>=1;  
    	}  
    	return ans;  
    }   

直接相乘的的复杂度是(o1),快速积的是(logN)，快速积是用在大数相乘时会溢出的情况，使用快速积来取模。
接下来来学习一下 快速积取模 与快速幂取模，快速积取模配合快速取模幂进行计算。

    int ModMul(int a,int b,int n)//快速积取模 a*b%n  
    {  
    	int ans=0;  
    	while(b)  
    	{  
    		if(b&1)  
      		 ans=(ans+a)%n;  
    		a=(a+a)%n;  
    	    b>>=1;  
    	}  
        return ans;  
    }  
    int ModExp(int a,int b,int n)//快速幂取模 a^b%n  
    {  
    	int ans=1;  
    	while(b)  
    	{  
    	if(b&1)  
      	 ans=ModMul(ans,a,n);  
    	a=ModMul(a,a,n);  
    	b>>=1;  
    	}  
    	return ans;  
    }  
然后让我们来了解一下
**费小马定理**：  
**“假如p是质数，a是整数，且a、p互质，那么a的(p-1)次方除以p的余数恒等于1，即：a^(p-1)≡1(mod p).”**

#### 什么是互质：互质数为数学中的一种概念，即两个或多个整数的公因数只有1的非零自然数。公因数只有1的两个非零自然数，叫做互质数**
** 二次探测定理：**
### 如果p是一个素数，0<x<p,则方程x^2≡1(mod p)的解为x=1或x=p-1。 ###

## Miller-Rabin 算法的理论基础：  
## 如果n是一个奇素数， 将n-1表示成2^s*r的形式(r是奇 数)a 是和n互素的任整数， 那么a^r≡1(mod n) 或者对某个j(0≤j ≤s －1， j∈Z) 等式 a^(2^j*r) ≡－1(mod n)成立。  这个理论是通过一个事实经由Fermat定理推导而来：n是一个奇素数，则方程x^2 ≡ 1 mod n只有±1两个解。

算法的过程：

对于要判断的数n

1.先判断是不是2，是的话就返回true。

2.判断是不是小于2的，或合数，是的话就返回false。

3.令n-1=u*2^t，求出u，t，其中u是奇数。

4.随机取一个a,且1<a<n

/*根据费马小定理，如果a^(n-1)≡1(mod p)那么n就极有可能是素数，如果等式不成立，那肯定不是素数了

因为n-1=u*2^t，所以a^(n-1)=a^(u*2^t)=(a^u)^(2^t)。*/

5.所以我们令x=(a^u)%n

6.然后是t次循环，每次循环都让y=(x*x)%n，x=y,这样t次循环之后x=a^(u*2^t)=a^(n-1)了

7.因为循环的时候y=(x*x)%n，且x肯定是小于n的，正好可以用二次探测定理，

如果(x^2)%n==1，也就是y等于1的时候，假如n是素数，那么x==1||x==n-1，如果x!=1&&x!=n-1，那么n肯定不是素数了，返回false。

8.运行到这里的时候x=a^(n-1)，根据费马小定理，x!=1的话，肯定不是素数了，返回false

9.因为Miller-Rabin得到的结果的正确率为 75%，所以要多次循环步骤4~8来提高正确率

10.循环多次之后还没返回，那么n肯定是素数了，返回true

**模板代码**

    # include <stdio.h>  
    # include <stdlib.h>  
      typedef long long ll;  
      ll ModMul(ll a,ll b,ll n)//快速积取模 a*b%n  
      {  
      ll ans=0;  
      while(b)  
      {  
      if(b&1)  
    ans=(ans+a)%n;  
      a=(a+a)%n;  
      b>>=1;  
      }  
      return ans;  
      }  
      ll ModExp(ll a,ll b,ll n)//快速幂取模 a^b%n  
      {  
      ll ans=1;  
      while(b)  
      {  
      if(b&1)  
    ans=ModMul(ans,a,n);  
      a=ModMul(a,a,n);  
      b>>=1;  
      }  
      return ans;  
      }  
      bool miller_rabin(ll n)//Miller-Rabin素数检测算法  
      {  
      ll i,j,a,x,y,t,u,s=10;  
      if(n==2)  
    return true;  
      if(n<2||!(n&1))  
    return false;  
      for(t=0,u=n-1;!(u&1);t++,u>>=1);//n-1=u*2^t  
      for(i=0;i<s;i++)  
      {  
      a=rand()%(n-1)+1;  
      x=ModExp(a,u,n);  
      for(j=0;j<t;j++)  
      {  
      y=ModMul(x,x,n);  
      if(y==1&&x!=1&&x!=n-1)  
    return false;  
      x=y;  
      }  
      if(x!=1)  
    return false;  
      }  
      return true;  
      }  
      int main()  
      {  
      ll n;  
      while(scanf("%lld",&n)!=EOF)  
    if(miller_rabin(n))  
      printf("Yes\n");  
    else  
      printf("No\n");  
      return 0;  
      }

题目AC代码

    #include <cstdio>
    #include <cstdlib>
    #include<iostream>
    #define N 10000
    using namespace std;
    typedef unsigned long long ll;
    ll ModMul(ll a,ll b,ll n)//快速积取模 a*b%n
    {
    ll ans=0;
      while(b)
      {
      if(b&1)
    	ans=(ans+a)%n;
      a=(a+a)%n;
      b>>=1;
      }
      return ans;
    }
    ll ModExp(ll a,ll b,ll n)//快速幂取模 a^b%n
    {
      ll ans=1;
      while(b)
      {
      if(b&1)
    	ans=ModMul(ans,a,n);
      a=ModMul(a,a,n);
      b>>=1;
      }
      return ans;
    }
    bool miller_rabin(ll n)//Miller-Rabin素数检测算法
    {
      ll i,j,a,x,y,t,u,s=10;
      if(n==2)
    	return true;
      if(n<2||!(n&1))
    	return false;
      for(t=0,u=n-1;!(u&1);t++,u>>=1);//n-1=u*2^t
      for(i=0;i<s;i++)
      {
      a=rand()%(n-1)+1;
      x=ModExp(a,u,n);
      for(j=0;j<t;j++)
      {
      y=ModMul(x,x,n);
      if(y==1&&x!=1&&x!=n-1)
    	return false;
      x=y;
      }
      if(x!=1)
    	return false;
      }
      return true;
    }
    
    int c[2000],k=1; 
    
    int main( )
    {
    	c[0]=2;
    	for(int j=3;j<=10000;j+=2)
    	{	
    		if(miller_rabin(j))c[k++]=j;	
    	}
    	
    	long long tar;
    	int t;
    	cin>>t;
    	while(t--)
    	{
    		cin>>tar;
    		for(int i=1;i<k;i++)
    		if(miller_rabin(tar-c[i])){
    			cout<<c[i]<<" "<<tar-c[i]<<endl;break;
    		}
    	}
    	return 0;
    }  