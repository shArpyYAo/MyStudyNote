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
