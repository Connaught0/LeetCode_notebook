# 239 max slider window

solution 1:
```
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        queue<int> sliderWin;
        //init sliderwin
        int i = 0;
        int max = INT16_MIN;
        while(sliderWin.size() < k)
        {
            if(nums[i] > max)
                max = nums[i];
            sliderWin.push(nums[i]);
            i++; 
        }
        //sliderWin catch the first k nums
        vector<int> ans;
        ans.push_back(max);
        while(i < nums.size())
        {
            sliderWin.pop();
            if(nums[i] > max)
                max = nums[i];
            sliderWin.push(nums[i]);
            ans.push_back(max);
        }
        return ans;
    }
```
只考虑了最大值，而没有考虑如果弹出的数值是最大值的时候该咋办。

solution 2：
```
public:
    class maxQueue{
        public:
            deque<int> res;
            void pop(int value)
            {
                if(!res.empty()&&value==res.front())
                    res.pop_front();
            }
            void push(int value)
            {
                while(!res.empty() && res.back() < value)
                    res.pop_back();
                res.push_back(value);
            }
            int front(){
                return res.front();
            }
    };
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        queue<int> sliderWin;
        //init sliderwin
        int i = 0;
        maxQueue max;
        while(sliderWin.size() < k)
        {
            max.push(nums[i]);
            sliderWin.push(nums[i]);
            i++; 
        }
        //sliderWin catch the first k nums
        vector<int> ans;
        ans.push_back(max.front());
        while(i < nums.size())
        {
            max.pop(sliderWin.front());
            sliderWin.pop();
            max.push(nums[i]);
            sliderWin.push(nums[i]);
            ans.push_back(max.front());
            //don not forget
            i++;
        }
        return ans;
    }
```
但是窗口其实已经保存在了单调队列里面，可以把额外存储的窗口优化掉

solution 3
```
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        //init sliderwin
        maxQueue max;
        for(int i=0; i < k; i++)
        {
            max.push(nums[i]);
        }
        //sliderWin catch the first k nums
        vector<int> ans;
        ans.push_back(max.front());
        for(int i = k; i < nums.size(); i++)
        {
            max.pop(nums[i - k]);
            max.push(nums[i]);
            ans.push_back(max.front());
        }
        return ans;
    }
```

# 347 topKfreq

抄的代码
还需要了解 priority_queue!
