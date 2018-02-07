# 1130 N的阶乘的长度 V2（斯特林近似）
基准时间限制：1 秒 空间限制：131072 KB   
输入N求N的阶乘的10进制表示的长度。例如6! = 720，长度为3。
## Input
第1行：一个数T，表示后面用作输入测试的数的数量。（1 <= T <= 1000)  
第2 - T + 1行：每行1个数N。（1 <= N <= 10^9)
## Output
共T行，输出对应的阶乘的长度。
## Input示例
3  
4  
5  
6
## Output示例
2  
3  
3 
# 思路
题目意思就是求n!的长度。n！很大要是一个个去相乘得到阶乘后又除10得到长度，整个过程的时间就慢得一b。  
看了大神的答案才知道有关于n!长度的一个公式--斯特林公式  
直接套用公式，但是注意：最后输出结果不要用%.0lf因为这样四舍五入，先转化为整数再输出  
斯特林公式 n！=sqrt（2*pi*n)*(n/e)^n  
那么 n的阶乘位数 = log10 (sqrt(2*pi*n)*(n/e)^n) +1 =0.5* log10 (2*pi*n)+n*log10(n/e) +1 ;

# 代码 
    
    #include<iostream>
    #include<cmath>
    #define e 2.718281828459
    using namespace std;
    typedef long long ll;
    
    int main()
    {
    	int t;
    	double n;
    	cin>>t;
    	while(t--)
    	{
    		cin>>n;
    		
    		cout<<(ll)(0.5*log10(2*acos(-1)*n)+n*log10(n/e)+1)<<endl;
    	}
    
    	return 0;
    } 