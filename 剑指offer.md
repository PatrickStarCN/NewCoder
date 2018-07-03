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
```JAVA
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

```JAVA
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
```
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

