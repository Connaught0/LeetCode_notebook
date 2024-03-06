# 860 Lemon Change
thinking:
```
bool lemonadeChange(vector<int>& bills) {
        int inHand = 0;
        for(int i = 0; i < bills.size(); i++){
            if(bills[i] > 5 && inhand < bills[i]){
                return false;
            }
            inHand += 5;
        }
        return true;
}
```
上当了，不该这么想的，2个5块，不等于10块。。。。
my solution：考虑所有的每个结果
```
bool lemonadeChange(vector<int>& bills) {
        vector<int> cash = vector<int>(3, 0);
        //0 5, 1 10, 2 20

        for(int i = 0; i < bills.size(); i++){
            if(bills[i] == 5){
                cash[0]++;
            }
            else if(bills[i] == 10){
                if(cash[0] >= 1)
                {    
                    cash[0]--;
                    cash[1]++;
                }
                else{
                    return false;
                }
            }
            else if(bills[i] == 20){
                if(cash[1] > 0 && cash[0] > 0)
                {
                    cash[1]--;
                    cash[0]--;
                }
                else if(cash[1] == 0 && cash[0] >= 3){
                    cash[0] -= 3;
                }
                else{
                    return false;
                }
            }
        }
        return true;
    }
```
贪心的话我会怎么做？
收到10直接找5，如果是20就优先10

# 406 Queue Reconstruct by Height
solution:
```
    static bool cmp(vector<int> a, vector<int> b){
        if(a[0] == b [0]) return a[1] < b[1];
        return a[0] > b [0]; 
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp);
        list<vector<int>> que;
        for(int i = 0; i < people.size(); i++){
           int foreTaller = people[i][1];
           std::list<vector<int>>::iterator it = que.begin();
           while(foreTaller > 0){
               it++;
               foreTaller--;
           }
           que.insert(it, people[i]);
        }
        int i = 0;
        while(i < people.size()){
            people[i] = que.front();
            que.pop_front();
            i++;
        }
        return people;

    }
```
我最大的问题就是在想，如果第二个是0改如何排序，ex. [7,1],[7,0]，这里的贪心思想是  
对于第二个是1我直接往后移即可，那么他是0就没有一个通用的判断方法。
这里通过构建一个队列，那么如果第二个即是当前其在队伍中的顺序，作为贪心的解，即可把0提到前面来。

# 452 最小的射击气球的次数
solution：
```
static bool cmp(vector<int> a, vector<int> b){
        if(a[0] == b[0]) return a[1] < b[1];
        return a[0] < b[0];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),cmp);
        int ans = 1;
        int aim = points[0][1];
        for(int i = 1; i < points.size();i++){
            if(points[i][0] <= aim){
                if(points[i][1] < aim)
                    aim = points[i][1];//如果可以打到这个气球，但是这个气球的尾巴比aim小，就把aim变小，每次尽量打最多的
            }
            else{
                ans++;
                aim = points[i][1];
            }
        }
        return ans;

    }
```