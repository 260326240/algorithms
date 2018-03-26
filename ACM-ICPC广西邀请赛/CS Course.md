# CS Course
[题目连接](http://acm.hdu.edu.cn/showproblem.php?pid=6186)  
Time Limit: 4000/2000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)
Total Submission(s): 1306    Accepted Submission(s): 615


# Problem Description
Little A has come to college and majored in Computer and Science.  
Today he has learned bit-operations in Algorithm Lessons, and he got a problem as homework.  

#### Here is the problem:  
You are giving n non-negative integers a1,a2,⋯,an, and some queries.  
A query only contains a positive integer p, which means you   
are asked to answer the result of bit-operations (and, or, xor) of all the integers except ap.  
## Input
There are no more than 15 test cases.   

Each test case begins with two positive integers n and p  
in a line, indicate the number of positive integers and the number of queries.  
##### 2≤n,q≤105  
Then n non-negative integers a1,a2,⋯,an follows in a line, 0≤ai≤109 for each i in range[1,n].  

After that there are q positive integers p1,p2,⋯,pqin q lines, 1≤pi≤n for each i in range[1,q].
## Output
For each query p, output three non-negative integers indicates the result of bit-operations(and, or, xor) of all non-negative integers except ap in a line.
 

## Sample Input
3 3  
1 1 1  
1  
2  
3  
## Sample Output
1 1 0  
1 1 0  
1 1 0  
 

## Source
2017ACM/ICPC广西邀请赛-重现赛（感谢广西大学）

## 题目大意
输入长度为N的序列A[]，输入P次查询，查询有三种位操作（与，或，异或），输入的p代表，除了数组A[P]内的，数组内所有数进行查询的三种操作。

## 思路
求前缀和与后缀和。  
开6个数组，输入序列时，前三个数组存（与，或，异或）的前缀值。  
输入完成后，后三个数组存，从n~1，生成的后缀值（与，或，异或）。  
当p=1时输出后缀和。当p=n时输出前缀和。  
当1<p<n时，输出：前三个数组与后三个输出的位操作值。

## 代码

    #include <iostream>  
    using namespace std;  
      
    const int maxn = 1e5 + 10;  
    int n,m,sum1[maxn],sum2[maxn],sum3[maxn],rsum1[maxn],rsum2[maxn],rsum3[maxn],a[maxn];  
      
      
    int main(){  
    while(~scanf("%d%d",&n,&m)){  
    scanf("%d",&a[1]);  
    sum1[1] = sum2[1] = sum3[1] = a[1];  
    for(int i = 2; i <= n; i ++){  
    scanf("%d",&a[i]);  
    sum1[i] = sum1[i-1] & a[i];  
    sum2[i] = sum2[i-1] | a[i];  
    sum3[i] = sum3[i-1] ^ a[i];  
    }  
    rsum1[n] = rsum2[n] = rsum3[n] = a[n];  
    for(int i = n-1; i >= 1; i --){  
    rsum1[i] = rsum1[i+1] & a[i];  
    rsum2[i] = rsum2[i+1] | a[i];  
    rsum3[i] = rsum3[i+1] ^ a[i];  
    }  
    int q;  
    while(m --){  
    scanf("%d",&q);  
    if(q == 1) printf("%d %d %d\n",rsum1[q+1],rsum2[q+1],rsum3[q+1]);  
    else if(q == n) printf("%d %d %d\n",sum1[n-1],sum2[n-1],sum3[n-1]);  
    else printf("%d %d %d\n",sum1[q-1] & rsum1[q+1],sum2[q-1]|rsum2[q+1],sum3[q-1]^rsum3[q+1]);  
    }  
    }  
    return 0;  
    }