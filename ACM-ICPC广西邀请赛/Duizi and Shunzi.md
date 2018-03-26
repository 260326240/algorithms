#Duizi and Shunzi
Time Limit: 6000/3000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)
Total Submission(s): 1652    Accepted Submission(s): 674


# Problem Description
    Nike likes playing cards and makes a problem of it.
    
    Now give you n integers, ai(1≤i≤n)
    
    We define two identical numbers (eg: 2,2) a Duizi,
    and three consecutive positive integers (eg: 2,3,4) a Shunzi.
    
    Now you want to use these integers to form Shunzi and Duizi as many as possible.
    
    Let s be the total number of the Shunzi and the Duizi you formed.
    
    Try to calculate max(s).
    
    Each number can be used only once.
 

## Input
The input contains several test cases.

For each test case, the first line contains one integer n(1≤n≤106).   
Then the next line contains n space-separated integers ai (1≤ai≤n)
 

## Output
For each test case, output the answer in a line.

## Sample Input
7   
1 2 3 4 5 6 7  
9  
1 1 1 2 2 2 3 3 3  
6  
2 2 3 3 3 3   
6  
1 2 3 3 4 5  
 

## Sample Output
2    
4  
3  
2  

    Hint
    ----------
    Case 1（1，2，3）（4，5，6）
    Case 2（1，2，3）（1，1）（2，2）（3，3）
    Case 3（2，2）（3，3）（3，3）
    Case 4（1，2，3）（3，4，5）
    

 
 

## Source
2017ACM/ICPC广西邀请赛-重现赛（感谢广西大学）
## 题目大意
输入一个长度为N的序列，求这个序列中能组成对子和顺子的总数最大和。每个元素只能取一次。

## 思路
看懂题目之后，想法是：打算是先求顺子个数，再求对子个数两个和得到第一个值，然后反过来求，得到第二个值，取最大输出。太累赘了，直接爆。
网上思路是这样，证明如何取最优，不重复，无后效性。
    
    在存下每个牌面有多少张，然后从前到后去判断，当这个牌面有大于等于2张的时候，无论如何先组成对子，
	
	因为对子的代价比顺子少1张，然后就开始考虑顺子了，如果当前的牌还剩一张，并且后面一位牌面的牌数为奇
	
	数，后面两位的牌面有牌的话，就组成一张顺子。
取对子的优先级比顺子高。

## 代码
    #include <iostream>  
    #define LL long long  
    using namespace std;  
      
    const int maxn = 1e6 + 10;  
    int n,a[maxn];  
      
    int main(){  
    while(~scanf("%d",&n)){  
    int num;  
    memset(a,0,sizeof(a));  
    for(int i = 1; i <= n; i ++){  
    scanf("%d",&num); a[num] ++;  
    }  
    LL ans = 0;  
    for(int i = 1; i <= n; i ++){  
    ans += a[i] / 2;  
    a[i] %= 2;  
    if(i + 1 < n && a[i] && a[i+1] % 2 && a[i+2]){  
    ans ++; a[i] --; a[i+1] --; a[i+2] --;  
    }  
    }  
    printf("%lld\n",ans);  
    }  
    return 0;  
    }  