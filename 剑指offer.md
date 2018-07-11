# 剑指offer

标签（空格分隔）： 秋招 笔试 java 练手

---

### NO.2 替换空格
    题目描述
    请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

第一个版本

 - 未实现在原字符串上的直接修改（会报数组越界）。新建了一个字符串s1
 - 从后向前替换，避免多个空格时，后面的字符会移动多次的情况，每个字符只需移动一次
 - 不考虑java里面的replace函数


```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        int count = 0;
        char[] s = str.toString().toCharArray();
        
        for(int i=0;i<s.length;i++)
        {
            if(s[i]==' '){
                count++;
            }
         }
        char[] s1 = new char[2*count + str.length()];
        int loc=2*count + str.length()-1;
        for(int i=s.length-1;i>=0;i--)
        {
            if(s[i]!=' ')
            {
                s1[i+count*2]=s[i];
            }
                else
                {   count--;
                    s1[i+2*count]='%';
                    s1[i+count*2+1]='2';
                    s1[i+count*2+2]='0';
                    
                }
        }
    	 return String.valueOf(s1);
    }
}
```
---
### NO.3 从尾到头打印链表
    题目描述
    输入一个链表，从尾到头打印链表每个节点的值。
后进先出，翻转，联想到堆栈

 - 利用堆栈先进后出的特性，链表的头先进则最后出，即将链表翻转，实现从尾到头
 - 还有递归解法

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
import java.util.Stack;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
         Stack<Integer> stack=new Stack<Integer>();
        while(listNode!=null){
            stack.push(listNode.val);
            listNode=listNode.next;
        }
        ArrayList<Integer> list=new ArrayList<Integer>();
        while(!stack.isEmpty())
        {
            list.add(stack.pop());
        }
        return list;
    }
}
```
---
### NO4. 重建二叉树
    题目描述
    输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}  和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
    

 - 递归实现，通过前序遍历确定根节点，中序遍历确定左右子树
 - 见剑指offer p63

    
```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.*;
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        if(pre.length == 0 || in.length == 0){
            return null;
        }
        TreeNode tn = new TreeNode(pre[0]);
        for (int i = 0;i < in.length;i++){
            if(pre[0] == in[i]){
                tn.left = reConstructBinaryTree(Arrays.copyOfRange(pre,1,i+1),Arrays.copyOfRange(in,0,i));
                tn.right = reConstructBinaryTree(Arrays.copyOfRange(pre,i+1,pre.length),Arrays.copyOfRange(in,i+1,in.length));
            }
             
        }
        return tn;
    }
}
```
---
### NO5. 用两个栈实现队列
    题目描述
    用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

 - 用stack1当做队列的入，stack当做队列的出
 - 每次stack1不为空时，将其所有元素出栈并压入stack2，这时stack2出栈即为先进先出
 - 每次stack2完成出栈操作后如果栈不为空则需要将所有数重新压进空的stack1，等待新的数据压进stack1，保证stack2的剩余数据比新数据早压入stack1

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
     stack1.push(node);
    }
    
    public int pop() {
    while(!stack1.empty()){
        stack2.push(stack1.pop());
    }
    int i=stack2.pop();
     while(!stack2.empty()){
        stack1.push(stack2.pop());
    }
        return i;
    }
}
```
---
### NO6. 旋转数组的最小数字
    题目描述
    把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

#### 解法一

 - 最普通的方式直接遍历数组找到最小值，没有利用旋转和排序数组的特性。复杂度$O(n)$
```java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if(array.length==0)
            return 0;
            else
            {
                int min=array[0];
                for(int i=0;i<array.length-1;i++)
                {    
                    if(min>array[i+1])
                    {
                        min=array[i];
                    }
                }
                return min;
            }
    }
}
```
#### 解法二
 - 稍微优化一下,利用旋转特性,旋转分界出前一个元素大于后一个元素，如果没有旋转则输出第一个值，复杂度还是$O(n)$

```java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if(array.length==0)
            return 0;
            else
            {
                for(int i=0;i<array.length-1;i++)
                {    
                    if(array[i]>array[i+1])
                      return array[i+1];
                }
            }
            return array[0];
    }
}
```
#### 解法三
- 二分法查找待补完

---
### NO7. 斐波那契数列
    题目描述
    大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39。
#### 解法一

 - 菲波那切数列第$N$项的值等于第$N-1$和$N-2$项的和，可以用递归解决
 - 递归存在严重的效率问题，在递归树中有很多节点重复，时间复杂度以$n$的指数倍增加。

 ```java
 public class Solution {
    public int Fibonacci(int n) {
    if(n<=0)
        return 0;
    if(n<=1)
        return 1;
    return Fibonacci(n-1)+Fibonacci(n-2);
    }
}
 ```
#### 解法二

 - 循环解决问题，从前往后，注意特殊项1,2
```java
public class Solution {
    public int Fibonacci(int n) {
    int fibN_0 = 1;
    int fibN_1 = 1;
    int fibN_N=0;
    
        if(n==0)
            return 0;
        else if (n==1||n==2)
            return 1;
        else{
        for(int i=3;i<=n;i++)
        {  
           
           fibN_N=fibN_1 + fibN_0;
           fibN_0=fibN_1;
           fibN_1=fibN_N;
        }
      return fibN_N;  
        }
    }
}
```

---
### NO8. 跳台阶1
    题目描述
    一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

 - $1$阶或者$2$阶，那么假定第一次跳的是一阶，那么剩下的是$n-1$个台阶，跳法是$f(n-1)$;假定第一次跳的是$2$阶，那么剩下的是$n-2$个台阶，跳法是$f(n-2)$,由假设可以得出总跳法为: $f(n)=f(n-1)+f(n-2)$.然后通过实际的情况可以得出：只有一阶的时候 $f(1) = 1$ ,只有两阶的时候可以有 $f(2) = 2$.可以发现最终得出的是一个斐波那契数列
 - 注意这个数列前两项与$NO.8$的有区别。

---
### NO9. 跳台阶2
    题目描述
    一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
    

 - 由NO8的分析，$f(n)=f(n-1)+f(n-2)+···+f(0)$,$f(n-1)=f(n-2)+f(n-3)+···+f(0)$，则结合两式由$f(n)=2*f(n-1)$,则可以递归解决，也可以知道这是一个等比数列$f(n)=2^{n-1}$
```java
public class Solution {
    public int JumpFloorII(int target) {
        if(target==1)
            return 1;
        else{
            return 2*JumpFloorII(target-1);
        }
    }
}
```
```java
public class Solution {
    public int JumpFloorII(int target) {
            return (int)Math.pow(2,target-1);
    }
}
```
---
### NO10. 矩形覆盖
    题目描述
    我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？
    
 - 归纳总结发现还是一个斐波那契额数列，跟之前解法一样，不付代码

---
### NO11. 数值的整数次方 
    题目描述
    输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表。

#### 解法一

 - 用逻辑左移避免负数最高位在算数左移时补一造成的死循环。
```java
public class Solution {
    public int NumberOf1(int n) {
        int count=0;
        while(n!=0)
        {
            if((n&1)!=0){
               count++;             
            }
              n=n>>>1; 
        }
        return count;
    }
}
```
#### 解法二
 - 用1不停左移检测每一位
 - 注意$flag！=0$这个判断会在越界时为0，也意味着每次都得循环32或者更多次(取决于系统)
 - 注意 if((n&flag)!=0) 这个判断条件不能写成if((n&flag)==0)，因为按位与的时候当前位为1或0，但结果不一定。

```java
public class Solution {
    public int NumberOf1(int n) {
        int count=0;
        int flag=1;
        while(flag!=0)
        {
            if((n&flag)!=0){
               count++;             
            }
              flag=flag<<1; 
        }
        return count;
    }
}
```
#### 解法三

 - 剑指offer P102

#### 解法四

 - 非常巧妙的一种做法将数字转成字符串，再转成字符数组，统计数组中1的个数。

```java
public class Solution {
    public int NumberOf1(int n) {
        int t=0;
            char[]ch=Integer.toBinaryString(n).toCharArray();
            for(int i=0;i<ch.length;i++){
                if(ch[i]=='1'){
                    t++;
                }
            }
            return t;
    }
}
```

---
### NO12. 数值的整数次方
    题目描述
    给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
    
#### 解法一
 - 注意特殊情况
 - 其实还缺一个特例的判断，即base为0的时候，这时double不能用if(base==0)来判断

```java
public class Solution {
    public double Power(double base, int exponent) {
        double  result=1;
        if(exponent==0)
            return 1;
        for(int i=0;i<Math.abs(exponent);i++){
            result*=base;
        }
        if(exponent<0){
            result=1/result;
        }
        return result;      
  }
}
```

### NO13. 调整数组顺序使奇数位于偶数前面
    题目描述
    输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
    
#### 解法一

 - 非常蠢的方法也是我的第一反应，但时间复杂度为$O(n)$，拿空间换时间

```java
public class Solution {
    public void reOrderArray(int [] array) {
        int count=0;
        int[]  result=new int[array.length];
        for(int i=0;i<array.length;i++)
        {
            if(array[i]%2==1)
            { 
             result[count]=array[i]; 
             count++;
            }
        }
       for(int i=0;i<array.length;i++)
        {
            if(array[i]%2==0)
            {
             result[count]=array[i]; 
             count++;
            }
        }
        for(int i=0;i<array.length;i++)
        {          
             array[i]=result[i];            
        }       
    }
}
```
### NO14. 链表中倒数第k个结点

    题目描述
    输入一个链表，输出该链表中倒数第k个结点。
    

 - 还是和翻转链表一样的思路，用栈来存储链表，不过栈数量多了会溢出，第二遍做的时候看能不能补上其他的解法
 - 注意特殊情况k比链表长度大，或者k为0，或者链表为空

```java
import java.util.Stack;
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        Stack<ListNode> s1=new Stack<ListNode>();
        int count=0;
        if（head==null）
            return null;
        while(head!=null)
        {
            s1.push(head);
            head=head.next;
            count++;
        }
        if(count<k||k==0)
            return null;
        for(int i=0;i<k-1;i++)
        {
            s1.pop();
        }
         return s1.pop();
    }
}
```


----------
### NO15. 反转链表
    题目描述
    输入一个链表，反转链表后，输出新链表的表头。
    

 - 过程见笔记本 之后把图贴上来

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
     ListNode pre=null;
     ListNode next=null;
     if(head==null)
         return null;
      while(head!=null)
      {
          next=head.next;
          head.next=pre;
          pre=head;
          head=next;
      }
        return pre;            
    }
}
```


----------


### NO16. 合并两个排序的链表
    输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

 - 代码鲁棒性，如果list1或者list2输出为null的情况
 - 每次找到最head1.val和head2.val中较小的那个加入merge后的链表，然后剩下的重复此操作，从而递归
 - 存在不递归解法，第二遍做看能不能补上

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        
        if(list1==null)
            return list2;
        else if(list2==null)
            return list1;
        ListNode mList=null;
        if(list1.val<list2.val)
        {
         mList=list1;
         mList.next=Merge(list1.next,list2);
        }
        else{
         mList=list2;
         mList.next=Merge(list1,list2.next);    
        }
        return mList;       
    }
}
```


----------
### NO17. 


---
## LEETCODE

### NO.1 TWO SUM

    给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

    示例:
    给定 nums = [2, 7, 11, 15], target = 9
    因为 nums[0] + nums[1] = 2 + 7 = 9
    所以返回 [0, 1]

#### 解法一

 - 暴力搜索，复杂度$O(n^2)$,第一时间反应的解法。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] result=new int[2];
        for(int i=0;i<nums.length-1;i++)
            for(int j=i+1;j<nums.length;j++)
            {
                if(nums[i]+nums[j]==target){
                    result[0]=i;
                    result[1]=j;
                    return result;
                }
            }
        return result;
    }
}
```

#### 解法二

 - 利用hashmap查询速度时间复杂度一般为$O(1)$的特点，减少时间复杂度。
 - hashmap由于key-value对的特性，所以查找速度快。
 - `map.put(nums[i], i)`,传进key和value参数
 - `Map<Integer, Integer> map = new HashMap<>()`新建一个HashMap
 - `map.containsKey(complement)`找到对应的数组索引
 - `map.get(complement)`取对应的数组索引
 - `throw new IllegalArgumentException("No two sum solution")`没有返回值时抛出异常

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
---
### no.2 ADD TWO NUMBERS
    给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。你可以假设除了数字 0 之外，这两个数字都不会以零开头。

    示例：
    输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
    输出：7 -> 0 -> 8
    原因：342 + 465 = 807

 - 我们首先从最低有效位也就是列表 $l1$ 和 $l2$的表头开始相加。由于每位数字都应当处于$0…9$ 的范围内，我们计算两个数字的和时可能会出现“溢出”。例如，$5 + 7 = 12$。在这种情况下，我们会将当前位的数值设置为 $2$，并将进位 $carry = 1$ 带入下一次迭代。进位$ carry$ 必定是 $0$ 或 1，这是因为两个数字相加（考虑到进位）可能出现的最大和为 $9 + 9 + 1 = 19$。
 - `dummyHead.next`

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
        
    }
}
```
### no.832 Flipping an Image
    给定一个二进制矩阵 A，我们想先水平翻转图像，然后反转图像并返回结果。
    水平翻转图片就是将图片的每一行都进行翻转，即逆序。例如，水平翻转 [1, 1, 0] 的结果是 [0, 1, 1]。
    反转图片的意思是图片中的 0 全部被 1 替换， 1 全部被 0 替换。例如，反转 [0, 1, 1] 的结果是 [1, 0, 0]。

    示例 1:

    输入: [[1,1,0],[1,0,1],[0,0,0]]
    输出: [[1,0,0],[0,1,0],[1,1,1]]
    解释: 首先翻转每一行: [[0,1,1],[1,0,1],[0,0,0]]；
    然后反转图片: [[1,0,0],[0,1,0],[1,1,1]]
#### 解法一

 * 用压栈出栈翻转每一行元素，然后判断翻转。

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        Stack<Integer> s1= new  Stack<Integer>();
        for(int i=0;i<A.length;i++)
        {
            for(int j=0;j<A[0].length;j++)
            {
                s1.push(A[i][j]);                
            }
            for(int j=0;j<A[0].length;j++)
            {
            
              A[i][j]=s1.pop(); 
                
                if(A[i][j]==0)
                    A[i][j]=1;
                else
                    A[i][j]=0;          
            }        
        }
       return A;  
    }
}
```
---
### Array Partition I
    给定长度为 2n 的数组, 你的任务是将这些数分成 n 对, 例如 (a1, b1), (a2, b2), ..., (an, bn) ，使得从1 到 n 的 min(ai, bi) 总和最大。

    示例 1:

    输入: [1,4,3,2]

    输出: 4
    解释: n 等于 2, 最大总和为 4 = min(1, 2) + min(3, 4).
 
 - 分析见 (https://leetcode.com/problems/array-partition-i/discuss/102170/Java-Solution-Sorting.-And-rough-proof-of-algorithm.)
 - 
```java
class Solution {
    public int arrayPairSum(int[] nums) {
         Arrays.sort(nums);
        int result = 0;
        for (int i = 0; i < nums.length; i += 2) {
            result += nums[i];
        }
        return result;
    }
}
```

