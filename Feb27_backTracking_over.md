回溯问题本质上就是，如果需要将某个点丢掉，去看后面一部分的点的话，便是回溯问题
组合 无重复，使用startIndex
排列 有重复，不使用startIndex
分割 startIndex --- i为分割区间
子集 需要记录所有的路径上的点
使用used数组比直接使用unoreder_set更节约

# 332 reconstruct ltinerary
thinking:
```
//生成一个站点和集，确认每一个机场都有经过
//票的used参数
    unordered_map<string, int> stations;
    int stationsCheck;//单纯的用int去监督如果出站的话不好解决
    //vector<bool> stationsCheck;
    vector<bool> usedTickets;
    vector<string> path;
    vector<vector<string>> ans;

    void checkStation(string station){
        if(!stations[station]) stations[station] = true;    
        stationsCheck++;
    }
    void uncheckStation(string station){
        if(stations[station]) stations[station] = false;    
        stationsCheck--;
    }
    void backTracking(vector<vector<string>>& tickets, vector<bool> usedTickets){
        if(stationsCheck == stations.size()){
            ans.push_back(path);
            return;
        }
        for(int i = 0; i < tickets.size(); i++){
            if(path.empty()){
                path.push_back(tickets[i][0]);
                checkStation(tickets[i][0]);
            }
            if(tickets[i][0] == path.back() && usedTickets[i] != true){
                path.push_back(tickets[i][1]);
                checkStation(tickets[i][1]);
                usedTickets[i] = true;
                backTracking(tickets, usedTickets);
                usedTickets[i] = false;

            }
        }
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for(int i = 0; i < tickets.size(); i++){
            if(stations.find(tickets[i][0]) == stations.end()){
                stations[tickets[i][0]] = false;
            }
            if(stations.find(tickets[i][1]) == stations.end()){
                stations[tickets[i][1]] = false;
            }
        }
        usedTickets = vector<bool>(tickets.size(), false);     
    }
```
实际上我纠结的就是如何管理所有的站点：数组+计数器，总体不够完善。map+计数器，难以判断是否达到终点。
回溯三部曲我自己的想法就是：  
当站点走完了结束  
按照当前路径的最后一个去找下一张票，  

在遍历`unordered_map<出发机场, map<到达机场, 航班次数>> targets`的过程中，可以使用"航班次数"这个字段的数字做相应的增减，来标记到达机场是否使用过了。
如果“航班次数”大于零，说明目的地还可以飞，如果“航班次数”等于零说明目的地不能飞了，而不用对集合做删除元素或者增加元素的操作。
相当于说我不删，我就做一个标记！

终止条件是：我们回溯遍历的过程中，遇到的机场个数，如果达到了（航班数量+1）  

`for (pair<const string, int>& target : targets[result[result.size() - 1]]) {`

# 51 n-queen
本质上是一个放置了第一个，然后根据规则，所有左右前后斜向的点都不能放。然后在放第二个，将剩余的所有的位置都判断，如果可以够放n个，便返回结果。

solution：
规则有三，不同同列，不能同行，不能共斜线
那么我们逐行放置便能解决不能同行的问题，只有判断同列和共斜线。
到最后一行的时候即能收集结果。

回溯三部曲：
是否到了最后一行

当前列是否有？
共斜线是否有？

# 37 Sudoku Solver
判断条件超级加倍的n皇后问题？
主要是不像n皇后更加直观的能够看出来规则较为简单。。。

solution：
规则有三，不能同行，不能同列，不能共九宫格
本质上就是二维遍历，先找到可以放数的地方，然后将九个数都放进去尝试，如果可以就进入第二层，遍历所有结果，直到返回ture即可！！！
