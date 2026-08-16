# LeetCode Hot 100 刷题记录

> 按专题记录解题思路、复杂度以及 C++ / Python 实现。当前仍在持续更新，部分专题包含 Hot 100 之外的扩展题。

## 使用说明

- “自主解答”记录首次作答情况，“二刷”记录复习时能否独立完成。
- 完成一轮后继续补充复习日期、易错点和更优解法。
- 代码以理解和复习为目的，后续会持续校正与优化。

## 目录

- [数组与哈希表](#数组与哈希表)
- [滑动窗口](#滑动窗口)
- [普通数组](#普通数组)
- [矩阵](#矩阵)
- [链表](#链表)
- [字符串](#字符串)
- [二叉树](#二叉树)
- [堆与优先级队列](#堆与优先级队列)
- [栈](#栈)
- [二分查找](#二分查找)
- [技巧](#技巧)
- [回溯算法](#回溯算法)
- [动态规划](#动态规划)
  - [01 背包问题](#01背包问题)
  - [完全背包问题](#完全背包问题)
  - [子序列问题](#子序列问题)
- [图论](#图论)

## 更新记录

### 2026-08-16

- **新增题目（11）：** 226. 翻转二叉树、543. 二叉树的直径、105. 从前序与中序遍历序列构造二叉树、437. 路径总和 III、236. 二叉树的最近公共祖先、124. 二叉树中的最大路径和、84. 柱状图中最大的矩形、121. 买卖股票的最佳时机、55. 跳跃游戏、45. 跳跃游戏 II、763. 划分字母区间。
- **二刷 / 三刷（3）：** 101. 对称二叉树（二刷独立完成）、42. 接雨水（二刷未独立完成）、739. 每日温度（三刷独立完成）。
- **复盘备注：** 本周集中练习二叉树递归、单调栈和贪心。基础递归与动态规划解法已有进展，但前缀和回溯、树形路径、单调栈边界和贪心区间维护仍需专项复习。

---

### 2026-08-09

- **新增题目：** 994. 腐烂的橘子、207. 课程表、100. 最大岛屿的面积、101. 孤岛的总面积、102. 沉没孤岛、117. 软件构建、208. 实现 Trie（前缀树）。
- **二刷 / 复习：** 102. 二叉树的层序遍历、46. 全排列、79. 单词搜索、131. 分割回文串、51. N 皇后、200. 岛屿数量。
- **复盘备注：** N 皇后二刷仍未独立完成，但补充了使用三个 `unordered_set` 分别记录列、主对角线和副对角线的解法；岛屿数量新增广度优先搜索实现。

---

#### 数组与哈希表

###### [1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：定义一个哈希表unordered_map。遍历一次，每次循环中先查查有没有能组成target的pair。如果没有则把当前元素插入map中；如果有直接返回。

时间复杂度：O(n)

C++代码：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int>um;
        vector<int>result;
        for(int i=0;i<nums.size();i++){
            // 如果找到了
            if(um.find(target-nums[i])!=um.end()){
                result.push_back(i);
                result.push_back(um[target-nums[i]]);
                break;
            }
            um[nums[i]]=i;
        }
        return result;
    }
};
```

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d={}
        for i in range(len(nums)):
            if target-nums[i] in d:
                return [i,d[target-nums[i]]]
            else:
                d[nums[i]]=i
        return []
```



###### [15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/?envType=problem-list-v2&envId=2cktkvj)

难度：中等偏难

自主解答：否。没想到双指针法

思路：先将数组排序以便去重。然后固定i，再用双指针法从两边向中间收缩，观察总和是否是0。

时间复杂度：O(n^2)

代码：

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n=nums.size();
        vector<vector<int>>result;
        sort(nums.begin(),nums.end());
        for(int i=0;i<n-2;i++){
            // 去重
            if(i>0&&nums[i]==nums[i-1])
                continue;
            unordered_map<int,int>um;
            int left=i+1,right=n-1;
            while(left<right){
                int sum=nums[i]+nums[left]+nums[right];
                if(sum<0)
                    left++;
                else if(sum>0)
                    right--;
                else{
                    result.push_back({nums[i],nums[left],nums[right]});
                    left++;right--;
                    while(left<right&&nums[left]==nums[left-1])
                        left++;
                    while(left<right&&nums[right]==nums[right+1])
                        right--;
                }
            }
        }
        return result;
    }
};
```

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result=[]
        nums.sort()
        n=len(nums)
        for i in range(n-2):
            if i>0 and nums[i]==nums[i-1]:
                continue
            d={}
            left=i+1
            right=n-1
            while left<right:
                total =nums[i]+nums[left]+nums[right]
                if total<0:
                    left+=1
                elif total>0:
                    right-=1
                else:
                    result.append([nums[i],nums[left],nums[right]])
                    left+=1
                    right-=1
                    while left<right and nums[left]==nums[left-1]:
                        left+=1
                    while left<right and nums[right]==nums[right+1]:
                        right-=1
        return result
```

###### [128. 最长连续序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-consecutive-sequence/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：否

思路：使用集合（哈希表）存储数据。遍历集合for num in set，如果set中找不到num-1，说明num是连续序列的第一个，这时候就要往后找num+1, num+2.....知道找不到为止

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int>us;
        for(int num:nums)
            us.insert(num);
        int maxLen=0;
        for(int num:us){
            // 如果num-1不在集合中，num就是第一个
            if(!us.count(num-1)){
                int len=0,cur=num;
                while(us.count(cur++)){
                    len++;
                }
                maxLen=max(len,maxLen);
            }
        }
        return maxLen;
    }
};
```



###### [11. 盛最多水的容器 - 力扣（LeetCode）](https://leetcode.cn/problems/container-with-most-water/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否。没想到可以双指针法一次遍历就能找到最大面积

思路：用双指针left和right，初始在两段。过程中，移动较短的线对应的指针。因为木桶效应，面积由短的线决定，所以只有移动短的线（比如left++或者right--）才有可能匹配到更大的面积。太妙了

时间复杂度：O(n)

代码：

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left=0,right=height.size()-1;
        int area=0;
        while(left<right){
            area=max(area,(right-left)*min(height[left],height[right]));
            if(height[left]<height[right]) 
                left++;
            else
                right--;
        }
        return area;
    }
};
```

###### [3. 无重复字符的最长子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否。不知道这类问题可以用滑动窗口解决

思路：使用双指针滑动窗口。使用一个集合保存遍历过的元素。右指针右移时，如果碰到元素在集合里面，就右移左指针，并删除集合中左指针对应的元素。

代码：

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int left=0,right=0;
        int length=0,maxLength=0;
        unordered_map<char,int>um;
        while(right<s.length()){
            // 如果没找到
            if(um.find(s[right])==um.end()){
                length=right-left+1;
                maxLength=max(maxLength,length);
                um[s[right]]=right;
                right++;
                continue;
            }else{
                um.erase(s[left]);
                left++;
            }
        }
        return maxLength;
    }
};
```

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = right = length = maxLen = 0
        a = set()
        while right < len(s):
            if s[right] not in a:
                length =right-left+1
                maxLen = max(maxLen, length)
                a.add(s[right])
                right += 1
            else:
                a.remove(s[left])
                left += 1
        return maxLen

```

###### [5. 最长回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-substring/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否。没思路

思路：回文子串分为奇数子串和偶数子串。可以以任意一个元素为中心（偶数则为任意相邻两个），然后指针分别向外扩散，从而找到最长回文子串。不过这题应该先采用动态规划的方法。dp(i, j)表示s[i...j]是否为回文子串，递推公式为dp(i,j)=dp(i+1,j-1)，注意i要从大到小遍历，这是由递推公式决定的。

时间复杂度：O(n^2)

动态规划代码：

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.length()<2)
            return s;
        int n=s.size(),maxLen=1,begin=0;
        vector<vector<bool>>dp(n,vector<bool>(n,0));
        for(int i=0;i<n;i++)
            dp[i][i]=1;
        for(int i=n-1;i>=0;i--){
            for(int j=i+1;j<n;j++){
                if(s[i]==s[j]){
                    if(j==i+1||dp[i+1][j-1]){
                        dp[i][j]=1;
                        if(j-i+1>maxLen){
                            maxLen=j-i+1;
                            begin=i;
                        }
                    }
                }
            }
        }
        return s.substr(begin,maxLen);
    }
};
```



双指针代码：

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int maxLen=0;
        int leftStart=0;
        int n=s.length();
        for(int i=0;i<n;i++){
            // 奇数回文
            int left=i,right=i;
            while(left>=0&&right<n&&s[left]==s[right]){
                if(right-left+1>maxLen){
                    maxLen=right-left+1;
                    leftStart=left;
                }
                left--;
                right++;
            }
            // 偶数回文
            left=i;right=i+1;
            while(left>=0&&right<n&&s[left]==s[right]){
                if(right-left+1>maxLen){
                    maxLen=right-left+1;
                    leftStart=left;
                }
                left--;
                right++;
            }
        }
        return s.substr(leftStart,maxLen);
    }
};
```

###### [283. 移动零 - 力扣（LeetCode）](https://leetcode.cn/problems/move-zeroes/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：使用ind记录非零元素的数量和应在的下标，遍历两次。第一次填充非零元素，第二次填充0

代码：

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int ind=0;
        for(int i=0;i<nums.size();i++){
            if(nums[i]!=0){
                nums[ind]=nums[i];
                ind++;
            }
        }
        for(int i=ind;i<nums.size();i++){
            nums[i]=0;
        }
    }
};
```

也可以使用双指针法遍历一次就解决，将nums[i]和nums[ind]交换就行

###### [448. 找到所有数组中消失的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：用一个长度为n+1的哈希表即可

代码：

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n=nums.size();
        vector<int>hash(n+1,0);
        for(int i:nums){
            hash[i]=1;
        }
        vector<int>result;
        for(int i=1;i<=n;i++){
            if(hash[i]==0){
                result.push_back(i);
            }
        }
        return result;
    }
};
```

进阶版：不额外申请空间，并且保证时间复杂度为O(n)

自主解答：否

思路：因为nums长度为n而且所有元素都是[1,n]之间，所以可以遍历nums把元素对应的下标标记为负数，以此来判断元素是否出现

代码：

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n=nums.size();
        for(int num:nums){
            // 将第num个数标记为负，表示出现过num
            int index=abs(num)-1;
            if(nums[index]>0)
                nums[index]*=-1;
        }
        vector<int>result;
        for(int i=0;i<n;i++){
            if(nums[i]>0){
                result.push_back(i+1);
            }
        }
        return result;
    }
};
```

###### [238. 除了自身以外数组的乘积 - 力扣（LeetCode）](https://leetcode.cn/problems/product-of-array-except-self/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等偏下

自主解答：否

思路：这题要用前缀积和后缀积。之前没有接触过所以想不出来

代码：

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n=nums.size();
        vector<int>answer(n,1);
        vector<int>prefix(n,1);
        vector<int>postfix(n,1);
        for(int i=1;i<n;i++){
            prefix[i]=nums[i-1]*prefix[i-1];
        }
        for(int i=n-2;i>=0;i--){
            postfix[i]=nums[i+1]*postfix[i+1];
        }
        for(int i=0;i<n;i++){
            answer[i]=prefix[i]*postfix[i];
        }
        return answer;
    }
};
```

进阶：不额外申请空间

自主解答：是

思路：把前缀和数组和后缀和数组替换成一个数即可

代码：

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n=nums.size();
        vector<int>answer(n,1);
        int prefix=1,postfix=1;
        for(int i=1;i<n;i++){
            prefix*=nums[i-1];
            answer[i]*=prefix;
        }
        for(int i=n-2;i>=0;i--){
            postfix*=nums[i+1];
            answer[i]*=postfix;
        }
        return answer;
    }
};
```



###### [33. 搜索旋转排序数组 - 力扣（LeetCode）](https://leetcode.cn/problems/search-in-rotated-sorted-array/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否

思路：大致跟二分查找类似。主要要知道，对于这种左旋之后的有序序列，至少有一半的元素是有序的。判断中间位置元素与左边元素的大小，就可以判断左边是有序还是右边有序。

我先自己另外写了一个正常的二分查找函数，如果target在一个有序序列中，就调用二分查找函数。后来发现不用这样，直接更改左右边界即可。因为这里面已经蕴含了二分查找的逻辑。

代码：

```C++
class Solution {
public:
    int binarySearch(vector<int>& nums, int l, int r, int target){
        int left=l,right=r;
        while(left<=right){
            int mid=(left+right)/2;
            if(target<nums[mid]){
                right=mid-1;
            }else if(target>nums[mid]){
                left=mid+1;
            }
            else
                return mid;
        }
        return -1;
    }
    int search(vector<int>& nums, int target) {
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=(left+right)/2;
            if(target==nums[mid])
                return mid;
            if(nums[mid]>=nums[left]){
                // 此时左边是有序的
                if(target>=nums[left]&&target<nums[mid])
                    right=mid-1;
                    //return binarySearch(nums, left, mid, target);
                else
                    left=mid+1;
            }else{
                // 此时右边是有序的
                if(target>nums[mid]&&target<=nums[right])
                    left=mid+1;
                    //return binarySearch(nums,mid,right,target);
                else
                    right=mid-1;
            }
        }
        return -1;
    }
};
```

###### [34. 在排序数组中查找元素的第一个和最后一个位置 - 力扣（LeetCode）](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否。没想到要使用两次二分查找，分别查找左边界和右边界。

思路：使用二分查找，分别查找左边界和右边界。

代码：

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int lBound=-1,rBound=-1;
        // 先找左边界
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=(left+right)/2;
            if(target<nums[mid])
                right=mid-1;
            else if(target>nums[mid])
                left=mid+1;
            else if(mid==0 || nums[mid-1]!=nums[mid]){
                lBound=mid;
                break;
            }else
                right=mid-1;
        }
        // 再找右边界
        left=0,right=nums.size()-1;
        while(left<=right){
            int mid=(left+right)/2;
            if(target<nums[mid])
                right=mid-1;
            else if(target>nums[mid])
                left=mid+1;
            else if(mid==nums.size()-1 || nums[mid+1]!=nums[mid]){
                rBound=mid;
                break;
            }else
                left=mid+1;
        }
        return {lBound,rBound};
    }
};
```

#### 滑动窗口

###### [209. 长度最小的子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-size-subarray-sum/)

n难度：简单

自主解答：是

思路：滑动窗口基础题，不难

代码：

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        // 滑动窗口
        int sum=0,n=nums.size();
        int minLen=INT_MAX;
        for(int i=0,j=0;j<n;j++){
            sum+=nums[j];
            while(sum>=target){
                minLen=min(minLen,j-i+1);
                sum-=nums[i];
                i++;
            }
        }
        return minLen==INT_MAX? 0:minLen;
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)

###### [76. 最小覆盖子串 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-window-substring/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否

思路：滑动窗口，基础思路跟上一题“209，最小长度子数组”差不多，但是对代码能力要求更高。

代码：以下代码通过的用例为265 / 268，在s和t很大时会超，时间复杂度O(m*sigma+n)，其中sigma为哈希表的长度，这里最大为52因为只有英文字母。

```C++
class Solution {
public:
    bool isCovered(unordered_map<char,int>a,unordered_map<char,int>b){
        // return true if b is covered by a
        for(auto pair:b){
            if(a[pair.first]<pair.second)
                return false;
        }
        return true;
    }
    string minWindow(string s, string t) {
        int m=s.size(),n=t.size();
        int start=0,minLen=INT_MAX;
        unordered_map<char,int>um_s;
        unordered_map<char,int>um_t;
        for(char c:t)
            um_t[c]++;
        for(int i=0,j=0;j<m;j++){
            um_s[s[j]]++;
            while(isCovered(um_s,um_t)){
                if(minLen>j-i+1){
                    minLen=j-i+1;
                    start=i;
                }
                um_s[s[i]]--;
                i++;
            }
        }
        return minLen==INT_MAX ? "" : s.substr(start,minLen);
    }
};
```

改进：isCovered函数用引用传递而不是值传递，这样可以通过所有用例

算法改进：这一题通过改进算法还能实现O(m+n)时间复杂度，这里不深究了

#### 普通数组

###### [53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

思路：贪心思路，舍弃所有小于0的局部和sum

时间复杂度O(n)

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result=INT_MIN,sum=0;
        for(int num:nums){
            sum+=num;
            result=max(result,sum);
            if(sum<0) sum=0;
        }
        return result;
    }
};
```

###### [56. 合并区间 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-intervals/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：否

思路：先排序列表，然后记住result的back存放的一定是等待合并的区间，前面的已经合并好了

时间复杂度O(n)

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end());
        int n=intervals.size();
        vector<vector<int>>result;
        for(int i=0;i<n;i++){
            if(i==0)
                result.push_back(intervals[i]);
            else{
                if(intervals[i][0]>result.back()[1])
                    result.push_back(intervals[i]);
                else if(intervals[i][1]>result.back()[1])
                    result.back()[1]=intervals[i][1];
            }
        }
        return result;
    }
};
```

###### [189. 轮转数组 - 力扣（LeetCode）](https://leetcode.cn/problems/rotate-array/description/?envType=study-plan-v2&envId=top-100-liked)

方法一：额外数组法

难度：简单

自主解答：是

时间复杂度O(n)，空间复杂度O(k)

```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k %= n;
        vector<int> temp(nums.end() - k, nums.end());
        for (int i = n - 1 - k; i >= 0; i--)
            nums[i + k] = nums[i];
        for (int i = 0; i < k; i++)
            nums[i] = temp[i];
    }
};
```

方法二：三次翻转法

难度：中等

自主解答：否

思路：这个解法比较巧妙。比如把1 2 3 4 5 6翻转成5 6 1 2 3 4的话，假设A=5 6, B=1 2 3 4。数组翻转有个性质，就是R(AB)=R(B)R(A)，根据这个性质，AB = R(R(AB)) = R( R(B)R(A) )，翻转三次

时间复杂度：O(n)，空间复杂度O(1)

```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k%=nums.size();
        reverse(nums.begin(),nums.end()-k);
        reverse(nums.end()-k,nums.end());
        reverse(nums.begin(),nums.end());
    }
};
```

###### [238. 除了自身以外数组的乘积 - 力扣（LeetCode）](https://leetcode.cn/problems/product-of-array-except-self/description/?envType=study-plan-v2&envId=top-100-liked)

方法一：前缀积&后缀积法

难度：简单

自主解答：是

时间复杂度O(n)，空间复杂度O(n)

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n=nums.size();
        vector<int>result(n);
        vector<int>prefix(n,1),postfix(n,1);
        for(int i=1;i<n;i++)
            prefix[i]=prefix[i-1]*nums[i-1];
        for(int i=n-2;i>=0;i--)
            postfix[i]=postfix[i+1]*nums[i+1];
        for(int i=0;i<n;i++)
            result[i]=prefix[i]*postfix[i];
        return result;
    }
};
```

方法二：用一个数代替前缀积数组合后缀积数组

难度：中等

自主解答：否

时间复杂度O(n)，空间复杂度O(1)

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 1);
        int prefix = 1, postfix = 1;
        for (int i = 1; i < n; i++) {
            prefix *= nums[i - 1];
            result[i] *= prefix;
        }
        for (int i = n - 2; i >= 0; i--) {
            postfix *= nums[i + 1];
            result[i] *= postfix;
        }
        return result;
    }
};
```

###### [41. 缺失的第一个正数 - 力扣（LeetCode）](https://leetcode.cn/problems/first-missing-positive/description/?envType=study-plan-v2&envId=top-100-liked)

方法一：哈希表法

难度：简单

自主解答：是

时间复杂度O(n)，空间复杂度O(n)

```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        unordered_set<int> us(nums.begin(), nums.end());
        int k = 1;
        while (us.count(k)) {
            k++;
        }
        return k;
    }
};
```

进阶：使用常数级别额外空间

难度：困难

自主解答：否

思路：这个方法叫原地哈希。想象教师里有n个桌子编号1-n；每个人的都坐到与自己数值相等的编号的桌子上，比如1号同学坐1号桌子。这样从一号桌子往后数，碰到的第一个数值不等于桌子编号的座位就是缺失的第一个正数

代码：

```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        // 给数字排列，数值为k的数应该排在第k个，即数组下标为k-1，这个就是targetInd
        // 这样再遍历一遍数组，第一个数值不等于排序的就是缺失的第一个正数
        int n=nums.size();
        for(int i=0;i<n;i++){
            int64_t targetInd=(int64_t)nums[i]-1;
            while(targetInd>=0&&targetInd<n&&nums[i]!=nums[targetInd]){
                swap(nums[i],nums[targetInd]);
                // 注意这里由于nums[i]变了，所以要更新targetInd
                targetInd=nums[i]-1;
            }
        }
        for(int i=0;i<n;i++){
            if(nums[i]!=i+1){
                return i+1;
            }
        }
        return n+1;
    }
};
```

时间复杂度O(n)，空间复杂度O(1)

#### 矩阵

###### [73. 矩阵置零 - 力扣（LeetCode）](https://leetcode.cn/problems/set-matrix-zeroes/description/?envType=study-plan-v2&envId=top-100-liked)

方法一：数组法

难度：中等

自主解答：否

思路：用两个额外数组分别存储每一行/每一列是否含有0

时间复杂度O(mn)，空间复杂度O(m+n)

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        // 空间复杂度O(m+n)
        int m=matrix.size(),n=matrix[0].size();
        vector<bool>hasZeroRow(m,0);
        vector<bool>hasZeroCol(n,0);
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0){
                    hasZeroRow[i]=1;
                    hasZeroCol[j]=1;
                }
            }
        }
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(hasZeroCol[j]||hasZeroRow[i])
                    matrix[i][j]=0;
            }
        } 
    }
};
```

方法二：O(1)空间复杂度

难度：中等偏难

自主解答：否

思路：用矩阵第一行和第一列充当上述两个数组。另外用两个额外布尔变量标记第一行/第一列是否有0

时间复杂度O(mn)，空间复杂度O(1)

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        // 空间复杂度O(1)
        int m = matrix.size(), n = matrix[0].size();
        bool hasZeroRow0 = false, hasZeroCol0 = false;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0)
                hasZeroCol0 = true;
        }
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0)
                hasZeroRow0 = true;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0)
                    matrix[i][j] = 0;
            }
        }
        if (hasZeroCol0) {
            for (int i = 0; i < m; i++)
                matrix[i][0] = 0;
        }
        if (hasZeroRow0) {
            for (int j = 0; j < n; j++)
                matrix[0][j] = 0;
        }
    }
};
```

###### [54. 螺旋矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：是

思路：用传统思路左闭右开区间遍历这个矩阵很麻烦。左闭右开适合遍历方阵。如果行和列不一样，要判断边界条件比如left==right或者top==bottom的情景

时间复杂度O(mn)，空间复杂度O(1)

```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        int m = matrix.size();
        int n = matrix[0].size();
        int left = 0, right = n - 1, top = 0, bottom = m - 1;
        while (left <= right && top <= bottom) {
            if (left == right) {
                for (int j = top; j <= bottom; j++)
                    result.push_back(matrix[j][left]);
                break;
            } else if (top == bottom) {
                for (int i = left; i <= right; i++)
                    result.push_back(matrix[top][i]);
                break;
            } else {
                for (int i = left; i < right; i++)
                    result.push_back(matrix[top][i]);
                for (int j = top; j < bottom; j++)
                    result.push_back(matrix[j][right]);
                for (int i = right; i > left; i--)
                    result.push_back(matrix[bottom][i]);
                for (int j = bottom; j > top; j--)
                    result.push_back(matrix[j][left]);
                left++;
                right--;
                top++;
                bottom--;
            }
        }
        return result;
    }
};
```

左闭右闭区间写法：
```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        int m = matrix.size();
        int n = matrix[0].size();
        int left = 0, right = n - 1, top = 0, bottom = m - 1;
        while (left <= right && top <= bottom) {
            for (int i = left; i <= right; i++)
                result.push_back(matrix[top][i]);
            for (int i = top+1; i <= bottom; i++)
                result.push_back(matrix[i][right]);
            if(top<bottom){
                for (int i = right-1; i >= left; i--)
                    result.push_back(matrix[bottom][i]);
            }
            if(left<right){
                for (int i = bottom-1; i >= top+1; i--)
                    result.push_back(matrix[i][left]);
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return result;
    }
};
```

###### [48. 旋转图像 - 力扣（LeetCode）](https://leetcode.cn/problems/rotate-image/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：是

思路：这题给我刷力竭了。整体思路不难，就是一层一层从外往里实现旋转即可，旋转就是四个数相互交换。确定左边界和右边界，难点是点的位置和边界之间的关系

代码：

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int left=0,right=matrix[0].size()-1;
        while(left<right){
            for(int i=left;i<right;i++){
                int temp=matrix[left][i];
                matrix[left][i]=matrix[right-i+left][left];
                matrix[right-i+left][left]=matrix[right][right-i+left];
                matrix[right][right-i+left]=matrix[i][right];
                matrix[i][right]=temp;
            }
            left++;
            right--;
        }
    }
};
```

这题有更简单的解法，就是先转置，再按行翻转

代码：

```C++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n=matrix[0].size();
        // 先转置
        for(int i=0;i<n;i++){
            for(int j=i;j<n;j++){
                swap(matrix[i][j],matrix[j][i]);
            }
        }
        // 再翻转行
        for(int i=0;i<n;i++){
            reverse(matrix[i].begin(),matrix[i].end());
        }
    }
};
```

###### [240. 搜索二维矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/search-a-2d-matrix-ii/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否

思路：这一题挺难想到的。以矩阵右上角为基准与target比较。如果偏大，就向左移；如果偏小就向下移。有点类似二叉排序树，右上角是根节点

时间复杂度O(m+n)，空间复杂度O(1)

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m=matrix.size();
        int n=matrix[0].size();
        int i=0,j=n-1;
        while(i<m&&j>=0){
            if(matrix[i][j]==target)
                return true;
            else if(matrix[i][j]>target)
                j--;
            else
                i++;
        }
        return false;
    }
};
```



#### 链表

###### [234. 回文链表 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-linked-list/description/?envType=problem-list-v2&envId=swc72RWg)

难度：简单

自主解答：是

思路：把链表转化成一个数组就能判断了

代码：

```C++
class Solution {
public:
    bool isPalindrome(vector<int>&list){
        int left=0,right=list.size()-1;
        while(left<=right){
            if(list[left]!=list[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
    bool isPalindrome(ListNode* head) {
        vector<int>list;
        while(head){
            list.push_back(head->val);
            head=head->next;
        }
        return isPalindrome(list);
    }
};
```

进阶：保证空间复杂度为O(1)

难度：中等

自主解答：是

思路：求链表长度、反转后半部分链表，比较前后两链表是否相同

代码：

```C++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(!head||!head->next)
            return true;
        // 先获得链表长度
        int len=0;
        ListNode* cur=head;
        while(cur){
            len++;
            cur=cur->next;
        }
        // 翻转后半部分的链表
        int mid=(len+1)/2;
        cur=head;
        while(mid--){
            cur=cur->next;
        }
        ListNode* pre=nullptr;
        while(cur){
            ListNode* temp=cur->next;
            cur->next=pre;
            pre=cur;
            cur=temp;
        }
        // 比较前一半和后一半是否相同
        while(head&&pre){
            if(head->val!=pre->val)
                return false;
            head=head->next;
            pre=pre->next;
        }
        return true;
    }
};
```



###### [206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：使用一个pre节点，是head指向pre，然后pre和head向后移动

代码：

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre=nullptr;
        while(head){
            ListNode* temp=head->next;
            head->next=pre;
            pre=head;
            head=temp;
        }
        return pre;
    }
};
```

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        pre=None
        cur=head
        while(cur):
            temp=cur.next
            cur.next=pre
            pre=cur
            cur=temp
        return pre
```

###### [21. 合并两个有序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-sorted-lists/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：否

思路：list1和list2交替前进，谁小就让cur的next指向谁，然后再前进一位

代码：

```C++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* cur=new ListNode();
        ListNode* dummyHead=cur;
        while(list1&&list2){
            if(list1->val<=list2->val){
                cur->next=list1;
                list1=list1->next;
            }
            else{
                cur->next=list2;
                list2=list2->next;
            }
            cur=cur->next;
        }
        if(list1==nullptr)
            cur->next=list2;
        else
            cur->next=list1;
        return dummyHead->next;
    }
};
```

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        dummyHead=ListNode()
        cur=dummyHead
        while list1 and list2:
            if list1.val<=list2.val:
                cur.next=list1
                cur=list1
                list1=list1.next
            else:
                cur.next=list2
                cur=list2
                list2=list2.next
        if list1 == None:
            cur.next=list2
        else:
            cur.next=list1
        return dummyHead.next
```

######  [160. 相交链表 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=problem-list-v2&envId=2cktkvj&)

难度：简单

自主解答：是

思路：先求出两个链表的长度。在让更长的链表头结点向前移动，直到两个头结点后面的距离一样。最后同时移动两个头结点，直到为空或者相交。

代码：

```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA=0,lenB=0;
        ListNode* a=headA;
        ListNode*b=headB;
        while(headA){
            lenA++;
            headA=headA->next;
        }
        while(headB){
            lenB++;
            headB=headB->next;
        }
        int diff=abs(lenA-lenB);
        while(diff--){
            if(lenA>lenB)
                a=a->next;
            else if(lenA<lenB)
                b=b->next;
        }
        while(a&&b){
            if(a==b){
                return a;
            }
            a=a->next;
            b=b->next;
        }
        return nullptr;
    }
};
```

###### [141. 环形链表 - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：类似之前的两数之和，用一个集合存储遍历过的元素。

时间复杂度：O(n)

代码：

```C++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        unordered_set<ListNode*>us;
        while(head){
            if(us.find(head)!=us.end()){
                return true;
            }
            us.insert(head);
            head=head->next;
        }
        return false;
    }
};
```

进阶：不申请额外内存

自主解答：是

思路：两个指针，一个一次跑一步，一个一次跑两步，看是否能相遇

代码：

```C++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* head_1=head;
        while(head&&head_1&&head_1->next){
            head=head->next;
            head_1=head_1->next->next;
            if(head==head_1)
                return true;
        }
        return false;
    }
};
```

###### [142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：跟上一题一样，用集合

代码：

```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*>us;
        while(head){
            if(us.find(head)!=us.end()){
                return head;
            }
            us.insert(head);
            head=head->next;
        }
        return nullptr;
    }
};
```

进阶：不申请额外内存

难度：中等

自主解答：否

思路：这里我们抛弃一直学的公式法，用直觉来感受。假设慢指针走了x，快指针走了2x，他们在环内相遇。然后，再让慢指针移动x，同时让一个新的指针new_ptr从起点开始以慢指针相同的速度也前进x。这时慢指针和new_ptr一定会相遇，而且位置就在快慢指针相遇的位置，因为慢指针总的移动了2x，而new_ptr移动了x，这跟前面快慢指针距离相同了。既然new_ptr和慢指针在同一位置而且速度相同，那么他们一定会在环的入口相遇！他们首次相遇的点就是环的入口！

代码：

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* detectCycle(ListNode* head) {
        ListNode *fast = head, *slow = head;
        while (slow && fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) {
                ListNode* ptr = head;
                while (ptr != slow) {
                    ptr = ptr->next;
                    slow = slow->next;
                }
                return ptr;
            }
        }
        return NULL;
    }
};
```

###### [19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：想要删除倒数第n个节点，就要找到正数第len-n个节点，其中len是链表的长度

代码：

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0, head);
        int len = 0;
        while (head) {
            len++;
            head = head->next;
        }
        // 要删除倒数第n个，就要找到正数第len-n个
        ListNode* cur = dummyHead;
        for (int i = 0; i < len - n; i++) {
            cur = cur->next;
        }
        ListNode* temp = cur->next;
        cur->next = temp->next;
        delete temp;
        return dummyHead->next;
    }
};
```

###### [24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

时间复杂度O(n)，空间复杂度O(1)

```C++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead=new ListNode(0,head);
        ListNode* cur=dummyHead;
        while(cur->next&&cur->next->next){
            ListNode* temp=cur->next;
            cur->next=cur->next->next;
            temp->next=cur->next->next;
            cur->next->next=temp;
            cur=temp;
        }
        return dummyHead->next;
    }
};
```

###### [2. 两数相加 - 力扣（LeetCode）](https://leetcode.cn/problems/add-two-numbers/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：是

思路：如果明白CPU运行整数加法的逻辑，这一题很简单。就是加数+进位

代码：

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummyHead=new ListNode();
        ListNode* cur=dummyHead;
        int carry=0, num=0;
        int sum=0;
        while(l1||l2){
            if(!l1){
                sum=l2->val+carry;
                l2=l2->next;
            }
            else if(!l2){
                sum=l1->val+carry;
                l1=l1->next;
            }
            else{
                sum=l1->val+l2->val+carry;
                l1=l1->next;
                l2=l2->next;
            }
            carry=sum/10;
            num=sum%10;
            ListNode* node=new ListNode(num);
            cur->next=node;
            cur=cur->next;
        }
        if(carry==1){
            ListNode* node=new ListNode(1);
            cur->next=node;
            cur=cur->next;
        }
        return dummyHead->next;
    }
};
```

###### [92. 反转链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list-ii/description/)

难度：中等

自主解答：是

思路：记住反转链表要三个指针：P0, Cur 和 Pre。

时间复杂度O(n)，空间复杂度O(1)

```C++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* dummyHead = new ListNode(0, head);
        ListNode* pre = nullptr;
        ListNode* p0 = dummyHead;
        for (int i = 1; i < left; i++) {
            p0 = p0->next;
        }
        ListNode* cur = p0->next;
        for (int i = left; i <= right; i++) {
            ListNode* temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        p0->next->next = cur;
        p0->next = pre;
        return dummyHead->next;
    }
};
```

###### [25. K 个一组翻转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

思路：这一题有点难，但是掌握反转链表的技巧之后也能解决。要注意反转链表时候，需要有一个前置节点P0，一个cur也一个初始化为空的pre节点。

```C++
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(!head) return nullptr;
        int n=0;
        for(ListNode* cur=head;cur;cur=cur->next)
            n++;
        ListNode* dummyHead=new ListNode(0,head);
        ListNode* p0=dummyHead;
        ListNode* cur=p0->next;
        while(n>=k){
            ListNode* temp_cur=cur;
            ListNode* pre=nullptr;
            for(int j=0;j<k;j++){
                ListNode* temp=cur->next;
                cur->next=pre;
                pre=cur;
                cur=temp;
            }
            temp_cur->next=cur;
            p0->next=pre;
            p0=temp_cur;
            n-=k;
        }
        return dummyHead->next;
    }
};
```

###### [138. 随机链表的复制 - 力扣（LeetCode）](https://leetcode.cn/problems/copy-list-with-random-pointer/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否

思路：哈希表法，用一个哈希映射存储每个节点和对应的复制节点。注意，C++17 遍历unordered_map可以直接auto [key, val]: umap

时间复杂度O(n)，空间复杂度O(n)

```C++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head==nullptr) return nullptr;
        unordered_map<Node*,Node*>umap;
        for(Node* cur=head;cur;cur=cur->next){
            umap[cur]=new Node(cur->val);
        }
        for(auto kv:umap){
            if(kv.first->next)
                umap[kv.first]->next=umap[kv.first->next];
            if(kv.first->random)
                umap[kv.first]->random=umap[kv.first->random];
        }
        return umap[head];
    }
};
```

方法二：拉链法

###### [148. 排序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/sort-list/description/?envType=study-plan-v2&envId=top-100-liked)

解法一：冒泡排序，但是会超时

难度：中等

自主解答：是

时间复杂度:O(n*n)，空间复杂度O(1)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        // 冒泡排序
        ListNode* cur=head;
        ListNode* dummyHead=new ListNode(0,head);
        int n=0;
        for(;cur;cur=cur->next)
            n++;
        for(int i=1;i<n;i++){
            cur=dummyHead->next;
            ListNode* p0=dummyHead;
            for(int j=0;j<n-i;j++){
                if(cur->val>cur->next->val){
                    ListNode* temp=cur->next;
                    cur->next=temp->next;
                    temp->next=cur;
                    p0->next=temp;
                    p0=temp;
                }else{
                    cur=cur->next;
                    p0=p0->next;
                }
            }
        }
        return dummyHead->next;
    }
};
```



###### [146. LRU 缓存 - 力扣（LeetCode）](https://leetcode.cn/problems/lru-cache/description/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

思路：LRU算法，定义一个双端队列和哈希映射。像在书架上放书一样，新书在最左边，最老的书在最右边。如果要查找一个书就拿住来，然后再从最左边放回去。如果要加入一个书也插入最左边，然后如果满了就从最右边拿一个书下来。

代码：

```C++
class LRUCache {
    struct Node {
        int key; int value; Node *pre; Node *next;
        Node(int k, int v) : key(k), value(v), pre(nullptr), next(nullptr) {}
    };
    unordered_map<int, Node*> cache;
    int capacity;
    Node* head;
    Node* tail;
public:
    LRUCache(int capacity) {
        this->capacity = capacity;
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->pre = head;
    }
    void remove(Node* node) {
        node->pre->next = node->next;
        node->next->pre = node->pre;
    }
    void insertHead(Node* node) {
        node->next = head->next;
        node->pre = head;
        head->next = node;
        node->next->pre = node;
    }
    int get(int key) {
        if (cache.find(key) == cache.end())
            return -1;
        Node* node = cache[key];
        remove(node);
        insertHead(node);
        return node->value;
    }

    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            remove(node);
            insertHead(node);
            node->value = value;
        } else {
            Node* node = new Node(key, value);
            if (cache.size() == capacity){
                cache.erase(tail->pre->key);
                remove(tail->pre);
            }
            insertHead(node);
            cache[key]=node;
        }
    }
};
```



#### 字符串

###### [20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：用栈即可

代码：

```C++
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        for (char c : s) {
            if (c == '(' || c == '[' || c == '{')
                stk.push(c);
            else {
                if (stk.empty())
                    return false;
                if (c == ')') {
                    if (stk.top() == '(') {
                        stk.pop();
                    } else
                        return false;
                } else if (c == ']') {
                    if (stk.top() == '[')
                        stk.pop();
                    else
                        return false;
                } else {
                    if (stk.top() == '{')
                        stk.pop();
                    else
                        return false;
                }
            }
        }
        return stk.empty();
    }
};
```

###### [49. 字母异位词分组 - 力扣（LeetCode）](https://leetcode.cn/problems/group-anagrams/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否

思路：这一题很妙，用首先，对于任意几个字母异位词，把它们排序之后一定是一样的，这就能当作map的key，而value就是单词自身组成的数组。比如，aet ->[eat, tea, tae]。

时间复杂度：O(n*klogk)，n是字符串数组长度，k是字符串平均长度

代码：

```C++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>>ump;
        for(string s:strs){
            string key=s;
            sort(key.begin(),key.end());
            ump[key].push_back(s);
        }
        vector<vector<string>>result;
        for(auto pair: ump){
            result.push_back(pair.second);
        }
        return result;
    }	
};
```

#### 二叉树

###### [94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：中序遍历左中右

代码：

```C++
class Solution {
public:
    void Travarsal(vector<int>&result, TreeNode* node){
        if(node==nullptr)   return;
        Travarsal(result, node->left);
        result.push_back(node->val);
        Travarsal(result, node->right);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int>result;
        Travarsal(result, root);
        return result;
    }
};
```

###### [104. 二叉树的最大深度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：直接递归

代码：

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr)   
            return 0;
        int m=max(maxDepth(root->left),maxDepth(root->right));
        return m+1;
    }
};
```

###### [101. 对称二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/symmetric-tree/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否

思路：需要把问题等价于判断两棵树是否互为镜像。

代码：

```C++
class Solution {
public:
    bool isMirror(TreeNode* left, TreeNode* right){
        if(!left&&!right)   return true;
        if(!left || !right) return false;
        if(left->val!=right->val)   return false;
        return isMirror(left->right,right->left)
            &&isMirror(left->left, right->right);
    }
    bool isSymmetric(TreeNode* root) {
        if(!root)   return true;
        return isMirror(root->left, root->right);
    }
};
```

###### [102. 二叉树的层序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：否；二刷：是

思路：忘记了queue的size()方法，可以直接返回队列的大小

代码：

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>>result;
        if(root==nullptr)   return result;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            int size=q.size();
            vector<int>level;
            while(size--){
                TreeNode* node=q.front();
                level.push_back(node->val);
                q.pop();
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
            result.push_back(level);
        }
        return result;
    }
};
```

###### [108. 将有序数组转换为二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)

难度：简单

自主解答：是

思路：二分法，左子数组生成左子树，用右子数组生成右子树。注意，C++中求一个vector的子序列，要在定义时用迭代器

代码：

```C++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if(nums.size()==0) return nullptr;
        int middle=nums.size()/2;
        TreeNode* root=new TreeNode(nums[middle]);
        vector<int>leftNum(nums.begin(),nums.begin()+middle);
        vector<int>rightNum(nums.begin()+middle+1,nums.end());
        root->left=sortedArrayToBST(leftNum);
        root->right=sortedArrayToBST(rightNum);
        return root;
    }
};
```

###### [98. 验证二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/validate-binary-search-tree/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：直接判断二叉树中序遍历是否递增即可。

代码：

```C++
class Solution {
public:
    void inorder(vector<int>&arr,TreeNode* node){
        if(node==nullptr)   return;
        inorder(arr,node->left);
        arr.push_back(node->val);
        inorder(arr,node->right);
    }
    bool isValidBST(TreeNode* root) {
        vector<int>array;
        inorder(array,root);
        for(int i=1;i<array.size();i++){
            if(array[i]<=array[i-1])    return false;
        }
        return true;
    }
};
```

###### [230. 二叉搜索树中第 K 小的元素 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

代码：

```C++
class Solution {
    int value;
    int count=0;
public:
    void inorder(TreeNode* root,int k){
        if(root==nullptr)   return;
        inorder(root->left,k);
        count++;
        if(count==k){
            value=root->val;
            return;
        }
        inorder(root->right,k);
    }
    int kthSmallest(TreeNode* root, int k) {
        inorder(root,k);
        return value;
    }
};
```

###### [199. 二叉树的右视图 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-right-side-view/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

思路：层序遍历就行

代码：

```C++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if(!root)   return {};
        vector<int>result;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            int size=q.size();
            while(size--){
                TreeNode* front=q.front();
                q.pop();
                if(front->left) q.push(front->left);
                if(front->right) q.push(front->right);
                if(size==0)
                    result.push_back(front->val);
            }
        }
        return result;
    }
};
```

###### [114. 二叉树展开为链表 - 力扣（LeetCode）](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：是

思路：一个pre一个cur，让pre的right指向cur然后pre=cur

代码：

```C++
class Solution {
    TreeNode* dummyNode=new TreeNode();
    TreeNode* pre=dummyNode;
public:
    void flatten(TreeNode* root) {
        if(root==nullptr)
            return;
        pre->right=root;
        pre=root;
        TreeNode* left=root->left;
        TreeNode* right=root->right;
        root->left=nullptr;
        flatten(left);
        flatten(right);
    }
};
```

#### 堆与优先级队列

###### [215. 数组中的第K个最大元素 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：优先级队列的基础题

代码：

```C++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int,vector<int>,greater<int>>pq;// 定义小顶堆
        for(int num:nums){
            pq.push(num);
            if(pq.size()>k)
                pq.pop();
        }
        return pq.top();
    }
};
```



###### [347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否

思路：这一题重点是要熟练掌握C++优先级队列的使用。priority_queue<T, vector<T>, cmp>, 其中cmp是比较函数cmp(a, b) == True说明a的优先级更低。默认情况是大顶堆，即元素越大优先级越高，cmp是less<int>函数，即a<b==True。

代码：

```C++
class Solution {
public:
    struct cmp{
        bool operator()(const pair<int,int>&a, const pair<int,int>&b){
            return a.second>b.second;// 小顶堆
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        // 默认优先级队列：大顶堆。
        //这里要用的是小顶堆，频率越小优先级越高，最后保留k个就是频率最大的
        priority_queue<pair<int,int>, vector<pair<int,int>>, cmp>pq;
        unordered_map<int,int>ump;
        for(int num:nums){
            ump[num]++;
        }
        for(auto it:ump){
            pq.push(it);
            if(pq.size()>k)
                pq.pop();
        }
        vector<int>result;
        while(!pq.empty()){
            result.push_back(pq.top().first);
            pq.pop();
        }
        return result;
    }
};
```

###### [23. 合并 K 个升序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-k-sorted-lists/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等偏难

自主解答：是

思路：使用优先级队列存储链表指针，指针元素越小优先级越高。pop堆顶指针后，需要push指针的next指针。知道堆为空，停止遍历

代码：

```C++
class Solution {
public:
    struct cmp{
        bool operator()(ListNode* a, ListNode* b){
            if(a&&b)
                return a->val>b->val;
            if(!a&&b)
                return true;
            if(a&&!b)
                return false;
            return false;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*,vector<ListNode*>,cmp>pq;
        for(auto list:lists){
            pq.push(list);
        }
        ListNode* dummyHead=new ListNode();
        ListNode* cur=dummyHead;
        while(!pq.empty()){
            ListNode* top=pq.top();
            pq.pop();
            if(top==nullptr)
                continue;
            cur->next=top;
            cur=cur->next;
            if(top->next)
                pq.push(top->next);
        }
        cur->next=nullptr;
        return dummyHead->next;
    }
};
```

代码优化：由于有空指针的情况，在写cmp比较函数的时候需要判断很多情况。可以直接让空指针不进入队列，就能解决这种情况

代码：

```C++
class Solution {
public:
    struct cmp{
        bool operator()(ListNode* a, ListNode* b){
            return a->val>b->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*,vector<ListNode*>,cmp>pq;
        for(auto list:lists){
            if(list)
                pq.push(list);
        }
        ListNode* dummyHead=new ListNode();
        ListNode* cur=dummyHead;
        while(!pq.empty()){
            ListNode* top=pq.top();
            pq.pop();
            if(top==nullptr)
                continue;
            cur->next=top;
            cur=cur->next;
            if(top->next)
                pq.push(top->next);
        }
        cur->next=nullptr;
        return dummyHead->next;
    }
};
```

#### 栈

###### [155. 最小栈 - 力扣（LeetCode）](https://leetcode.cn/problems/min-stack/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：否；二刷：是

思路：用两个栈实现，一个栈stack存储元素，另一个栈存储最小元素。类似单调栈。

代码：

```C++
class MinStack {
    stack<int>s;
    stack<int>ms;
public:
    MinStack() {
        
    }
    
    void push(int val) {
        if(ms.empty()||val<=ms.top())   ms.push(val);
        else    ms.push(ms.top());
        s.push(val);
    }
    
    void pop() {
        s.pop();
        ms.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return ms.top();
    }
};
```

进阶：不用辅助栈，额外空间O(1)完成。字节跳动常考	

难度：困难

自主解答：否；二刷：否

思路：使用一个长整形变量minVal去存储最小值。stack里存储当前值与最小值之间的差值diff=val-minVal。当元素弹出栈时，如果栈顶值diff<0，说明当前minVal就是最小值，弹出之后需要更新minVal的值，minVal需要回到上一个值lastVal。根据公式，diff=val-minVal, 即diff=curVal-lastVal，lastVal=curVal-diff。所以minVal=minVal-diff。如果diff≥0，说明栈顶不是最小值，可以直接弹出不影响minVal。

代码：

```C++
class MinStack {
    stack<long long>s;
    long long minVal;
public:
    MinStack() {
        minVal=0;
    }
    
    void push(int val) {
        if(s.empty()){
            s.push(0);
            minVal=val;
        }else{
            long long diff=val-minVal;
            if(diff<0)
                minVal=val;
            s.push(diff);
        }
    }
    
    void pop() {
        // s.top是diff。如果diff≥0，说明栈顶不是最小值，可以直接弹出不影minVal。
        // 如果diff＜0， 说明当前栈顶就是最小值，minVal需要恢复到上一个最小值
        // lastVal。根据公式，diff=val-minVal, 即diff=curVal-lastVal，
        // lastVal=curVal-diff。所以minVal=minVal-diff
        if(s.top()<0)
            minVal=minVal-s.top();
        s.pop();
    }
    
    int top() {
        if(s.top()<0)    return minVal;
        return minVal+s.top();
    }
    
    int getMin() {
        return minVal;
    }
};
```

###### [739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等偏难

自主解答：否。二刷：是

思路：这题要用单调栈，元素从栈底到栈顶单调递减。栈里存放的是元素对应的下标，这样能方便计算。

代码：

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> result(n, 0);
        stack<int> stk;
        for (int i = 0; i < n; i++) {
            while (!stk.empty() && temperatures[i]>temperatures[stk.top()]) {
                result[stk.top()] = i - stk.top();
                stk.pop();
            }
            stk.push(i);
        }
        return result;
    }
};
```

###### [496. 下一个更大元素 I - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-i/)

难度：简单

自主解答：是

思路：依旧单调栈，只不过要用一个哈希表记录每个元素的下一个更大元素的值

代码：

```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int>s;
        s.push(nums2[0]);
        unordered_map<int,int>um;
        for(int i=1;i<nums2.size();i++){
            while(!s.empty()&&s.top()<nums2[i]){
                um[s.top()]=nums2[i];
                s.pop();
            }
            s.push(nums2[i]);
        }
        vector<int>result(nums1.size(),0);
        for(int i=0;i<result.size();i++){
            result[i]=um.count(nums1[i])==0? -1:um[nums1[i]];
        }
        return result;
    }
};
```

###### [503. 下一个更大元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-ii/)

难度：中等

自主解答：是

思路：遍历两遍即可。注意元素的下标问题

代码：

```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        // 遍历两遍
        int n=nums.size();
        vector<int>result(n,-1);
        stack<int>s;
        s.push(0);
        for(int i=1;i<2*n;i++){
            while(!s.empty()&&nums[i%n]>nums[s.top()]){
                result[s.top()]=nums[i%n];
                s.pop();
            }
            s.push(i%n);
        }
        return result;
    }
};
```

###### [42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/description/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

思路：可以用单调栈，一个记录前缀最大值preMax，一个记录后缀最大值postMax。对于每一个元素i，其长度为1高度为height[i]，所能接到的雨水量为min(preMax[i], postMax[i]) - height[i]。

时间复杂度：O(n)；空间复杂度:O(1)

代码：

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        // 可以用前缀集和后缀集（其实就相当于单调栈）
        int n=height.size();
        vector<int>preMax(n),postMax(n);
        preMax[0]=height[0];
        postMax[n-1]=height[n-1];
        for(int i=1;i<n;i++)
            preMax[i]=max(preMax[i-1],height[i]);
        for(int i=n-2;i>=0;i--)
            postMax[i]=max(postMax[i+1],height[i]);
        int ans=0;
        for(int i=0;i<n;i++){
            ans+=min(preMax[i],postMax[i])-height[i];
        }
        return ans;
    }
};
```

进阶：使用O(1)时间复杂度

思路：相向双指针法，使用两个值记录preMax和postMax即可

代码：

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int n=height.size();
        int left=0,right=n-1;
        int preMax=height[0];
        int postMax=height[n-1];
        int ans=0;
        while(left<right){
            if(preMax<postMax){
                ans+=preMax-height[left];
                left++;
                preMax=max(preMax,height[left]);
            }else{
                ans+=postMax-height[right];
                right--;
                postMax=max(postMax,height[right]);
            }
        }
        return ans;
    }
};
```

###### [394. 字符串解码 - 力扣（LeetCode）](https://leetcode.cn/problems/decode-string/description/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

思路：这一题要反复刷，熟练了之后就简单了。总结就是要用两个站分别存储数字和数字之前的完整的字符串

代码：

```C++
class Solution {
public:
    string decodeString(string s) {
        string curStr;
        stack<int>numStack;
        stack<string>strStack;
        int count=0;
        for(int i=0;i<s.size();i++){
            if(s[i]<='z'&&s[i]>='a')
                curStr+=s[i];
            else if(s[i]<='9'&&s[i]>='0'){
                count=count*10+s[i]-'0';
            }else if(s[i]=='['){
                strStack.push(curStr);
                numStack.push(count);
                curStr.clear();
                count=0;
            }
            else if(s[i]==']'){
                int topNum=numStack.top();
                numStack.pop();
                string topStr=strStack.top();
                strStack.pop();
                string temp=curStr;
                while(topNum--)
                    curStr=topStr.append(temp);
            }
        }
        return curStr;
    }
};
```

#### 二分查找

###### [35. 搜索插入位置 - 力扣（LeetCode）](https://leetcode.cn/problems/search-insert-position/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

思路：二分查找，不要碰到符合条件的就return；最好要像区分红蓝边界一样，左边蓝色边界，右边红色边界。使用左开右开区间，最后结果是left指向蓝色区间的最右侧，right指向红色区间的最左侧

代码：

```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left=-1,right=nums.size();
        while(left<right-1){
            int mid=left+(right-left)/2;
            if(nums[mid]<target){
                left=mid;//(mid,right)
            }
            else{
                right=mid;//(left,mid)
            }
        }
        return right;
    }
};
```

时间复杂度: o(logn)

###### [74. 搜索二维矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/search-a-2d-matrix/submissions/724494778/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

代码：

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m=matrix.size(),n=matrix[0].size();
        int left=-1,right=m*n;
        int i=0,j=0;
        while(left<right-1){
            int mid=left+(right-left)/2;
            i=mid/n,j=mid%n;
            if(matrix[i][j]<target){
                left=mid;
            }else{
                right=mid;
            }
        }
        if(right>=m*n)
            return false;
        i=right/n,j=right%n;
        return target==matrix[i][j];
    }
};
```

###### [34. 在排序数组中查找元素的第一个和最后一个位置 - 力扣（LeetCode）](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

思路：依旧使用lower_bound的思路

代码：

```C++
class Solution {
public:
    int lower_bound(vector<int>& nums, int target){
        int left=-1,right=nums.size();
        while(left<right-1){
            int mid=left+(right-left)/2;
            if(nums[mid]<target)
                left=mid;
            else
                right=mid;
        }
        return right;
    }
    vector<int> searchRange(vector<int>& nums, int target) {
        int left_bound=lower_bound(nums,target);
        int right_bound=lower_bound(nums,target+1)-1;
        if(left_bound<=right_bound)
            return {left_bound,right_bound};
        return {-1,-1};
    }
};
```

###### [33. 搜索旋转排序数组 - 力扣（LeetCode）](https://leetcode.cn/problems/search-in-rotated-sorted-array/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否

思路：这一题比普通二分难，主要是数组会旋转，把递增数组分成了两个递增段。我们把从右边转过来的数组当做第二段，剩下的当第一段。这时候要判断mid和target是否属于同一个段。如果属于，就正常二分；否则就根据具体情况来二分

代码：

```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left=-1,right=nums.size();
        int back=nums.back();
        while(left<right-1){
            //判断nums[mid]和target是否在同一个递增段里面
            // 如果不在一个递增段里面，就
            // 反之，就按照正常二分的逻辑走
            int mid=left+(right-left)/2;
            if(nums[mid]<back&&target>back){
                // 此时mid在第一段，target在第二段。第二段就是旋转到左边的递增段
                right=mid;
            }else if(nums[mid]>back&&target<=back){
                // 此时mid在第二段，target在第一段。
                left=mid;
            }
            else if(nums[mid]<target){
                left=mid;
            }else{
                right=mid;
            }
        }
        return right<nums.size()&&nums[right]==target? right:-1;
    }
};
```

时间复杂度：O(logn)

###### [153. 寻找旋转排序数组中的最小值 - 力扣（LeetCode）](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：否

思路：找到蓝色区域和红色区域对应的逻辑就好做了

代码：

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        // 蓝色部分相当于所有都>nums.back，红色部分相当于所有都<=back
        int left=-1,right=nums.size();
        while(left<right-1){
            int mid=left+(right-left)/2;
            if(nums[mid]>nums.back())
                left=mid;
            else
                right=mid;
        }
        return nums[right];
    }
};
```

时间复杂度：O(logn)

###### [4. 寻找两个正序数组的中位数 - 力扣（LeetCode）](https://leetcode.cn/problems/median-of-two-sorted-arrays/description/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

方法一：暴力解法。把两个数组拼成一个有序的大数组，再找中位数

代码：

```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        // 暴力解法，时间复杂度O(m+n)
        int m=nums1.size(),n=nums2.size();
        vector<int>res(m+n);
        int i=0,j=0;
        for(int k=0;k<m+n;k++){
            if(j>=n||i<m&&nums1[i]<nums2[j]){
                res[k]=nums1[i++];
            }else{
                res[k]=nums2[j++];
            }
        }
        int mid=(m+n-1)/2;
        return (m+n)%2==0 ? ((double)res[mid]+res[mid+1])/2 : res[mid];
    }
};
```

时间复杂度：O(n+m)，空间复杂度：O(n+m)

方法二：二分查找法

思路：

#### 技巧

###### [136. 只出现一次的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/single-number/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：否

思路：用异或运算的性质解决。异或运算相同为0，相异为1.任何数与0异或都为本身；与自己异或都为0；而且异或运算具有交换律和结合律。所以这一题中，把所有数都互相异或，结果就是只出现了一次的数。

代码：

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result=0;
        for(int num:nums){
            result^=num;
        }
        return result;
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)

思考：如果数组中每个元素均出现三次，而只有一个元素出现一次，应该如何找到这个元素？

###### [169. 多数元素 - 力扣（LeetCode）](https://leetcode.cn/problems/majority-element/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：否

思路：像擂台战一样，谁撑到最后就胜利；

代码：

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        // 擂台战
        int maj=0,hp=0;
        for(int i=0;i<nums.size();i++){
            if(hp==0){
                maj=nums[i];
                hp=1;
            }else{
                if(maj==nums[i])
                    hp++;
                else
                    hp--;
            }
        }
        return maj;
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)

###### [75. 颜色分类 - 力扣（LeetCode）](https://leetcode.cn/problems/sort-colors/description/?envType=problem-list-v2&envId=2cktkvj)

难度：简单

自主解答：是

思路：跟前面移动零很像，使用ind记录0或1应在的位置，然后遍历交换。需要遍历两次，第一次交换0，第二次交换1

代码：

```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n=nums.size();
        int ind=0;
        for(int i=ind;i<n;i++){
            if(nums[i]==0){
                swap(nums[i],nums[ind]);
                ind++;
            }
        }
        for(int i=ind;i<n;i++){
            if(nums[i]==1){
                swap(nums[i],nums[ind]);
                ind++;
            }
        }
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)

**进阶**：只扫描一趟

自助解答：否

思路：双指针法，一个left一个right。如果i扫描到0就与left交换，扫描到2就与right交换。注意与right交换之后，i不能加一，因为这个数之前还没扫描到

代码：

```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        // 只扫描一遍
        int n=nums.size();
        int l=0,r=n-1,i=0;
        while(i<=r){
            if(nums[i]==0)
                swap(nums[i++],nums[l++]);
            else if(nums[i]==2)
                swap(nums[i],nums[r--]);
            else
                i++;
        }
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)

###### [31. 下一个排列 - 力扣（LeetCode）](https://leetcode.cn/problems/next-permutation/description/?envType=problem-list-v2&envId=2cktkvj)

难度：中等

自主解答：是

思路：这一题有一个固定的算法，需要记住。先从后往前找到第一个降序的数i。再从后往前找到第一个比i大的数j，交换i和j。最后把i后面的 所有数反转。

代码：

```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        //先从后往前，找到第一个降序的
        int n=nums.size();
        for(int i=n-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
                // 再从后往前，找到第一个比nums[i]大的，交换数字
                for(int j=n-1;j>i;j--){
                    if(nums[j]>nums[i]){
                        swap(nums[i],nums[j]);
                        break;
                    }
                }
                // reverse i后面的所有数字
                reverse(nums.begin()+i+1,nums.end());
                return;
            }
        }
        reverse(nums.begin(),nums.end());
    }
};
```

###### [287. 寻找重复数 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-duplicate-number/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

思路：类似力扣41题寻找数组中第一个缺失的正数，使用原地哈希法

代码：

```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        // 原地哈希法，类似寻找数组中第一个缺失的正数
        int n=nums.size();
        for(int i=0;i<n;i++){
            int targetInd=nums[i]-1;
            while(nums[i]!=i+1){
                if(nums[i]==nums[targetInd]){
                    return nums[i];
                }
                swap(nums[i],nums[targetInd]);
                targetInd=nums[i]-1;
            }
        }
        return 0;
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)

**进阶**：上述算法虽然时空复杂度都符合要求，但是没有满足不修改数组这个要求

思路：使用类似链表法，i从0开始，i->next=nums[i], nums[i]->next=nums[nums[i]]这样连接下去。因为nums[i]范围都在[1,n-1]所以不会出现数组越界；链表中一定会出现环（这一点证明我还没搞清楚）。有了环之后，就能用寻找环形链表入环口的思路了，即快慢指针法

代码：

```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        // fast->next=nums[fast];slow->next=nums[slow]
        int fast=0,slow=0;
        while(1){
            slow=nums[slow];
            fast=nums[nums[fast]];
            if(slow==fast)
                break;
        }
        int head=0;
        while(head!=slow){
            head=nums[head];
            slow=nums[slow];
        }
        return head;
    }
};
```

时间复杂度：O(n)；空间复杂度：O(1)。且没有修改数组

#### 回溯算法

###### [46. 全排列 - 力扣（LeetCode）](https://leetcode.cn/problems/permutations/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：否；二刷：是

思路：用一个数组记录某个num是否在path里。如果在的话，for循环中就直接跳过了。

代码：

```C++
class Solution {
public: 
    void backtracking(const vector<int>& nums, vector<vector<int>>&result,vector<int>&path,vector<int>&onPath){
        if(path.size()==nums.size()){
            result.push_back(path);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(onPath[i]==1)
                continue;
            path.push_back(nums[i]);
            onPath[i]=1;
            backtracking(nums,result,path,onPath);
            onPath[i]=0;
            path.pop_back();
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>>result;
        vector<int>path;
        vector<int>onPath(nums.size(),0);
        backtracking(nums,result,path,onPath);
        return result;
    }
};
```

###### [78. 子集 - 力扣（LeetCode）](https://leetcode.cn/problems/subsets/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

代码：

```C++
class Solution {
public:
    void backtracking(const vector<int>& nums, vector<vector<int>>&result,vector<int>&path,int startInd){
        result.push_back(path);
        for(int i=startInd;i<nums.size();i++){
            path.push_back(nums[i]);
            backtracking(nums,result,path,i+1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>>result;
        vector<int>path;
        backtracking(nums,result,path,0);
        return result;
    }
};
```

###### [17. 电话号码的字母组合 - 力扣（LeetCode）](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

代码：

```C++
class Solution {
public:
    void dfs(const vector<string>& strs, vector<string>& result, string& path,
             int startInd) {
        if (path.size() == strs.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < strs[startInd].size(); i++) {
            path += strs[startInd][i];
            dfs(strs, result, path, startInd + 1);
            path.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        vector<string> dict = {"",    "",    "abc",  "def", "ghi",
                               "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> result;
        string path;
        vector<string> strs;
        for (char c : digits) {
            strs.push_back(dict[c - '0']);
        }
        dfs(strs,result,path,0);
        return result;
    }
};
```

###### [39. 组合总和 - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

代码：

```C++
class Solution {
public:
    void dfs(const vector<int>& nums, vector<vector<int>>&result,vector<int>&path,int startInd,int sum,int target){
        if(sum>=target){
            if(sum==target)
                result.push_back(path);
            return;
        }
        for(int i=startInd;i<nums.size();i++){
            path.push_back(nums[i]);
            sum+=nums[i];
            dfs(nums,result,path,i,sum,target);
            sum-=nums[i];
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>>result;
        vector<int>path;
        dfs(candidates,result,path,0,0,target);
        return result;
    }
};
```

时间复杂度：O(2^n)

###### [22. 括号生成 - 力扣（LeetCode）](https://leetcode.cn/problems/generate-parentheses/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

代码：

```C++
class Solution {
public:
    void dfs(vector<string>&result,string &path,int sum,int n){
        if(sum<0||sum>n||path.size()==n*2){
            if(sum==0)
                result.push_back(path);
            return;
        }
        {
            path+='(';
            dfs(result,path,sum+1,n);
            path.pop_back();
        }
        {
            path+=')';
            dfs(result,path,sum-1,n);
            path.pop_back();
        }
    }
    vector<string> generateParenthesis(int n) {
        vector<string>result;
        string path;
        dfs(result,path,0,n);
        return result;
    }
};
```

时间复杂度：O(2^2n)

这一题貌似还有更好的思路。用left和right表示左括号和右括号。这里先不展开了

###### [79. 单词搜索 - 力扣（LeetCode）](https://leetcode.cn/problems/word-search/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：否；二刷：是

思路：这里使用回溯法来代替visited数组。当遍历到一个节点的时候使其变成0，防止进入无限循环。后续再恢复原有数值，这就是回溯的思想

代码：

```C++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        int m=board.size(),n=board[0].size();
        bool result=false;
        int dir[4][2]={{0,1},{0,-1},{1,0},{-1,0}};
        auto dfs=[&](this auto&&dfs,int i,int j,int k)->void{
            if(i<0||j<0||i>=m||j>=n||board[i][j]!=word[k]||board[i][j]==0)
                return;
            if(board[i][j]==word[k]&&k==word.size()-1){
                result=true;
                return;
            }
            k++;
            board[i][j]=0;
            for(int h=0;h<4;h++){
                int i_h=i+dir[h][0];
                int j_h=j+dir[h][1];
                dfs(i_h,j_h,k);
            }
            // 回溯
            k--;
            board[i][j]=word[k];
        };
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]==word[0])
                    dfs(i,j,0);
            }
        }
        return result;
    }
};
```

这一题和图论中的岛屿问题很像。二刷的时候我用一个visited数组来防止遍历回去无限循环。但是这里和岛屿问题不同的是，visited数组最后要回溯回去。

代码如下：

```C++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        int m=board.size(),n=board[0].size();
        int dir[4][2]={0,1,0,-1,1,0,-1,0};
        bool result=false;
        vector<vector<bool>>visited(m,vector<bool>(n,0));
        auto dfs=[&](this auto&& dfs,int i,int j,int h)->void{
            if(i<0||j<0||i>=m||j>=n||board[i][j]!=word[h]||visited[i][j])
                return;
            if(h==word.size()-1){
                result=true;
                return;
            }
            visited[i][j]=1;
            for(int k=0;k<4;k++){
                int next_i=i+dir[k][0],next_j=j+dir[k][1];
                dfs(next_i,next_j,h+1);
            }
            visited[i][j]=0;
        };
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]==word[0]){
                    dfs(i,j,0);
                }
            }
        }
        return result;
    }
};
```

###### [131. 分割回文串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-partitioning/description/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否；二刷：是

思路：想象一个长为n的字符串，有n个间隔。比如abc的间隔是a|b|c|。这一题相当于选间隔的组合问题。如果选第一个间隔就分割成a|bc，选第二个就是ab|c，选第三个就是abc|。上面是每一层的操作。a|bc这个节点往下递归，就是从b后面的这个间隔开始选，分别得到a|b|c和a|bc|；前者继续递归得到a|b|c|。所以这里循环的时候，也有一个startInd，而且切分都是从startInd开始，子串是[startInd,i]的左闭右闭区间。当startInd==s.length()的时候递归结束。

时间复杂度：O(n*2^n)

代码：

```C++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>>result;
        vector<string>path;
        auto isParlindrome=[&](int l,int r)->bool{
            if(l>r)
                return false;
            while(l<=r){
                if(s[l]==s[r]){
                    l++;
                    r--;
                }else{
                    return false;
                }
            }
            return true;
        };
        auto dfs=[&](this auto&&dfs,int startInd){
            //终止条件
            if(startInd>=s.length()){	
                result.push_back(path);
                return;
            }
            for(int i=startInd;i<s.length();i++){
                if(isParlindrome(startInd,i)){
                    string sub=s.substr(startInd,i-startInd+1);
                    path.push_back(sub);
                }else{
                    continue;
                }
                dfs(i+1);
                path.pop_back();
            }
        };
        dfs(0);
        return result;
    }
};
```

动态规划优化：判断s[startInd...i]是否为回文子串，可以用动态规划实现

时间复杂度：O(n*2^n)。注意这里的n是因为string sub=s.substr(startInd,i-startInd+1)和result.push_back(path)这两步都是O(n)复杂度

代码：

```C++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>>result;
        vector<string>path;
        int n=s.size();
        vector<vector<bool>>dp(n,vector<bool>(n,0));// dp数组表示s[i...j]是否是一个回文串
        for(int i=0;i<n;i++)
            dp[i][i]=1;
        for(int i=n-1;i>=0;i--){
            for(int j=i+1;j<n;j++){
                if(j==i+1)
                    dp[i][j]=s[i]==s[j];
                else
                    dp[i][j]=s[i]==s[j]&&dp[i+1][j-1];
            }
        }
        auto dfs=[&](this auto&&dfs,int startInd){
            //终止条件
            if(startInd>=s.length()){
                result.push_back(path);
                return;
            }
            for(int i=startInd;i<s.length();i++){
                if(dp[startInd][i]){
                    string sub=s.substr(startInd,i-startInd+1);
                    path.push_back(sub);
                }else{
                    continue;
                }
                dfs(i+1);
                path.pop_back();
            }
        };
        dfs(0);
        return result;
    }
};
```

###### [51. N 皇后 - 力扣（LeetCode）](https://leetcode.cn/problems/n-queens/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否；二刷：否

思路：依旧回溯算法。递归深度为n，每一层也循环n次，其中n为棋盘的size。每次要判断一个点变成Queen的话是否是valid的，如果是的话就修改board状态并深入一层递归。

时间复杂度：O(n!)

代码：

```C++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>>result;
        vector<string>board(n,string(n,'.'));
        auto dfs=[&](this auto&&dfs,int i){
            // i是当前行，j是当前列
            if(i==n){
                result.push_back(board);
                return;
            }
            for(int j=0;j<n;j++){
                if(isValidBoard(board,i,j)){
                    board[i][j]='Q';
                    dfs(i+1);
                    board[i][j]='.';
                }
            }
        };
        dfs(0);
        return result;
    }
    bool isValidBoard(vector<string>&board, int row,int col){
        int dir[4][2]={{1,1},{-1,-1},{1,-1},{-1,1}};
        int n=board.size();
        for(int i=0;i<n;i++){
            if(board[i][col]=='Q')
                return false;
        }
        for(int j=0;j<n;j++){
            if(board[row][j]=='Q')
                return false;
        }
        // 检查斜线，四个方向右下，左上，右上，左下
        for(int k=0;k<4;k++){
            int dr=dir[k][0];
            int dc=dir[k][1];
            int new_r=row+dr;
            int new_c=col+dc;
            while(new_r>=0&&new_r<n&&new_c>=0&&new_c<n){
                if(board[new_r][new_c]=='Q')
                    return false;
                new_r+=dr;
                new_c+=dc;
            }
        }
        return true;
    }
};
```

这一题然后可以优化。本质上N皇后问题和全排列是非常类似的问题，因为全排列是选了一个元素之后，这个元素在深度递归中就不能再选了，我们会用一个onPath数组记录某个元素是否被选；其实N皇后也一样，某一行的一列有了一个皇后之后，后面的行的相同列就不能再出现皇后了。这个在复习时可以实现

二刷心得：使用unordered_set来代替onpath数组，这样更容易理解，只是开销稍微大一点。要使用三个集合来存储，分别存列、主对角线和副对角线的值。

代码：

```C++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>>result;
        vector<string>board(n,string(n,'.'));
        unordered_set<int>colomns;
        unordered_set<int>diag1;
        unordered_set<int>diag2;
        auto dfs=[&](this auto&&dfs,int row)->void{
            if(row==n){
                result.push_back(board);
                return;
            }
            for(int col=0;col<n;col++){
                if(colomns.contains(col)||diag1.contains(row+col)||diag2.contains(row-col))
                    continue;
                colomns.insert(col);
                diag1.insert(row+col);
                diag2.insert(row-col);
                board[row][col]='Q';
                dfs(row+1);
                colomns.erase(col);
                diag1.erase(row+col);
                diag2.erase(row-col);
                board[row][col]='.';
            }
        };
        dfs(0);
        return result;
    }
};
```

#### 动态规划

###### [118. 杨辉三角 - 力扣（LeetCode）](https://leetcode.cn/problems/pascals-triangle/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

时间复杂度：O(n*n)

代码：

```C++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>>result(numRows);
        for(int i=0;i<numRows;i++){
            result[i].assign(i+1,1);
            if(i==0)
                continue;
            for(int j=1;j<i;j++){
                result[i][j]=result[i-1][j-1]+result[i-1][j];
            }
        }
        return result;
    }
};
```

###### [198. 打家劫舍 - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber/description/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

思路：一定要明确dp数组的定义。这一题的dp[i]就表示从0-i-1的房屋能偷到的最大金额。所以最后只用输出dp[n-1]就行了，不用来一个result记录最大值

时间复杂度：O(n)

代码：

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if(n==1)
            return nums[0];
        vector<int>dp(n,0);
        dp[0]=nums[0];
        dp[1]=max(nums[0],nums[1]);
        int result=dp[1];
        for(int i=2;i<n;i++){
            dp[i]=max(nums[i]+dp[i-2],dp[i-1]);
            result=max(result,dp[i]);
        }
        return result;
    }
};
```

###### [62. 不同路径 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths/description/)

难度：简单

自主解答：是

时间复杂度：O(n^2)

代码：

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>>dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++)
            dp[i][0]=1;
        for(int j=0;j<n;j++)
            dp[0][j]=1;
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

###### [63. 不同路径 II - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths-ii/)

难度：简单

自主解答：是

时间复杂度：O(n^2)

代码：

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m=obstacleGrid.size(),n=obstacleGrid[0].size();
        vector<vector<int>>dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++){
            if(obstacleGrid[i][0]==0)
                dp[i][0]=1;
            else
                break;
        }
        for(int j=0;j<n;j++){
            if(obstacleGrid[0][j]==0)
                dp[0][j]=1;
            else
                break;
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(obstacleGrid[i][j]==1)
                    dp[i][j]=0;
                else
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

###### [64. 最小路径和 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-path-sum/?envType=study-plan-v2&envId=top-100-liked)

难度：简单

自主解答：是

思路：跟上面的挺相似的

时间复杂度：O(m*n)

代码：

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<vector<int>>dp(m,vector<int>(n,0));
        dp[0][0]=grid[0][0];
        for(int j=1;j<n;j++){
            dp[0][j]=dp[0][j-1]+grid[0][j];
        }
        for(int i=1;i<m;i++)
            dp[i][0]=dp[i-1][0]+grid[i][0];
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```

###### [96. 不同的二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-binary-search-trees/description/)

难度：简单

自主解答：是

时间复杂度：O(n^2)

代码：

```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int>dp(n+1,0);
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            for(int j=0;j<i;j++){
                dp[i]+=dp[j]*dp[i-j-1];
            }
        }
        return dp[n];
    }
};
```

##### 01背包问题

###### [46. 携带研究材料（第六期模拟笔试）](https://kamacoder.com/problempage.php?pid=1046)

这一题为01背包问题基础题

难度：中等

自主解答：否；二刷：是

思路：01背包问题，这里用二维数组解决，其中dp(i,j)表示物品0~i和背包重量为j时，所能装载的最大价值。递推公式要牢记

代码：

```C++
#include<iostream>
#include<vector>
using namespace std;
int main(){
    int m,n;
    cin>>m>>n;
    vector<int>weight(m,0);
    vector<int>value(m,0);
    for(int i=0;i<m;i++)
        cin>>weight[i];
    for(int i=0;i<m;i++)
        cin>>value[i];
    vector<vector<int>>dp(m,vector<int>(n+1,0));
    //dp[i][j]表示物品0~i，背包容量为j时所能获得的最大价值
    //dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i]]+value[i])
    for(int j=0;j<=n;j++){
        if(j>=weight[0])
            dp[0][j]=value[0];
        else
            dp[0][j]=0;
    }
    for(int i=1;i<m;i++){
        for(int j=0;j<=n;j++){
            if(j<weight[i])
                dp[i][j]=dp[i-1][j];
            else
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i]]+value[i]);
        }
    }
    cout<<dp[m-1][n];
    return 0;
}
```

也可以使用一维dp数组节省空间，但注意此时遍历j时要从大到小，这是为了避免在同一层遍历时后面的列会用到前面的列的数据

代码：

```C++
#include<iostream>
#include<vector>
using namespace std;
int main(){
    int m,n;
    cin>>m>>n;
    vector<int>weight(m,0);
    vector<int>value(m,0);
    for(int i=0;i<m;i++)
        cin>>weight[i];
    for(int i=0;i<m;i++)
        cin>>value[i];
    vector<int>dp(n+1,0);
    for(int i=0;i<m;i++){
        for(int j=n;j>=weight[i];j--){
            dp[j]=max(dp[j],dp[j-weight[i]]+value[i]);
        }
    }
    cout<<dp[n];
    return 0;
}
```

###### [416. 分割等和子集 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-equal-subset-sum/)

难度：中等

自主解答：是

思路：看作01背包问题，其中物品的重量和价值相等。最后只需要判断背包的最大价值是否和最大重量相等即可

代码：

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum=0;
        for(int num:nums)
            sum+=num;
        if(sum%2!=0)
            return false;
        int capacity=sum/2;
        int m=nums.size();
        vector<int>dp(capacity+1,0);
        for(int i=0;i<m;i++){
            for(int j=capacity;j>=nums[i];j--){
                dp[j]=max(dp[j],dp[j-nums[i]]+nums[i]);
            }
        }
        return dp[capacity]==capacity;
    }
};
```

###### [1049. 最后一块石头的重量 II - 力扣（LeetCode）](https://leetcode.cn/problems/last-stone-weight-ii/)

难度：中等

自主解答：是

思路：这一题跟上一题类似

代码：

```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum=0;
        for(int stone :stones){
            sum+=stone;
        }
        int capacity=sum/2;
        int m=stones.size();
        vector<int>dp(capacity+1,0);
        for(int i=0;i<m;i++){
            for(int j=capacity;j>=stones[i];j--){
                dp[j]=max(dp[j],stones[i]+dp[j-stones[i]]);
            }
        }
        return sum-2*dp[capacity];
    }
};
```

###### [494. 目标和 - 力扣（LeetCode）](https://leetcode.cn/problems/target-sum/)

难度：简单

自主解答：是

思路：这一题第一眼是排列组合问题，可以用回溯法解决

代码：

```C++
class Solution {
public:
    void dfs(const vector<int>& nums, int target,int sum,int &result,int startIndex){
        if(sum==target){
            result++;
        }
        for(int i=startIndex;i<nums.size();i++){
            sum+=2*nums[i];
            dfs(nums,target,sum,result,i+1);
            sum-=2*nums[i];
        }
    }
    int findTargetSumWays(vector<int>& nums, int target) {
        int result=0,sum=0;
        for(int num:nums)
            sum+=num;
        sum*=-1;
        dfs(nums,target,sum,result,0);
        return result;
    }
};
```

下面是动态规划解法

难度：中等偏难

自主解答：否；二刷：是

思路：依旧转化为01背包问题，只不过这次求的是装满背包有多少种方式。dp(i,j)表示0~i的物品在容量为j的背包下有多少种装满的方式，dp(i,j)=dp(i-1,j)+dp(i-1,j-weight[i])

代码：

```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        //设所有正数和为pos，所有负数和的绝对值为nag
        //pos+nag=sum,pos-nag=target-->pos=(sum+target)/2;
        // 转化为背包问题。要装满容量为pos的背包，一共有多少种方法？
        // dp[i][j]表示物品0~i，装满容量j的背包的方法数，则dp[i][j]=dp[i-1][j]+dp[i-1][j-weight[i]]
        int sum=0;
        for(int num:nums)
            sum+=num;
        if((sum+target)%2!=0||(sum+target)<0)
            return 0;
        int capiticity=(sum+target)/2;
        vector<int>dp(capiticity+1,0);
        dp[0]=1;
        for(int i=0;i<nums.size();i++){
            for(int j=capiticity;j>=nums[i];j--)
                dp[j]+=dp[j-nums[i]];
        }
        return dp[capiticity];
    }
};
```

###### [474. 一和零 - 力扣（LeetCode）](https://leetcode.cn/problems/ones-and-zeroes/)

难度：中等

自主解答：是

思路：二维的01背包问题，背包的重量有两个维度为别是0的数量和1的数量。其他的就按照01背包来做就可以了

代码：

```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        //依旧01背包，其中背包容量为m个0和n个1，物品就是字符串，每个物品的价值视作1，求最大价值
        //dp[i][j]=max(dp[i][j],dp[i-x][j-y]+1),其中x，y是第k个字符串中0和1的数量
        vector<vector<int>>dp(m+1,vector<int>(n+1,0));
        for(int k=0;k<strs.size();k++){
            int x=0,y=0;
            for(char s:strs[k]){
                if(s=='0') x++;
                else y++;
            }
            for(int i=m;i>=x;i--){
                for(int j=n;j>=y;j--){
                    dp[i][j]=max(dp[i][j],dp[i-x][j-y]+1);
                }
            }
        }
        return dp[m][n];
    }
};
```

##### 完全背包问题

###### [52. 携带研究材料（第七期模拟笔试）](https://kamacoder.com/problempage.php?pid=1052)

这一题是完全背包问题，用二维dp数组解决时，和01背包仅有初始化和递推公式上的差别

难度：简单

自主解答：是

代码：

```C++
#include<iostream>
#include<vector>
using namespace std;
int main(){
    int n,capacity;
    cin>>n>>capacity;
    vector<int>weight(n,0);
    vector<int>value(n,0);
    for(int i=0;i<n;i++){
        cin>>weight[i]>>value[i];
    }
    vector<vector<int>>dp(n,vector<int>(capacity+1,0));
    //dp[i][j]表示物品0~i，背包容量为j时所能获得的最大价值
    //dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i]]+value[i])
    // 初始化第一行i=0的数据
    for(int j=weight[0];j<=capacity;j++){
        dp[0][j]=dp[0][j-weight[0]]+value[0];
    }
    for(int i=1;i<n;i++){
        for(int j=0;j<=capacity;j++){
            if(j<weight[i])
                dp[i][j]=dp[i-1][j];
            else
                dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i]]+value[i]);
        }
    }
    cout<<dp[n-1][capacity];
    return 0;
}
```

如果用一维dp数组，和01背包的唯一区别就是对j的遍历时从小到大的。为什么这样自己领悟吧

代码：

```C++
#include<iostream>
#include<vector>
using namespace std;
int main(){
    int n,capacity;
    cin>>n>>capacity;
    vector<int>weight(n,0);
    vector<int>value(n,0);
    for(int i=0;i<n;i++){
        cin>>weight[i]>>value[i];
    }
    //dp[i][j]表示物品0~i，背包容量为j时所能获得的最大价值
    //dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i]]+value[i])
    // 初始化第一行i=0的数据
    vector<int>dp(capacity+1,0);
    for(int i=0;i<n;i++){
        for(int j=weight[i];j<=capacity;j++){
            dp[j]=max(dp[j],dp[j-weight[i]]+value[i]);
        }
    }
    cout<<dp[capacity];
    return 0;
}
```

###### [518. 零钱兑换 II - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change-ii/)

难度：中等

自主解答：是

思路：完全背包问题，求装满背包的方法，注意二维dp数组第一列都初始化为1

代码：

```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        // 这一题是完全背包问题，求装满背包的方法，amount就是背包容量
        // 递推公式：dp[i][j]=dp[i-1][j]+dp[i][j-weight[i]]
        int n=coins.size();
        vector<uint32_t>dp(amount+1,0);
        dp[0]=1;
        for(int i=0;i<n;i++){
            for(int j=coins[i];j<=amount;j++){
                dp[j]+=dp[j-coins[i]];
            }
        }
        return dp[amount];
    }
};
```

###### [377. 组合总和 Ⅳ - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iv/description/)

难度：中等

自主解答：否；二刷：是

思路：这一题是排列问题，求完全背包中装满背包的排列。其实这一题和爬楼梯是等价的，楼梯长度相当于是背包容量，而一次能爬的距离相当于物品的重量。在递推中，要先遍历楼梯长度，再遍历物品重量

代码：

```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        // dp[i]表示到target=i有多少种不同的排列
        // dp[i]=dp[i-1]+dp[i-2]+dp[i-3]
        int n=nums.size();
        vector<uint32_t>dp(target+1,0);
        dp[0]=1;
        for(int i=0;i<=target;i++){
            for(int j=0;j<nums.size();j++){
                if(i>=nums[j])
                    dp[i]+=dp[i-nums[j]];
            }
        }
        return dp[target];
    }
};
```

###### [322. 零钱兑换 - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change/)

难度：中等

自主解答：是

思路：想清楚dp数组含义和递推公式，这一题就简单了。依旧完全背包

代码：

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        // dp[i][j]表示物品0~i，背包容量j时，装满背包所需的最少物品个数
        // dp[i][j]= min(dp[i-1][j],dp[i][j-weight[i]]+1)
        // 初始化
        int n = coins.size();
        vector<uint32_t> dp(amount + 1, INT_MAX);
        dp[0]=0;
        for (int i = 0; i < n; i++) {
            for(int j=coins[i];j<=amount;j++)
                dp[j]=min(dp[j],dp[j-coins[i]]+1);
        }
        return dp[amount]==INT_MAX ? -1 : dp[amount];
    }
};
```

###### [279. 完全平方数 - 力扣（LeetCode）](https://leetcode.cn/problems/perfect-squares/)

难度：中等

自主解答：是

思路：这一题和上一题基本一样

代码：

```C++
class Solution {
public:
    int numSquares(int n) {
        // 依旧完全背包,dp[i][j]=min(dp[i-1][j],dp[i][j-weight[i]]+1);
        vector<int>dp(n+1,INT_MAX);
        dp[0]=0;
        for(int i=1;i<=100;i++){
            for(int j=i*i;j<=n;j++){
                dp[j]=min(dp[j],dp[j-i*i]+1);
            }
        }
        return dp[n];
    }
};
```

###### [139. 单词拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/word-break/description/)

难度：困难

自主解答：否；二刷：是

思路：这一题很难。要刷3-4编才能完全掌握。使用爬楼梯的思路，比完全背包的思路更好

代码：

```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        // dp[i]表示s[0~i-1]是否能从wordDict拼接而来
        // 递推公式：dp[i]=dp[j] && (s[j~i-1] in wordDict for j in (0, i))
        int n=s.size();
        vector<bool>dp(n+1,false);
        dp[0]=true;
        for(int i=1;i<=n;i++){
            // 判断
            for(int j=0;j<i;j++){
                string subStr=s.substr(j,i-j);
                if(dp[j]&&find(wordDict.begin(),wordDict.end(),subStr)!=wordDict.end()){
                    dp[i]=true;
                    break;
                }
            }
        }
        return dp[n];
    }
};
```

##### 子序列问题

###### [300. 最长递增子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-increasing-subsequence/)

难度：中等

自主解答：否；二刷：是

思路：正常解法要用两次遍历才能解决

代码：

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        //dp[i]表示以nums[i]结尾的最长递增子序列长度
        int n=nums.size();
        vector<int>dp(n,1);
        int maxLen=0;
        for(int i=0;i<n;i++){
            for(int j=0;j<i;j++){
                if(nums[i]>nums[j]){
                    dp[i]=max(dp[i],dp[j]+1);
                }
            }
            maxLen=max(maxLen,dp[i]);
        }
        return maxLen;
    }
};
```

###### [674. 最长连续递增序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

难度：简单

自主解答：是

代码：

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n=nums.size();
        vector<int>dp(n,0);
        dp[0]=1;
        int result=1;
        for(int i=1;i<n;i++){
            if(nums[i]>nums[i-1])
                dp[i]=dp[i-1]+1;
            else
                dp[i]=1;
            result=max(result,dp[i]);
        }
        return result;
    }
};
```

其实这一题用不着dp数组，直接用一个int类型变量记录就行，代码如下：

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int len=1,result=1;
        for(int i=1;i<nums.size();i++){
            if(nums[i]>nums[i-1])
                len++;
            else
                len=1;
            result=max(result,len);
        }
        return result;
    }
};
```

###### [718. 最长重复子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

难度：中等

自主解答：是

代码：

```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        //dp[i][j]表示以nums1[i]，nums2[j]结尾的最长重复子数组长度
        int m=nums1.size(),n=nums2.size();
        vector<vector<int>>dp(m+1,vector<int>(n+1,0));
        int result=0;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(nums1[i-1]==nums2[j-1])
                    dp[i][j]=dp[i-1][j-1]+1;
                result=max(result,dp[i][j]);
            }
        }
        return result;
    }
};
```

###### [1143. 最长公共子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-common-subsequence/)

难度：中等

自主解答：是

代码：

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m=text1.size(),n=text2.size();
        vector<vector<int>>dp(m+1,vector<int>(n+1));
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(text1[i-1]==text2[j-1])
                    dp[i][j]=dp[i-1][j-1]+1;
                else
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[m][n];
    }
};
```

###### [1035. 不相交的线 - 力扣（LeetCode）](https://leetcode.cn/problems/uncrossed-lines/description/)

难度：简单

自主解答：是

思路：跟上一题一模一样，就是求最长公共子序列

代码：

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m=nums1.size(),n=nums2.size();
        vector<vector<int>>dp(m+1,vector<int>(n+1));
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(nums1[i-1]==nums2[j-1])
                    dp[i][j]=dp[i-1][j-1]+1;
                else
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[m][n];
    }
};
```

###### [53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/description/)

难度：简单

自主解答：是

思路：贪心算法，很简单

代码：

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 先用贪心
        int sum=0,maxSum=INT_MIN;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
            maxSum=max(maxSum,sum);
            if(sum<0)
                sum=0;
        }
        return maxSum;
    }
};
```

也可以用动态规划，依旧不难

代码：

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // dp[i]表示以nums[i]结尾的子数组的最大和
        int n=nums.size();
        vector<int>dp(n,0);
        dp[0]=nums[0];
        int result=dp[0];;
        for(int i=1;i<n;i++){
            if(dp[i-1]<0)
                dp[i]=nums[i];
            else
                dp[i]=nums[i]+dp[i-1];
            result=max(result,dp[i]);
        }
        return result;
    }
};
```

###### [392. 判断子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/is-subsequence/description/)

难度：中等

自主解答：是

思路：这一题和最长公共子序列等价

代码：

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        //直接判断最长公共子序列是否和s相等即可
        //dp[i][j]表示以s[i]结尾t[j]结尾的最长公共子序列的长度
        int m=s.size(),n=t.size();
        vector<vector<int>>dp(m+1,vector<int>(n+1,0));
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(s[i-1]==t[j-1])
                    dp[i][j]=dp[i-1][j-1]+1;
                else
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return m==dp[m][n];
    }
};
```

###### [115. 不同的子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/distinct-subsequences/description/)

难度：困难

自主解答：否；二刷：是

思路：这一题我想到了正确的dp数组定义和递推公式，但是在初始化这一步弄错了。dp数组的第一列应该全部初始化成1，相当于一个数组一定包含一个空数组。一定要注意i和j不能超过字符串的边界，在if语句中是i-1和j-1

代码：

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        int m=s.size(),n=t.size();
        vector<vector<unsigned>>dp(m+1,vector<unsigned>(n+1));
        for(int i=0;i<=m;i++)
            dp[i][0]=1;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(s[i-1]==t[j-1])
                    dp[i][j]=dp[i-1][j-1]+dp[i-1][j];
                else
                    dp[i][j]=dp[i-1][j];
                //cout<<dp[i][j]<<" ";
            }
            //cout<<endl;
        }
        return dp[m][n];
    }
};
```

###### [583. 两个字符串的删除操作 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-operation-for-two-strings/)

难度：困难

自主解答：是

思路：如果想明白dp数组的定义和递推公式就不难了。一定要注意循环中是判断word1(i-1)和word2(j-1)，别忘了减1

代码：

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m=word1.size(),n=word2.size();
        vector<vector<int>>dp(m+1,vector<int>(n+1,0));
        for(int i=0;i<=m;i++)
            dp[i][0]=i;
        for(int j=0;j<=n;j++)
            dp[0][j]=j;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(word1[i-1]==word2[j-1])
                    dp[i][j]=dp[i-1][j-1];
                else
                    dp[i][j]=min(dp[i-1][j],dp[i][j-1])+1;
            }
        }
        return dp[m][n];
    }
};
```

这一题还有个简单的思路，就是先求最长公共子序列的长度，后面就简单了。因为要两个字符串变成相同，最少步数一定是变成最长公共子序列。

###### [72. 编辑距离 - 力扣（LeetCode）](https://leetcode.cn/problems/edit-distance/description/)

难度：困难

自主解答：是

思路：这一题跟上一题基本上没有区别，就是递推公式中如果word1[i-1]!=word2[j-1]时，增加一个选项dp(i-1)(j-1)+1，这是因为可以替换

代码：

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m=word1.size(),n=word2.size();
        vector<vector<int>>dp(m+1,vector<int>(n+1,0));
        for(int i=0;i<=m;i++)
            dp[i][0]=i;
        for(int j=0;j<=n;j++)
            dp[0][j]=j;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(word1[i-1]==word2[j-1])
                    dp[i][j]=dp[i-1][j-1];
                else
                    dp[i][j]=min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;
            }
        }
        return dp[m][n];
    }
};
```

###### [647. 回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindromic-substrings/)

难度：中等

自主解答：是

思路：dp(i,j)=dp(i+1,j-1)，由此可知i需要从大到小遍历

代码：

```C++
class Solution {
public:
    int countSubstrings(string s) {
        int n=s.size();
        vector<vector<bool>>dp(n,vector<bool>(n,false));
        int result=0;
        for(int i=0;i<n;i++)
            dp[i][i]=true;
        for(int i=n-1;i>=0;i--){
            for(int j=i+1;j<n;j++){
                if(s[i]==s[j]){
                    if(j-i==1)
                        dp[i][j]=true;
                    else if(j-i>1)
                        dp[i][j]=dp[i+1][j-1];
                }
                result+=dp[i][j];
            }
        }
        return result+n;
    }
};
```

###### [516. 最长回文子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-subsequence/)

难度：中等

自主解答：是

思路：dp(i,j)表示s[i...j]的最长回文子序列长度。dp(i,j) = dp(i+1,j-1)+2 if s[i]==s[j], else max(dp(i+1,j),dp(i,j-1))

代码：

``` C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n=s.size();
        vector<vector<int>>dp(n,vector<int>(n,0));
        for(int i=n-1;i>=0;i--){
            for(int j=i;j<n;j++){
                if(s[i]==s[j]){
                    if(i==j)
                        dp[i][j]=1;
                    else
                        dp[i][j]=dp[i+1][j-1]+2;
                }else
                    dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
            }
        }
        return dp[0][n-1];
    }
};
```

###### [5. 最长回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-substring/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：是

思路：动态规划法，先求s[i..j]是不是回文的，再求长度

时间复杂度：O(n^2)

代码：

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        // 动态规划法，先求s[i..j]是不是回文的，再求长度
        int n=s.size(),left=0,len=0;
        vector<vector<bool>>dp(n,vector<bool>(n,0));
        for(int i=n-1;i>=0;i--){
            for(int j=i;j<n;j++){
                if(s[i]==s[j]&&j-i<=1)
                    dp[i][j]=true;
                else if(s[i]==s[j]&&j-i>1)
                    dp[i][j]=dp[i+1][j-1];
                else
                    dp[i][j]=false;
                if(dp[i][j]&&len<j-i+1){
                    len=j-i+1;
                    left=i;
                }
            }
        }
        return s.substr(left,len);
    }
};
```

###### [152. 乘积最大子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-product-subarray/?envType=study-plan-v2&envId=top-100-liked)

难度：中等

自主解答：否；二刷：是

思路：我想到了用动态规划dp数组来存储每一个子数组的乘积，然后去找最大的，但是最后超时了，通过了95%的用例

时间复杂度：O(n^2)

代码：

```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int result=INT_MIN, n=nums.size();
        vector<vector<int>>dp(n,vector<int>(n,1));
        for(int i=n-1;i>=0;i--){
            for(int j=i;j<n;j++){
                if(i==j)
                    dp[i][j]=nums[i];
                else
                    dp[i][j]=dp[i+1][j-1]*nums[i]*nums[j];
                result=max(result,dp[i][j]);
            }
        }
        return result;
    }
};
```

正确解法为，用一个dp数组记录每个元素结尾的子数组的最大乘积和最小乘积。因为最小乘积乘以一个负数就变成了最大的，最大乘积乘以一个负数就变成了最小的，所以最小值和最大值都要记录。

时间复杂度：O(n)

代码：

```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        // dp[i][]表示以nums[i]结尾的子数组的乘积最值；dp[i][0]是最小值，dp[i][1]是最大值
        int n=nums.size();
        vector<vector<int>>dp(n,vector<int>(2,0));
        dp[0][0]=nums[0];
        dp[0][1]=nums[0];
        int result=nums[0];
        for(int i=1;i<n;i++){
            if(nums[i]>=0){
                dp[i][0]=min(nums[i],dp[i-1][0]*nums[i]);
                dp[i][1]=max(nums[i],dp[i-1][1]*nums[i]);
            }else{
                dp[i][0]=min(nums[i],dp[i-1][1]*nums[i]);
                dp[i][1]=max(nums[i],dp[i-1][0]*nums[i]);
            }
            result=max(result,dp[i][1]);
        }
        return result;
    }
};
```

###### [32. 最长有效括号 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-valid-parentheses/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否；二刷：否

思路：动态规划的思路，依旧dp(i)表示以nums(i)结尾的最长有效括号的长度。如果nums[i]是'('，则不可能是有效括号，dp[i]=0。如果nums[i]是右括号')'，则根据nums[i-1]的值来分情况讨论。具体见代码。这一题要多刷才能熟练。

代码：

```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        // 动态规划法
        // 考虑字符串为"()(())"，dp[i]表示以nums[i]结尾的最长有效括号的长度
        int n = s.size();
        vector<int> dp(n, 0);
        int result = 0;
        for (int i = 1; i < n; i++) {
            if (s[i] == '(')
                continue;
            if (s[i - 1] == '(')
                dp[i] = i - 2 >= 0 ? dp[i - 2] + 2 : 2;
            else if (i - 1 - dp[i - 1] >= 0 && s[i - 1 - dp[i - 1]] == '(')
                dp[i] = i - 2 - dp[i - 1] >= 0
                            ? dp[i - 1] + 2 + dp[i - 2 - dp[i - 1]]
                            : dp[i - 1] + 2;
            result = max(result, dp[i]);
        }
        return result;
    }
};
```

这一题还可以用栈来解决。二刷的时候一定要会

#### 图论

###### [200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/description/?envType=study-plan-v2&envId=top-100-liked)

**深度优先搜索**

难度：中等

自主解答：否；二刷：是

思路：代码基本写对了，但是有些小细节，比如说grid的元素是char而不是0,1。注意学会使用lamda表达式的递归写法，第一个参数是thie auto &&self

代码：

```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        // lambda表达式的递归调用:this auto&& self
        int dir[4][2]={0,1,0,-1,1,0,-1,0};
        int m=grid.size(),n=grid[0].size();
        int num=0;
        vector<vector<bool>>visited(m,vector<bool>(n,0));
        auto dfs = [&](this auto&&self,int x,int y)->void{
            if(x<0||y<0||x>=m||y>=n)
                return;
            if(grid[x][y]=='0'||visited[x][y])
                return;
            visited[x][y]=1;
            for(int k=0;k<4;k++){
                int nextX=x+dir[k][0];
                int nextY=y+dir[k][1];
                self(nextX,nextY);
            }
        };
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1'&&!visited[i][j]){
                    dfs(i,j);
                    num++;
                }
            }
        }
        return num;
    }
};
```

**广度优先搜索**

自主解答：否

思路：广度优先搜索和二叉树的层序遍历一样，都要用到队列

代码：

```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        // 广度优先搜索
        int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};
        int m = grid.size(), n = grid[0].size();
        int count = 0;
        vector<vector<bool>> visited(m, vector<bool>(n, 0));
        auto bfs = [&](int i, int j) -> void {
            queue<pair<int, int>> q;
            q.push({i, j});
            visited[i][j] = true;
            while (!q.empty()) {
                auto cur = q.front();
                q.pop();
                for (int k = 0; k < 4; k++) {
                    int next_i = cur.first + dir[k][0];
                    int next_j = cur.second + dir[k][1];
                    if (next_i < 0 || next_j < 0 || next_i >= m ||
                        next_j >= n || grid[next_i][next_j] == '0' ||
                        visited[next_i][next_j] == true)
                        continue;
                    visited[next_i][next_j] = true;
                    q.push({next_i, next_j});
                }
            }
        };
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1' && visited[i][j] == false) {
                    bfs(i, j);
                    count++;
                }
            }
        }
        return count;
    }
};
```

###### [994. 腐烂的橘子 - 力扣（LeetCode）](https://leetcode.cn/problems/rotting-oranges/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否

思路：多源的广度优先搜索。类似于二叉树的层序遍历，层序遍历是要首先把根节点放入队列。但是这里是要将所有一开始都是腐烂的节点放入队列

代码：

```C++
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        //广度优先搜索
        int m=grid.size(),n=grid[0].size();
        int dir[4][2]={0,1,0,-1,1,0,-1,0};
        int minutes=-1;
        int fresh=0;
        queue<pair<int,int>>q;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1)
                    fresh++;
                else if(grid[i][j]==2)
                    q.push({i,j});
            }
        }
        if(fresh==0)
            return 0;
        while(!q.empty()){
            int size=q.size();
            while(size--){
                auto [i,j]=q.front();
                q.pop();
                for(int k=0;k<4;k++){
                    int next_i=i+dir[k][0];
                    int next_j=j+dir[k][1];
                    if(next_i<0||next_j<0||next_i>=m||next_j>=n||grid[next_i][next_j]!=1)
                        continue;
                    grid[next_i][next_j]=2;
                    q.push({next_i,next_j});
                    fresh--;
                }
            }
            minutes++;
        }
        return fresh==0? minutes: -1;
    }
};
```

###### [207. 课程表 - 力扣（LeetCode）](https://leetcode.cn/problems/course-schedule/?envType=study-plan-v2&envId=top-100-liked)

难度：中等偏难

自主解答：否

思路：这题要判断图是否有环。我使用的是深度优先遍历+回溯+集合来判断，如果节点出现在了集合中就说明有环了。但是这样有个缺点，时间复杂度会非常的大，因为包含了大量重复的判断，导致超时。代码如下
```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        // 判断有没有环
        int n=prerequisites.size();
        bool result=true;
        // 定义邻接表
        vector<vector<int>>graph(numCourses);
        for(auto prerequisite:prerequisites){
            graph[prerequisite[0]].push_back(prerequisite[1]);
        }
        auto dfs=[&](this auto&& self,int i,unordered_set<int>&us)->void{
            if(us.find(i)!=us.end()){
                result=false;
                return;
            }
            us.insert(i);
            for(auto neighbor:graph[i]){
                self(neighbor,us);
            }
            us.erase(i);
        };
        for(int i=0;i<numCourses;i++){
            unordered_set<int>us;
            dfs(i,us);
        }
        return result;
    }
};
```

下面是三色标记法，简单又实用，不用使用复杂的unordered_set

自主解答：否

```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        int n = prerequisites.size();
        bool result = true;
        // 定义邻接表
        vector<vector<int>> graph(numCourses);
        for (auto prerequisite : prerequisites) {
            graph[prerequisite[0]].push_back(prerequisite[1]);
        }
        // 三色标记法，0表示未遍历，1表示在路径中，2表示该节点不在环中
        vector<int> status(numCourses, 0);
        auto dfs = [&](this auto&& dfs, int i) -> bool {
            if (status[i] == 1)
                return false;
            if (status[i] == 2)
                return true;
            status[i] = 1;
            for (int next : graph[i]) {
                if (!dfs(next))
                    return false;
            }
            status[i] = 2;
            return true;
        };
        for (int i = 0; i < numCourses; i++) {
            if (!dfs(i))
                return false;
        }
        return true;
    }
};
```

###### [100. 最大岛屿的面积](https://kamacoder.com/problempage.php?pid=1172)

难度：中等

自主解答：是

代码：

```C++
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
int dir[4][2] = { 0,1,0,-1,1,0,-1,0 };
int dfs(vector<vector<int>>&grid,vector<vector<bool>>&visited,int i,int j,int m,int n);
int main(){
    int m,n;
    cin>>m>>n;
    vector<vector<int>>grid(m,vector<int>(n));
    vector<vector<bool>>visited(m,vector<bool>(n));
    int maxArea=0;
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++)
            cin>>grid[i][j];
    }
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            maxArea=max(maxArea,dfs(grid,visited,i,j,m,n));
        }
    }
    cout<<maxArea;
    return 0;
}
int dfs(vector<vector<int>>&grid,vector<vector<bool>>&visited,int i,int j,int m,int n){
    if(i<0||j<0||i>=m||j>=n||grid[i][j]==0||visited[i][j]==1)
        return 0;
    int area=1;
    visited[i][j]=1;
    for(int k=0;k<4;k++){
        int next_i=i+dir[k][0];
        int next_j=j+dir[k][1];
        area+=dfs(grid,visited,next_i,next_j,m,n);
    }
    return area;
}
```

###### [101. 孤岛的总面积](https://kamacoder.com/problempage.php?pid=1173)

难度：困难

自主解答：是

思路：这题改了好久，有点复杂。使用深度优先的话不能提前return，需要使用一个布尔变量变量来记录是否是孤岛

代码：

```C++
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};
int dfs(vector<vector<int>> &grid, vector<vector<bool>> &visited, int i, int j,
        int n, int m) {
  bool isSea = false;
  int area = 1;
  visited[i][j] = 1;
  for (int k = 0; k < 4; k++) {
    int next_i = i + dir[k][0];
    int next_j = j + dir[k][1];
    if (next_i < 0 || next_j < 0 || next_i >= n || next_j >= m ||
        grid[next_i][next_j] == 0 || visited[next_i][next_j] == 1)
      continue;
    int nextArea = dfs(grid, visited, next_i, next_j, n, m);
    if (nextArea == 0)
      isSea = true;
    else
      area += nextArea;
  }
  if (i == 0 || j == 0 || i == n - 1 || j == m - 1 || isSea)
    return 0;
  return area;
}
int main() {
  int n, m;
  cin >> n >> m;
  vector<vector<int>> grid(n, vector<int>(m));
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      cin >> grid[i][j];
    }
  }
  vector<vector<bool>> visited(n, vector<bool>(m, 0));
  int island = 0;
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      if (grid[i][j] == 1 && visited[i][j] == 0) {
        island += dfs(grid, visited, i, j, n, m);
      }
    }
  }
  cout << island;
  return 0;
}
```

###### [102. 沉没孤岛](https://kamacoder.com/problempage.php?pid=1174)

难度：中等偏难

自主解答：否

思路：沿着矩阵四周遍历，把所有相连的1都变成0。这样就只剩下1是孤岛了。

代码：

```C++
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
int dir[4][2] = { 0,1,0,-1,1,0,-1,0 };
void bfs(vector<vector<int>>&grid,int i,int j){
	int m=grid.size(),n=grid[0].size();
	grid[i][j]=0;
	queue<pair<int,int>>q;
	q.push({i,j});
	while(!q.empty()){
		auto cur=q.front();
		q.pop();
		int x=cur.first,y=cur.second;
		grid[x][y]=0;
		for(int k=0;k<4;k++){
			int next_x=x+dir[k][0];
			int next_y=y+dir[k][1];
			if(next_x<0||next_y<0||next_x>=m||next_y>=n
				||grid[next_x][next_y]==0)
				continue;
			q.push({next_x,next_y});
		}
	}
}
int main(){
	int m,n;
    cin>>m>>n;
    vector<vector<int>>grid(m,vector<int>(n));
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++)
            cin>>grid[i][j];
    }
	vector<vector<int>>grid_copy=grid;
	// 遍历四周
	for(int i=0;i<m;i++){
		if(grid_copy[i][0]==1)
			bfs(grid_copy,i,0);
		if(grid_copy[i][n-1]==1)
			bfs(grid_copy,i,n-1);
	}
	for(int j=0;j<n;j++){
		if(grid_copy[0][j]==1)
			bfs(grid_copy,0,j);
		if(grid_copy[m-1][j]==1)
			bfs(grid_copy,m-1,j);
	}
	for(int i=0;i<m;i++){
		for(int j=0;j<n;j++){
			if(grid[i][j]==1&&grid_copy[i][j]==1)
				grid[i][j]=0;
			cout<<grid[i][j]<<" ";
		}
		cout<<endl;
	}
}
```

###### [117. 软件构建](https://kamacoder.com/problempage.php?pid=1191)

难度：中等

自主解答：否

思路：这题考验图的拓扑排序。我们要使用一个indegree数组来记录所有节点的入度。然后把入度为0的节点放入队列中，后面再取出队列中的节点并修改改节点指向节点的入度，直到队列取空。

代码：

```C++
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
int main(){
	int n,m,s,t;
	cin>>n>>m;
	vector<vector<int>>graph(n);
	vector<int>indegree(n,0);
	vector<int>result;
	for(int i=0;i<m;i++){
		cin>>s>>t;
		graph[s].push_back(t);
		indegree[t]++;
	}
	queue<int>q;
	for(int i=0;i<n;i++){
		if(indegree[i]==0)
			q.push(i);
	}
	while(!q.empty()){
		int front=q.front();
		q.pop();
		result.push_back(front);
		// 修改节点的入度
		for(int node: graph[front]){
			indegree[node]--;
			if(indegree[node]==0)
				q.push(node);
		}
	}
	if(result.size()!=n)
		cout<<-1;
	else{
		for(int i=0;i<n-1;i++)
			cout<<result[i]<<" ";
		cout<<result[n-1];
	}
}
```

###### [208. 实现 Trie (前缀树) - 力扣（LeetCode）](https://leetcode.cn/problems/implement-trie-prefix-tree/?envType=study-plan-v2&envId=top-100-liked)

难度：困难

自主解答：否

思路：前缀树，这是一棵多叉树，每个节点都记录多个子节点和一个isEnd标签

代码：

```C++
class Trie {
private:
    struct TrieNode{
        unordered_map<char,TrieNode*>children;
        bool isEnd;
        TrieNode():isEnd(false){}
    };
    TrieNode *root;
public:
    Trie() {
        root=new TrieNode;
    }

    void insert(string word) {
        TrieNode* node=root;
        for(char c:word){
            if(node->children.find(c)==node->children.end()){
                node->children[c]=new TrieNode();
            }
            node=node->children[c];
        }
        node->isEnd=true;
    }

    bool search(string word) {
        TrieNode* node=root;
        for(char c:word){
            if(node->children.find(c)==node->children.end()){
                return false;
            }
            node=node->children[c];
        }
        return node->isEnd;
    }

    bool startsWith(string prefix) {
        TrieNode* node=root;
        for(char c:prefix){
            if(node->children.find(c)==node->children.end()){
                return false;
            }
            node=node->children[c];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

