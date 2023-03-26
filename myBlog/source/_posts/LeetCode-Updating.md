---
title: LeetCode(Updating...)
date: 2023-01-13 13:57:16
tags:
---
# LeetCode（Updating…）

## 2、两数相加（考核链式）

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。



__思路__：由于JS中没有内置链表，这里是自定义好的链式。其工作原理为，定义值val和next指向，而next会指向下一个node的val（对象套对象）。

1、判断l1和l2是否为其链式最后一位数，如果都是，跳出循环。注意：这里的l1和l2都是对象，所以即便他们都是空的，也不会跳出循环。所以，只有他们都为null，即为链式的结尾时，才会跳出while循环。

2、将l1和l2的node的val处理，赋给l3的node

3、l1和l2转跳到下一个node

4、当跳出循环，表示l1和l2都已经到尾部，这时判断是否有进位，有的话给l3再添加一个进位即可

5、注意：l3 new完后赋值给a3（不要对原链进行操作），这样l3可以作为链式的头部，如果直接用l3进行所有操作，最后返回l3的时候，只有链式的最后一个位或倒数第二位（有进位的情况）。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
 //这里自定义的链式，next都指向下一个node的val
var addTwoNumbers = function(l1, l2) {
    let a1 = l1
    let a2 = l2
    let carry = 0
    let l3 = new ListNode()
    let a3 = l3
    //如果a1和a2中有一个没有读到null（结尾），循环继续。
    while(a1 || a2){
        //如果a1先结束，给后面的值都赋为0
        const val1 = a1?a1.val:0;
        //a2同理
        const val2 = a2?a2.val:0;
        const val3 = (val1 + val2 + carry) % 10
        //判断是否有进位
        carry = Math.floor((val1 + val2 + carry) / 10)

        a3.next = new ListNode(val3)
        //判断a是否为结尾，不是的话，读下一个node
        if(a1) {a1 = a1.next}
        if(a2) {a2 = a2.next}
        a3 = a3.next
    }
    //判断最后一位是否有进位
    if(carry === 1){
        a3.next = new ListNode(carry)
    }
    return l3.next
};


```

补充上面第五点：

![img](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301141549778.png)

* 情况一

```
let a = new ListNode();
// let b = a
a.next = new ListNode(1)
a = a.next
a.next = new ListNode(2)
a = a.next
console.log(a)
```

结果：![image-20230111163621543](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301111636119.png)

很明显，其实上面的代码只是不断更新a这个对象中的next属性，将最后new出来的ListNode(2)又赋给了a，最后a还是只有一个node



* 情况二

```
let a = new ListNode();
let b = a
//此时，b===a
b.next = new ListNode(1)
b = b.next
//b已经是一个全新的对象（即next指向的下一个node）， b!==a
b.next = new ListNode(2)
b = b.next
console.log(a)
```

结果：![image-20230111163956390](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301111639562.png)

可以看出，这里没有直接修改链式a的属性，而是给a.next嵌套了一个新的node对象。





## 3、无重复字符的最长字串

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

```
例子1：
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
例子2：
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

这道题想的挺久的，一开始是下面的思路（其实是读题没读对），后面才发现想的方向错了。下面的代码其实是解决一个字符串中，首尾元素重复的子字符串的最大或最小长度。

```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let arr = Array.from(s)
    let len = arr.length
    let maxCount = 0
    for(let i = 0; i < len - 1; i++){
        let j = i + 1
        let count = 0

        while(1){
            count++
            if(arr[i] == arr[j]) {
                break
            }
            if(count > maxCount) {
                maxCount = count
            }

            j++
            if(j == len) {
                break
            }

        }
    }
    return maxCount
}
```

后来实在想不到了，就看到下面这幅图，思想很清晰，具体实现过程写在代码的注释里了。

![img](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301121417343.png)

```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    //先将字符串转数组
    let arr = Array.from(s)
    //将该数组的第一位赋给新数组[]
    let newArr = [arr[0]]
    let len = arr.length
    let newLen = newArr.length
    let maxCount = 0
    //对数组arr进行遍历
    for(let i = 1; i < len; i++){
        //在arr的基础上对newArr进行遍历
        for(let j = 0; j < newLen; j++){
            //判断是否新数组中是否已经存在 属于arr下标i对应的元素
            let index = newArr.indexOf(arr[i])
            newArr.push(arr[i])
            //存在的话（newArr中相同的元素最多有两个，且其他元素皆无相同元素），
            //将找到相同元素的第一个元素前面的全部数组删除。
            if(index >= 0){
                newArr.splice(0 , index + 1)
            }
            //如果修改后的数组长度要大于前面记录的maxCount，赋值
            if(newArr.length > maxCount){
                maxCount = newArr.length
            }
        }
    }
    return maxCount
}
```



## 5、最长回文串

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

 ```
 例子：
 输入：s = "babad"
 输出："bab"
 解释："aba" 同样是符合题意的答案。
 ```

又是想了很久的题目，思路是左右指针偏移的情况。

![截屏2019-12-06上午7.54.28.png](https://pic.leetcode-cn.com/533a8538f42e6a6495b87d0c054224fdaa2e6da1cd9158f3e9042894137961fc-截屏2019-12-06上午7.54.28.png)

本以为是很简单的情况，但越写发现情况越多，虽然最后代码写出来了，但实在是太复杂了。。。

```
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let arr = Array.from(s)
    let len = arr.length
    let OutArr = [arr[0]]
    //数组长度小于三的情况
    if(arr.length <= 2){
        if(arr[0]==arr[1]){
            return s
        } else {
            return arr[0]
        }
    }

    //数组长度大于等于三的情况
   for(let i = 1; i < (len - 1); i++){
        let j = (i - 1)//左指针
        let k = (i + 1)//右指针
        //如果一开始左指针和i相同，就让左指针一直左移，直到不同；
        while((arr[j] == arr[i])){
            //跳出循环两个条件
            //1、左指针到达尽头
            //2、左指针的数不等于右指针的数（这种情况是为了解决左右指针一开始就不同的字符串 如 1223 12224）
            if(arr[k] != arr[j] && (k-j) >= OutArr.length){
                OutArr = arr.slice(j,k)
                break
            }
            if(j == 0) break
            if(j > 0){
                j--//左指针左移
            }
        }
        while((arr[k] == arr[i])){
            //这边和上面是同理的
            if( arr[k] != arr[j]){
                //(k-j) >= OutArr.length是为了保证最后一轮进入不了判断3，而影响OutArr结果
                if(arr[j]==arr[i] && (k-j) >= OutArr.length){
                    OutArr = arr.slice(j , k+1)
                }
                if(arr[j] != arr[i] && (k-j) >= OutArr.length){
                    OutArr = arr.slice(j+1 , k+1)
                }
                k++
                break
            }
            if(k == (len - 1)) break
            if(k < len){
                k++//右指针右移
            }
        }

        //判断3：进入开始判断左右指针(此时左右指针之间的数字为一个或多个arr[i]，即(左指针)11111(右指针))
        while(arr[k] == arr[j]){
            //左右指针相同，就继续扩大范围
            if(arr[k] == arr[j]){
               let tempArr = arr.slice(j, (k + 1))
               //判断这次循环的回文字符串是否是最大的
               if(tempArr.length >= OutArr.length) {
                    OutArr = tempArr
                }
                //如果左右指针在不断移动的过程中仍相同，当左指针或右指针到尽头，跳出来
                if(j == 0 || k == (len + 1)){
                    break
                }
            }
            j--
            k++
        }
   }

   return OutArr.join('')
};

```

别人写的，思路一样，这个其实就是将情况分成了两种，奇数长度（aba）和偶数长度（abba）：

```
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
        if (s.length<2){
            return s
        }
        let res = ''
        for (let i = 0; i < s.length; i++) {
            // 回文子串长度是奇数
            helper(i, i)
            // 回文子串长度是偶数
            helper(i, i + 1) 
        }

        function helper(m, n) {
            while (m >= 0 && n < s.length && s[m] == s[n]) {
                m--
                n++
            }
            // 注意此处m,n的值循环完后  是恰好不满足循环条件的时刻
            // 此时m到n的距离为n-m+1，但是mn两个边界不能取 所以应该取m+1到n-1的区间  长度是n-m-1
            if (n - m - 1 > res.length) {
                // slice也要取[m+1,n-1]这个区间 
                res = s.slice(m + 1, n)
            }
        }
        return res
};

作者：Salvatore
链接：https://leetcode.cn/problems/longest-palindromic-substring/solutions/697935/chao-jian-dan-de-zhong-xin-kuo-san-fa-yi-qini/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



## 20、有效的括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。



这道题其实没啥思路，感觉自己有点陷入一直用for循环的死胡同了，啥都想要用for解决= =

看到栈方法后，思路一下就清晰了，其实用栈解决这道题，会很简单。

![image-20230114135003411](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301141350492.png)

![image-20230114135022537](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301141350601.png)

流程图如下：

![image-20230114140914559](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301141409629.png)

```
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    let stack = []
    let map = new Map([
        ['{','}'],
        ['(',')'],
        ['[',']']
    ])
    let len = s.length
    if(len % 2 != 0){
        return false
    }
    for(let i = 0; i < len; i++){
        //判断s[i]是否为{ [ (,是的话，就给栈加入
        if(map.has(s[i])){
            stack.push(s[i])

        } else if(map.get(stack[stack.length - 1]) == s[i]){
            //判断s[i]是否为} ] )且对应栈里的最后一位
            stack.pop()
        } else {
            return false
        }

        //判断
        
    }
    return stack.length == 0? true : false
};
```



## 35、搜索插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

> ```
> 实例一：
> 输入: nums = [1,3,5,6], target = 5
> 输出: 2
> 实例二：
> 输入: nums = [1,3,5,6], target = 2
> 输出: 1
> 实例三：
> 输入: nums = [1,3,5,6], target = 7
> 输出: 4
> ```

这道题其实很好想到使用二分法做，但写的时候还是遇到很多问题，其实就是边界定义搞不清楚。

用了很多if去限制边界，这样其实是很麻烦的。

> ```
> /**
>  * @param {number[]} nums
>  * @param {number} target
>  * @return {number}
>  */
> var searchInsert = function(nums, target) {
>     let min = 0
>     let max = nums.length - 1
>     let index
>     let out = nums.length
>     while(min <= max) {
>         //取中间索引，使用以下方法而不是Math.floor((max + min) /2)，是防止溢出
>         index = min + Math.floor((max - min) /2)
>         //如果目标值比中间索引对应的值大，让最小值向中间索引右边移动一位，
>         //因为跳出循环条件是最大值小于最小值
>         //所以当目标值>中间索引对应的值时，target对应的索引必然不是此时index，且必然会进入下一次循环
>         //而当目标值<或=中间索引对应的值时,target对应的索引可能是此时index，所以需要用out记录此index
>         if(target > nums[index]){
>             min = index + 1
>         } else {
>             out = index
>             max = index - 1
>         }
>     }
>     return out
> };
> ```



## 69、x的平方根

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

 

* 牛顿迭代法

![image-20230131154010954](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301311540205.png)

由于是求平方根

即是`y = x^2 - C`，选择x0=C为初始值

其中C就是y=0时候，开方对应的要求得的值即：C=x^2 =>  x = C^1/2 （x为最终求得的平方根，C为要计算的平方根），其中切点为（x0，x0^2 - C）

可以得到切线函数为 `y - (x0^2 + C) = 2x0(x - x0)  `

使 y=0 可以得到新的与x轴相交的切线的点 x1

`x = 1/2(x0 + C/x0)` 

将x赋给x0，这样子，x就会不断迭代，向着真正的根x逼近。

由于JS中数字是有精度的，当x0无限接近x的时候，x^2也无限接近于C，当精度到达一定数量级，x^2 == C

> ```
> /**
>  * @param {number} x
>  * @return {number}
>  */
> var mySqrt = function(x) {
>     let c = x
>     let x0 = x
>     while(x0*x0 > x){
>         x0 = 0.5*(x0 + c/x0) | 0
>     }
>     return Math.floor(x0)
> };
> ```



## 70、爬楼梯


假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？



* 斐波那契数列

​		其实这道题，可以倒着想。当在第n层的时候有f(n)种方法到达第n层，那么要想达到第n层，就只有从第n-1层爬一阶或者第n-2层爬两阶两种方法。

​		那么第n-1层=>f(n-1)种方法到第n-1层；同理第n-2层=>f(n-2)种方法到第n-2层；

​		这也就可以推算出，f(n) = f(n-1) + f(n+2) 然后可以递归回到第1、2层

​		到达第一层只有一种方法:f(1)=1，到达第二层有两种方法f(2)=2，这样倒退就可以得到f(n)了。

> ```
> /**
>  * @param {number} n
>  * @return {number}
>  */
> var climbStairs = function(n) {
>     let arr = new Array(n).fill(0)
>     arr[0] = 1
>     arr[1] = 1
>     for(let i = 2 ; i <= n ; i++){
>         arr[i] = arr[i-1] + arr[i-2]
>     }
>     return arr[n]
> 
> };
> ```



## 101、对称二叉树

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

![image-20230201163945557](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202302011639641.png)



* 递归

![image-20230201164030250](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202302011640326.png)

思路其实就上面这图。每个节点的左子树（右子树）和另一个节点的右子树（左子树）相等。

> ```
> /**
>  * Definition for a binary tree node.
>  * function TreeNode(val, left, right) {
>  *     this.val = (val===undefined ? 0 : val)
>  *     this.left = (left===undefined ? null : left)
>  *     this.right = (right===undefined ? null : right)
>  * }
>  */
> /**
>  * @param {TreeNode} root
>  * @return {boolean}
>  */
> var isSymmetric = function(root) {
> 	//定义左右子树比较方法
>     let Compare = (left , right) => {
>     	//如果子树比较到了尽头，返回true
>         if(left == null && right == null) return true
>         //左右子树都存在，比较
>         if(left && right){
>         	//如果该节点相同，就继续往下比较其子树
>             if(left.val == right.val){
>             	//先左边子树左边和右边子树右边进行比较，比较完了没问题后，左边子树右边和右边子树左边
>             	//进行比较。PS:只有Compare返回true才有继续比较的必要，只要出现一个false，前面递归
>             	//调用的所有函数都会直接变成false
>                 return Compare(left.left, right.right) && Compare(left.right, right.left)
>             } else  {
>                 return false
>             }
>         }
>         //只有一个子树存在的情况，必然不对称
>         return false
>     }
>     //啥也没有的情况，也是对称
>     if(root == null){
>         return true
>     }
>     //开始递归
>     return Compare(root.left , root.right)
> };
> ```
>



## 108、将有序数组转换为二叉搜索树

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 **高度平衡** 二叉搜索树。

**高度平衡** 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

 ![image-20230202150045274](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202302021500688.png)

* 迭代法

定义二叉树比较方法、插入方法；

每次调用插入方法都去调用二叉树比较方法，然后在正确位置插入。

* 递归法

由于给定的数组已经是升序排列，所以可以采用二分法（取中间索引）的方式，去分成一个一个的子树。再通过递归，将建立好的子树拼凑起来，最后就是一个平衡的二叉树。

> ```
> /**
>  * Definition for a binary tree node.
>  * function TreeNode(val, left, right) {
>  *     this.val = (val===undefined ? 0 : val)
>  *     this.left = (left===undefined ? null : left)
>  *     this.right = (right===undefined ? null : right)
>  * }
>  */
> /**
>  * @param {number[]} nums
>  * @return {TreeNode}
>  */
> const Bulid = (nums, start, end) => {
>     if(start > end) {
>         return null
>     }
>     //取中间索引对应的值
>     const middle = Math.floor((start + end)/2)
>     const root = new TreeNode(nums[middle])
> 	//构建左右子树（递归）
>     root.left = Bulid(nums , start , middle - 1)
>     root.right = Bulid(nums, middle + 1, end)
>     return root
> }
> 
> var sortedArrayToBST = function(nums) {
> 	//递归入口
>     return Bulid(nums, 0, nums.length - 1)
> };
> ```



## 110、平衡二叉树

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 。



* 思路

先序遍历每一个节点，对于每个节点，如果该节点左右子树的高度差不超过1，继续先序遍历。

递归短路条件优先级： 1、如果左右高度差不超1 => 2、左遍历 => 3、右遍历

> ```
> /**
>  * Definition for a binary tree node.
>  * function TreeNode(val, left, right) {
>  *     this.val = (val===undefined ? 0 : val)
>  *     this.left = (left===undefined ? null : left)
>  *     this.right = (right===undefined ? null : right)
>  * }
>  */
> /**
>  * @param {TreeNode} root
>  * @return {boolean}
>  */
> 
> const Depth = (root) => {
>     if(root === null) return 0
>     return Math.max(Depth(root.left) ,Depth(root.right)) + 1
> }
> 
> var isBalanced = function(root) {
>     if(root === null) return true
>     let left = Depth(root.left)
>     let right = Depth(root.right)
> 
>     return Math.abs(left - right) <= 1 && isBalanced(root.left) &&  isBalanced(root.right)
> };
> ```

## 121、买卖股票的最佳时机

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。



* 暴力解法

直接两个for结束战斗，但很可能超时。



* 贪心算法

只需要遍历价格数组一遍，记录历史最低点，然后在每一天考虑这么一个问题：如果我是在历史最低点买进的，那么我今天卖出能赚多少钱？

这种想法其实就是取最左最小值，取最右最大值，那么得到的差值就是最大利润。

> ```
> /**
>  * @param {number[]} prices
>  * @return {number}
>  */
> var maxProfit = function(prices) {
>     const n = prices.length
>     let minPrice = prices[0]
>     let sellPrice = 0
>     for(let i = 0; i < n; i++){
>         if(prices[i] < minPrice){
>             minPrice = prices[i]
>         }
>         sellPrice = Math.max(prices[i] - minPrice ,sellPrice)
>     }
>     return sellPrice
> };
> ```



## 141、环形链表

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。



* 暴力解法

两个For，不写



* 快慢指针法

一个快指针按照两个next的速度遍历链表，一个慢指针一个一个遍历链表。

如果存在循环，快指针一定会在某时刻等于慢指针；其实就是绕操场跑圈的思路，跑得快的人，一定会在某个时候碰见跑的慢的人。

> ```
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val) {
>  *     this.val = val;
>  *     this.next = null;
>  * }
>  */
> 
> /**
>  * @param {ListNode} head
>  * @return {boolean}
>  */
> var hasCycle = function(head) {
>     let fast = head, slow = head
>     if(head == null || head.next == null) return false
>     while(fast){
>         if(fast.next == null) return false
>         // if(fast.next.next == null) return false
>         fast = fast.next.next
>         slow = slow.next
>         if(fast == slow) return true
>         if(slow.next == null) return false
>     }
>     return false
> }
> ```



## 160、相交链表

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交：

![image-20230206143205932](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202302061432001.png)

* 双指针法

pA走过的路径为A链+B链

pB走过的路径为B链+A链

pA和pB走过的长度都相同，都是A链和B链的长度之和，相当于将两条链从尾端对齐，如果相交，则会提前在相交点相遇，如果没有相交点，则会在最后相遇。

![image-20230206143322114](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202302061433171.png)

> pA:1->2->3->4->5->6->null->9->5->6->null
>
> pB:9->5->6->null->1->2->3->4->5->6->null



> ```
> /**
>  * Definition for singly-linked list.
>  * function ListNode(val) {
>  *     this.val = val;
>  *     this.next = null;
>  * }
>  */
> 
> /**
>  * @param {ListNode} headA
>  * @param {ListNode} headB
>  * @return {ListNode}
>  */
> var getIntersectionNode = function(headA, headB) {
>     if(headA == null || headB == null) return null
>     let pA = headA , pB = headB
>     while(pA !== pB){
>         if(pA !== null){
>             pA = pA.next
>         } else {
>             pA = headB 
>         }
>         if(pB !== null){
>             pB = pB.next
>         } else {
>             pB = headA
>         }
>         
>     }
>     return pA
> };
> ```
>





## 205、同构字符串

给定两个字符串 `s` 和 `t` ，判断它们是否是同构的。

如果 `s` 中的字符可以按某种映射关系替换得到 `t` ，那么这两个字符串是同构的。

每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

![image-20230207134543873](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202302071345949.png)



* 索引法

同构字符串，两个字符串的每字符 首次出现、最后出现、指定位出现 索引始终相同。

> ```
> /**
>  * @param {string} s
>  * @param {string} t
>  * @return {boolean}
>  */
> var isIsomorphic = function(s, t) {
>     if(s === t && s === '') return true
>     for(let i = 0; i < s.length; i++){
>         if(s.indexOf(s[i], i+1) !== t.indexOf(t[i], i+1)) return false
>     }
>     return true
> };
> ```





## 263、丑数

**丑数** 就是只包含质因数 `2`、`3` 和 `5` 的正整数。

给你一个整数 `n` ，请你判断 `n` 是否为 **丑数** 。如果是，返回 `true` ；否则，返回 `false` 。

 

质因数其实是 `n = 2^a + 3^b + 5^c （a，b，c为0到正无穷的整数）`

只要数字a取余数字b为0，b就是a的因子

> ```
> /**
>  * @param {number} n
>  * @return {boolean}
>  */
> var isUgly = function(n) {
>     if(n <= 0) return false
>     while(n % 2 == 0) n /= 2
>     while(n % 3 == 0) n /= 3
>     while(n % 5 == 0) n /= 5
>     if(n == 1) return true
>     return false 
> };
> ```

​	

## 350、两个数组的交集 II

给你两个整数数组 `nums1` 和 `nums2` ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取**较小值**）。可以不考虑输出结果的顺序。





* 暴力解法

先找到交集的数组，再用这个数组和nums1和nums2比较，找到较小值。（麻烦哦！）



* 双指针

1、将两个数组小到大顺序排列

2、两个指针分别对两个数组从0开始遍历

3、如果指针对应元素相同，在返回数组中压入该元素。

4、如果对应元素不相同，较小元素对应的指针右移一位，再次比较。

5、重复第三和第四步，直到某一数组指针到达最右，结束循环。

6、最后返回的数组就是答案。

> ```
> /**
>  * @param {number[]} nums1
>  * @param {number[]} nums2
>  * @return {number[]}
>  */
> var intersect = function(nums1, nums2) {
>     let ans = []
>     let pB = 0
>     let pS = 0
>     let numB = nums1.sort((a, b) => a - b)
>     let numS = nums2.sort((a, b) => a - b)
>     while(pS < numS.length && pB < numB.length){
>         if(numB[pB]  == numS[pS]){
>             ans.push(numS[pS])
>             pB++
>             pS++
>         } else{
>             numB[pB]  > numS[pS]?pS++:pB++
>         }
> 
>     }
>     return ans
> };
> ```



## 389、找不同

给定两个字符串 `s` 和 `t` ，它们只包含小写字母。

字符串 `t` 由字符串 `s` 随机重排，然后在随机位置添加一个字母。

请找出在 `t` 中被添加的字母。

**示例 1：**

```
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
```



* 暴力解法

将两个字符串转数组后排序，然后for循环逐一对比每一个元素。

> ```
> /**
>  * @param {string} s
>  * @param {string} t
>  * @return {character}
>  */
> var findTheDifference = function(s, t) {
>     let T = t.split('').sort()
>     let S = s.split('').sort()
>     for(let i = 0; i < T.length; i++){
>         if(S[i]!==T[i]) return T[i]
>     }
> }   
> ```



* 位运算

**相同两数按位异或`^` = `0`。`0 ^`任何数 = 数本身。`^`满足交换律**

既然，两个字符串相比较，后面那位只多出了一个元素，两个字符串其余元素完全相同。那就可以用异或了。最后异或出来的数字，就是多出来的那一个。

> ```
> var findTheDifference = function(s, t) {
>     let i = -1, r = 0
>     while(++i < s.length) r ^= s.charCodeAt(i) ^ t.charCodeAt(i)
>     return String.fromCharCode(r ^ t.charCodeAt(i))
> };
> 
> ```
