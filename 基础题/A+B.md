# 1000 A + B  
基准时间限制：1 秒 空间限制：131072 KB 分值: 0   
给出2个整数A和B，计算两个数的和。
#### Input
2个整数A B，中间用空格分割。（0 <= A, B <= 10^9）
#### Output
输出A + B的计算结果。
#### Input示例
1 2
#### Output示例
3
## 思路
注意：大数据longlong的输入输出。
## 代码
    #include<iostream>
    using namespace std;
    int main()
    {
    long long a,b;
    cin>>a>>b;
    cout<<a+b<<endl;
    return 0;
    }
