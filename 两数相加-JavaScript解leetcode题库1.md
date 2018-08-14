### 两数相加 - JavaScript解leetcode题库(1~2)

记录Ciya刷leetcode题库的解答（题序1-2），点击蓝色链接可查看题面详情。

1. [两数之和](https://leetcode-cn.com/problems/two-sum/description/)：  

   **难度：**简单

   给定一个整数数组和一个目标值，找出数组中和为目标值的**两个**数。

   你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

   **示例:**

   ```
   给定 nums = [2, 7, 11, 15], target = 9
   
   因为 nums[0] + nums[1] = 2 + 7 = 9
   所以返回 [0, 1]
   ```

   **解答：**

   ```js
   /**
    * @param {number[]} nums
    * @param {number} target
    * @return {number[]}
    */
   var twoSum = function(nums, target) {
     for(var i = 0; i < nums.length; i++){
       var pairIdx = nums.indexOf(target - nums[i], i + 1);
       if(pairIdx !== -1)return [i, pairIdx]
     }
   };
   ```

   

2. [两数相加 - 链表](https://leetcode-cn.com/problems/add-two-numbers/description/) ：  

   **难度：**中等

   给定两个**非空**链表来表示两个非负整数。位数按照**逆序**方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

   你可以假设除了数字 0 之外，这两个数字都不会以零开头。

   **示例：**

   ```
   输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
   输出：7 -> 0 -> 8
   原因：342 + 465 = 807
   ```

   **解答：**

   一开始Ciya（喜欢借助原生API偷懒的孩子）采用了这样的思路：读取`ListNode`转化为数字，数字相加得到和，将和转换为`ListNode`输出。执行时没看出什么问题，不过提交的时候没有通过——因为测试例子中有超过31个节点的链表，转化为数字得到超过`Math.pow(2, 53)`的**大数**，进一步计算和转换出现精度丢失的问题。

   鉴于 将长链表转换得到的大数乃至后续大数计算打补丁 这个方式的执行效率必然不如链表一个节点一个节点加下去，于是按节点依次相加的思路重写了一份解答，并考虑优化点（多了几个if）：1 -  直接覆盖l1节点输出（如果题目要求入参不能动就不能这么做）；2 - 当有一个链表到头了，直接将另一链表的后续节点嫁接到和   

   

   ```js
   /**
    * Definition for singly-linked list.
    * function ListNode(val) {
    *     this.val = val;
    *     this.next = null;
    * }
    */
   /**
   * @param {ListNode} l1
   * @param {ListNode} l2
   * @return {ListNode}
   */
   var addTwoNumbers = function(l1, l2) {
     //计算结果直接覆盖l1,减少新建节点消耗
     var LastNodeOfL1 = 0;//必要时指向l1最末节点
     var curNode1 = l1;//初始计算节点
     var curNode2 = l2;
     var carry = 0;//初始进位
     //对 l1和l2 均有的位数 计算
     while(curNode1 && curNode2){
       var sum = curNode1.val + curNode2.val + carry;//和
       carry = (sum > 9) ? 1 : 0;//下次进位
       curNode1.val = sum % 10;
       //节点更新
       if(!curNode1.next)LastNodeOfL1 = curNode1;
       curNode1 = curNode1.next;
       curNode2 = curNode2.next;
     };
     //三种情景 l1位数大于l2 l1位数小于l2 l1和l2位数相等
     if(curNode1){
       //情景a: l1位数大于l2
       while(carry){// 进位计算
         curNode1.val++;
         carry = curNode1.val > 9 ? 1 : 0;
         if(carry){
           curNode1.val = curNode1.val % 10;
           if(!curNode1.next)curNode1.next = new ListNode(0);
           curNode1 = curNode1.next;
         }
       }
     }else{
       if(curNode2){
         //情景b: l1位数小于l2 让l1最末节点指向curNode2
         LastNodeOfL1.next = curNode2;
         curNode1 = LastNodeOfL1.next;
         while(carry){// 进位计算
           curNode1.val++;
           carry = curNode1.val > 9 ? 1 : 0;
           if(carry){
             curNode1.val = curNode1.val % 10;
             if(!curNode1.next)curNode1.next = new ListNode(0);
             curNode1 = curNode1.next;
           }
         }
       }else{
         //情景c: l1和l2位数相等
         if(carry)LastNodeOfL1.next = new ListNode(1);
       }      
     }
     return l1;
   };
   ```

   ![1534242809510](https://mmbiz.qpic.cn/mmbiz_png/6pibAfVWZWGicFLt2VuUU2z3s03NgILZicUhy6dnAeNLa38olsow0n75U2WZcpGo5TVXEtscxohDnu47xntORl0ow/0?wx_fmt=png)

   
