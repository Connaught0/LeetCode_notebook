# 343 Integer Break
这题其实在拆的时候就会发现，大的数字，是可以由小的数字加起来得到，因此我就开始在纸上推  
可以发现从4开始，拆出来的数组的乘积可以大于等于本身。  
因此3成为了最小的单元比如说8可以拆成2^4和3^2*2=>即比较2^3 and 3^2的大小比较。可以将最小单元定位3/4。5将其拆开得到的结果会比直接使用他更大，因此将所有的用3/4来表示，就可以得到最大的乘积。。。其实就是指数函数比底数的问题吧  
这样就是贪心算法的由来？
greedy solution：
```
        if(n == 2) return 1;
        if(n == 3) return 2;
        int loss = n % 3;
        int time = n / 3;
        if(loss == 1){
            return 4* pow(3,time -1);
        }
        else if(loss == 0){
            return pow(3, time);
        } 
        else{
            return loss * pow(3, time);
        }
```

dp:
```
    int integerBreak(int n) {
        vector<int> maxNum = vector<int>(n + 1);
        if(n == 2) return 1;
        if(n == 3) return 2; 
        maxNum[2] = 2;
        maxNum[3] = 3;
        for(int i = 4; i <= n ; i++){
            int a = i/2;
            int b = i/2 + i%2;
            maxNum[i] = maxNum[a] * maxNum[b];
        }
        return maxNum[n];
    }
```
一开始按照2和3作为最小单元去推断的，导致结果不对，并且我想这就是让其折断得到的乘积最大，但是8就是用2做最小单元是错位的结果。
dp solution：
```
vector<int> maxNum = vector<int>(n + 1);
        if(n == 2) return 1;
        if(n == 3) return 2; 
        maxNum[2] = 2;
        maxNum[3] = 3;
        for(int i = 4; i <= n ; i++){
            maxNum[i] = 0;
            for(int j = 2; j < i ; j++){
                maxNum[i] = max(maxNum[i], maxNum[j] * maxNum[i - j]);  
            }
        }
        return maxNum[n];
```