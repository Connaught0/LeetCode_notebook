# 738 Monotone Increasing Digits
simulation thinking:
```
    int monotoneIncreasingDigits(int n) {
        deque<int> num;
        while(n > 0){
            num.push_front(n%10);
            n = n/10;
        }
        int floor = 0;
        for(int i = 1; i < num.size(); i++){
            if(num[i] < num [i-1]){
                num[i - 1] --;
                num[i] += 10;
            }
            if(num[i] > 9){
                floor = num[i] - 9;
                num[i] = 9;
                floor *= 10;
            }
        }
        n = 0;
        for(int i = 0; i < num.size(); i++){
            n += num[i] * (pow(10, num.size() - i - 1));
        }
        return n;
    }
```
想着从前往后来看，不过应当从后往前来看
```
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        deque<int> num;
        while(n > 0){
            num.push_front(n%10);
            n = n/10;
        }
        int floor = 0;
        for(int i = num.size() - 1; i > 0; i--){
            if(num[i] < num [i - 1]){
                num[i - 1] --;
                num[i] += 10;
            }
            if(num[i] > 9){
                floor = num[i] - 9;
                num[i] = 9;
            }
        }
        n = 0;
        for(int i = 0; i < num.size(); i++){
            n += num[i] * (pow(10, num.size() - i - 1));
        }
        return n;
    }
};
```
现在解决了判断的问题，但是无法从后往前判断前面降位，后续溢出的问题
solution：
```
    int monotoneIncreasingDigits(int n) {
        deque<int> num;
        while(n > 0){
            num.push_front(n%10);
            n = n/10;
        }
        int flag = num.size();
        for(int i = num.size() - 1; i > 0; i--){
            if(num[i] < num [i - 1]){
                num[i - 1] --;
                num[i] = 9;
                flag = i;
            }
        }
        for(int i = flag; i < num.size(); i++){
            num[i] = 9;
        }
        n = 0;
        for(int i = 0; i < num.size(); i++){
            n += num[i] * (pow(10, num.size() - i - 1));
        }
        return n;
    }
```