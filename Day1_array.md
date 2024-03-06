# 704 binary search
solution: 递归
```
int searchTool(vector<int>& nums, int fore, int back, int target){
        
        if(back == fore)
            return nums[back] == target ? back:-1;
        int mid = int((back + fore)/2);
        int big = -1, small = -1;

        if(target == nums[mid]){
            return mid;
        }
        else if(target > nums[mid]){
            if(mid >= nums.size())
                return -1;
            big = searchTool(nums, mid + 1, back, target);
        }
        else if(target < nums[mid]){
            if(mid <= 0)
                return -1;
            small = searchTool(nums, fore, mid - 1, target);
        }
        return max( max(big ,small),-1); 
    }
    int search(vector<int>& nums, int target) {
        return searchTool(nums,0,nums.size()-1, target);
    }
```
solution 迭代:
```
    int search(vector<int>& nums, int target) {
        int fore = 0;//左闭
        int back = nums.size();//右开？？
        while(fore < back){
            int mid = fore + (back - fore)/2;
            if(target > nums[mid])
            {
                fore = mid + 1;
            }
            else if(target < nums[mid]){
                back = mid;
            }
            else{
                return mid;
            }
        }
        return -1;
    }
```
# 27 remove element
solution double point:
```
    int removeElement(vector<int>& nums, int val) {
        int back = nums.size() - 1;
        int fore = 0;
        while(fore<=back){
            if(nums[fore] == val)
            {
                nums[fore] = nums[back];
                back --;
            }
            else{
                fore ++;
            }
        }
        return back + 1;
    }
```