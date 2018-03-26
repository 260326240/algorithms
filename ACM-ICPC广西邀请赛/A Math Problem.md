# A Math Problem
[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6182)

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)
Total Submission(s): 2752    Accepted Submission(s): 950


# Problem Description
You are given a positive integer n, please count how many positive integers k satisfy kk≤n.
 
# Input
There are no more than 50 test cases.  
Each case only contains a positivse integer n in a line.  
1≤n≤1018
 

# Output
For each test case, output an integer indicates the number of positive integers k satisfy kk≤n in a line.
 

# Sample Input
1  
4 

# Sample Output
1   
2  

## 思路
题目大意：输入N，求有多少个K的K次方小于N的数。  
思路：枚举小于10的18次方的所有K的K次方，保存到数组，从第一项到最后一项，比较哪一项大于N，然后输出K。  
##### 有个陷阱，当数组最大项小于10的18次方，但是输入的N大于最大项就没输出。
所以动手设置 1e18位最大项。

##### 倒着枚举也行。
## 代码
    #include<iostream>
    #include<cmath>
    #include<cstdio>
    using namespace std;
    #define  maxn 20
    long long p[maxn];
    int main()
    {
    long long r=pow(10,18);int q=0;
    for(int i=1;i<=15;i++)
    {
    long long ans=1;
    for(int j=1;j<=i;j++)ans*=i;
    p[i]=ans; 
    } 
    p[16] = 9223372036854775807;
    long long n; 
    while(cin>>n)
    {
    for(int i=1;i<=16;i++)
    {
    
    if(n<p[i])//如果n<数组里面的数就返回前一个数的下标 
    {
    cout<<i-1<<endl;
    break;
    } 
    }
    
    
    }
    
    return 0;
    }