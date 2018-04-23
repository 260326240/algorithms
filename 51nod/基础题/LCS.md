# LCS最长公共子序列
求两个序列的最长公共子序列

    #include <iostream>
    #include <cmath>
    #include <cstring>
    #include <cstdio>
    using namespace std;
    
    typedef  long long ll;  
    
    char a[1001],b[1001];
    int dp[1001][1001];
    int main() {
    	cin>>a>>b;
    	int sum=0;
    	for(int i=0;i<strlen(a);i++)
    		for(int j=0;j<strlen(b);j++)
    		{
    			if(a[i-1]==b[j-1])
    			{
    				dp[i][j]=dp[i-1][j-1]+1;
    			}
    			else 
    			{
    				dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
    			}
    		}
    	cout<<dp[strlen(a)-1][strlen(b)-1]<<endl;
    return 0;
    }