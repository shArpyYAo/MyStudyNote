# MyStudyNote
自己总结的学习笔记（算法、数据结构、浏览器渲染优化相关、网站性能相关、js、css、html）

2018/7/5.

最近看了一篇文章，激起了我的学习欲望，想要刷算法题提高自己的核心竞争力。不知道能坚持多久。
发现在前端需要蛮多的数据转换的，比如说从后台拿来数据，要转换成uiData方便显示。
在leetcode上看了下题目，发现一些简单或中等难度的题目都挺适合拿来训练的。
当然浏览器渲染优化相关、网站性能相关、js、css、html这些也要学习，但是还没提上日程就是了。。。

## 算法leetcode刷题总结

### 第一题
> 给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

##### 示例

    给定 nums = [2, 7, 11, 15], target = 9
    因为 nums[0] + nums[1] = 2 + 7 = 9
    所以返回 [0, 1]
    
##### 我的代码

    /**
     * @param {number[]} nums
     * @param {number} target
     * @return {answer[]}
     */
    var twoSum = function(nums, target) {
        let temp = [];
        let mark = {};
        let answer = [];
        for(let i = 0;i < nums.length; i++){
          mark[nums[i]] = i + 1;
        }
        for(let i = 0;i < nums.length; i++){
          temp[i] = target - nums[i];
          if(mark[temp[i]] !== undefined && mark[temp[i]] - 1 !== i){
            answer[0] = i;
            answer[1] = mark[temp[i]] - 1;
            break;
          }
        }
        return answer;
    };
    
##### 思路
首先是建立一个标记数组mark，下标为nums数组的每一个元素，对应存放nums数组每一个元素的下标 + 1.
接下来遍历nums数组，首先计算target与nums数组每一个元素的差，存放到temp数组里
然后用mark数组来判断在nums数组里是否有元素与差相等。这样就省去了一次判断是否相等的一次遍历，存储空间则利用js数组的特性，省了很多空间。
mark[temp[i]] - 1 !== i这个判断条件一是为了避免自己跟自己判断重复，二是直接用mark[temp[i]] - 1来记录对应下标。
执行用时：80 ms。在leetcode上战胜 90.17 % 的 javascript 提交记录

##### leetcode上记录的最快解法 --- 2018/7/5 复制

    /**
     * @param {number[]} nums
     * @param {number} target
     * @return {number[]}
     */
    var twoSum = function(nums, target) {
        var hash = {}
        var i, num, pair, len = nums.length
        for(i=0; i< len;i++) {
            num = nums[i]
            pair = target - num
            if (hash.hasOwnProperty(pair)) {
                return [hash[pair], i]
            }
            hash[num] = i

        }
    };

##### 思路
他的思路跟我类似，但是他判断相同的地方用的是hasOwnProperty方法，总体来说比我的优雅，也比我的快
可以看出自己的js基础还是比较弱的

### 第二题

> 给定一个字符串，找出不含有重复字符的最长子串的长度

##### 示例

    给定 "abcabcbb" ，没有重复字符的最长子串是 "abc" ，那么长度就是3。
    给定 "bbbbb" ，最长的子串就是 "b" ，长度是1。
    给定 "pwwkew" ，最长子串是 "wke" ，长度是3。请注意答案必须是一个子串，"pwke" 是 子序列  而不是子串
    
##### 我的代码

        /**
         * @param {string} s
         * @return {answer}
         */
        var lengthOfLongestSubstring = function(s) {
            let tempO = {};
            let stack = [];
            let index = 0;
            let len = 0;
            let answer = 1;
            for(let i = 0;i < s.length; i++){
                let temp = s[i];
                if(tempO[temp] === undefined){
                    tempO[temp] = 0;
                    stack[index] = temp;
                    index++;
                    if(answer < stack.length){
                        answer = stack.length;
                    }
                }else {
                    let tempIndex = 0;
                    let tempStack = [];
                    index = stack.lastIndexOf(temp) + 1;
                    if(index === stack.length){

                    }else{
                        for(let j = index;j < stack.length;j++){
                            tempStack[tempIndex] = stack[j];
                            tempIndex++;
                        }
                    }
                    tempStack[tempIndex] = temp;

                    if(answer < tempStack.length){
                        answer = tempStack.length;
                    }
                    stack = tempStack;
                    index = tempIndex + 1;
                }

            }
            if(s.length === stack.length){
                answer = s.length;
            }
            if(stack.length > answer){
                answer = stack.length;
            }
            return answer;
       };
      
##### 思路
可以看到，实现的还是很菜的。嵌套挺深的。
先是循环字符串s，用tempO对象来判断重复（利用js对象的哈希寻值）
一边判断一边建立字典。同时更新答案
如果判断重复了，首先寻找重复字母在栈中的位置，留下重复字母后面的。同时更新答案

##### leetcode上记录的最快解法 --- 2018/7/5 复制

    /**
     * @param {string} s
     * @return {number}
     */
    var lengthOfLongestSubstring = function(s) {
        if (!s.length) return 0;
        var max = 1, flag = 0
        for(var i = 0; i < s.length; i++) {

            var index = s.indexOf(s[i], flag) 
            if (index !== -1 && index < i) flag = index + 1;

            max = Math.max(max, i - flag + 1)
        }
        return max
    };
    
##### 思路
没有对比，就没有伤害。。。
他是直接寻找重复字母的位置，然后接着重复字母的位置接着找，跟我的思路差不多
但是实现比我的好多了，我还有个栈。。。。

### 第三题

> 给定两个大小为 m 和 n 的有序数组 nums1 和 nums2 。请找出这两个有序数组的中位数。要求算法的时间复杂度为 O(log (m+n)) 。

##### 示例1

    nums1 = [1, 3]
    nums2 = [2]

    中位数是 2.0
    
##### 示例2

    nums1 = [1, 2]
    nums2 = [3, 4]

    中位数是 (2 + 3)/2 = 2.5
    
##### 我的代码

        /**
         * @param {number[]} nums1
         * @param {number[]} nums2
         * @return {number}
         */
        var findMedianSortedArrays = function(nums1, nums2) {
            let findIndex = [], over, padding = 999999;
            let mid, subIndex = 0, preOK;
            if(nums1[0] === undefined){
                if((nums2.length % 2) !== 0){
                    return nums2[Math.floor(nums2.length / 2)];
                }else{
                    return (nums2[Math.floor(nums2.length / 2)] + nums2[Math.floor(nums2.length / 2) - 1]) / 2;
                }
            }
            if(nums2[0] === undefined){
                if((nums1.length % 2) !== 0){
                    return nums1[Math.floor(nums1.length / 2)];
                }else{
                    return (nums1[Math.floor(nums1.length / 2)] + nums1[Math.floor(nums1.length / 2) - 1]) / 2;
                }
            }
            if(nums1[0] > nums2[0]){
                [nums1, nums2] = [nums2, nums1];
            }
            if(nums1.length === 1 && nums2.length !== 1){
                nums1[1] = nums2[nums2.length - 1];
                nums2.length = nums2.length - 1;
            }
            over = nums1.length - 1;
            console.log('1: ' + nums1);
            console.log('2: ' + nums2);
            for(let i = 0;i < nums2.length;i++){
                findIndex.push(binsearch(nums1, nums2[i], 0, nums1.length - 1));
            }
            console.log(findIndex);
            mid = Math.floor((nums1.length + nums2.length) / 2);
            for(let i = 0; i < findIndex.length;i++){
                let temp = mid - (findIndex[i] + i + 1);
                // console.log('temp: ' + temp);
                if(padding > Math.abs(temp)){
                    padding = temp;
                    subIndex = i;
                }
                if(findIndex[i] + (i + 1) === mid){
                    if((nums1.length + nums2.length) % 2 !== 0){
                        return nums2[i];
                    }else{
                        if(findIndex[i - 1] + (i) === mid - 1){
                            return (nums2[i] + nums2[i - 1]) / 2;
                        }else{
                            return (nums2[i] + nums1[findIndex[i]]) / 2;
                        }
                    }
                }
            }
            if((nums1.length + nums2.length) % 2 !== 0){
                if(padding < 0){
                    return nums1[findIndex[subIndex] + padding + 1];
                }else{
                    return nums1[findIndex[subIndex] + padding];
                }
            }else{
                if(findIndex[subIndex] + (subIndex + 1) === mid - 1){
                    return (nums2[subIndex] + nums1[findIndex[subIndex] + padding]) / 2;
                }else{
                    /*console.log('nums1[findIndex[subIndex] + padding]: ' + nums1[findIndex[subIndex] + padding]);
                    console.log('nums1[findIndex[subIndex] + padding - 1: ' + nums1[findIndex[subIndex] + padding - 1]);*/
                    if(padding < 0){
                        return (nums1[findIndex[subIndex] + padding + 1] + nums1[findIndex[subIndex] + padding]) / 2;
                    }else{
                        return (nums1[findIndex[subIndex] + padding] + nums1[findIndex[subIndex] + padding - 1]) / 2;
                    }
                }
            }
        };

        function binsearch(nums1, key, left, right){
            let mid;
            while(left <= right)
            {
                if(right === left){

                    return left;
                }else if(right - left === 1){
                    if(nums1[left] > key){
                        return left - 1;
                    }
                    if(nums1[left] <= key && key <= nums1[right]){
                        return left;
                    }
                    if(nums1[right] < key){
                        return right;
                    }
                }else{
                    mid = Math.floor(left + (right-left)/2);
                    if(nums1[mid] < key)
                    {
                        //console.log('1.mid: ' + mid);
                        left = mid;
                    }else if(nums1[mid] > key) {
                        //console.log('2.mid: ' + mid);
                        right = mid;
                    }else{
                        return mid;
                    }
                }
            }
            return -1;
        }
      
##### 思路
大致思路是先遍历nums2数组，然后用二分法在nums1中寻找插入位置到findIndex数组中
然后遍历findIndex数组。
例如nums1：[1,3,4],nums2：[2],findIndex：[0]
为什么findIndex[0]是零呢，这是因为findIndex记录这nums2在nums1中的插入位置
findIndex[0]表示nums2下标0的数2在nums1中的插入位置是0.
如果nums2为[6]，那么findIndex[0]就是2.
然后findIndex数组的每个数加上他们的下标再加1（我们叫它findNums2InNums3）.刚好是nums1和nums2合并后nums2的数在nums3的下标
这样直接判断nums3的中位数下标是否与findNums2InNums3的其中一个数相等
如果相等，那就是我们要的
如果不等，那意味着中位数存在于nums1中
那就计算nums3中位数下标与findNums2InNums3的其中一个数的最近距离，
通过这个距离算出nums1的数。（具体看代码，不过我猜没人想看也看不懂。。。。）
这个算法时间复杂度为m*log(n)。用时420ms。还是达不到题目的要求。但是还是过了
我的提交执行用时战胜 2.77 % 的 javascript 提交记录，这个结果也是显然的。。。。

##### leetcode上记录的最快解法 --- 2018/7/10 复制

    /**
     * @param {number[]} nums1
     * @param {number[]} nums2
     * @return {number}
     */
    var findMedianSortedArrays = function(nums1, nums2) {
        var a = [];
        for (var i = 0, j = 0; i < nums1.length && j < nums2.length;) {
            if (nums1[i] > nums2[j]) {
                a.push(nums2[j++]);
            }
            else {
                a.push(nums1[i++]);
            }
        }
        var left = nums1.length == i ? nums2.slice(j) : nums1.slice(i);
        a = a.concat(left);
        if (a.length % 2) {
            return a[Math.floor(a.length / 2)];
        }
        else {
            return (a[Math.floor(a.length / 2)] + a[Math.floor(a.length / 2 - 1)]) / 2;
        }
    };
    
##### 思路
