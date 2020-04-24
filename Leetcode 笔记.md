# Leetcode 笔记

## Apr 21
### 两数之和

* 题目上有限定同一元素不能使用两遍  **corner case**

* ![1587440250936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1587440250936.png)

* 提速点在于使用哈希值查找（python中为字典）

* 我的代码：

  ```python
  class Solution:
      def twoSum(self, nums: List[int], target: int) -> List[int]:
          the_dict = {}
          for index in range(len(nums)):
              the_dict[nums[index]] = index
          for index in range(len(nums)):
              current_num = nums[index]
              if target-current_num in the_dict:
                  if index != the_dict[target-current_num]:
                      return [index,the_dict[target-current_num]]
  ```

  

* 大佬的代码:

  ```python
  class Solution:
      def twoSum(self, nums, target):
          """
          :type nums: List[int]
          :type target: int
          :rtype: List[int]
          """
          hashmap = {}
          for index, num in enumerate(nums):
              another_num = target - num
              if another_num in hashmap:
                  return [hashmap[another_num], index]
              hashmap[num] = index
          return None
  ```

  

  * enumerate 枚举list中的index和对应的element
  * 因为在把当前元素添加到hash中之前先进行了符合要求的查找，所以相当于**结果去重**
  * in 检验所给值是否存在于字典的键中
    * ![1587440718627](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1587440718627.png)
    * ![1587440733423](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1587440733423.png)

* 学到的点：

  * hash对应的时间复杂度近似等于1
  * 搜索元素的过程可以和添加元素相结合 -> 即 在添加一个元素到缓存之前可以先找找看是不是能达到题目所给的要求

* Java 版本:

  ```Java
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          HashMap<Integer,Integer> myHashMap = new HashMap<Integer,Integer>();
          for (int i=0;i<nums.length;i++)
          {
              int anotherNum = target - nums[i];
              if (myHashMap.get(anotherNum)!=null)
              {
                  int[] answer={myHashMap.get(anotherNum),i};
                  return answer;
              }
              myHashMap.put(nums[i],i);
          }
          return null;
      }
  }
  ```

  

* 注意点：

  * HasnMap 类型不能写<int,int> 要写<Integer,Integer>

* 大佬解法

  ```java
  import java.util.HashMap;
  import java.util.Map;
  
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          int[] indexs = new int[2];
          
          // 建立k-v ，一一对应的哈希表
          HashMap<Integer,Integer> hash = new HashMap<Integer,Integer>();
          for(int i = 0; i < nums.length; i++){
              if(hash.containsKey(nums[i])){
                  indexs[0] = i;
                  indexs[1] = hash.get(nums[i]);
                  return indexs;
              }
              // 将数据存入 key为补数 ，value为下标
              hash.put(target-nums[i],i);
          }
          // // 双重循环 循环极限为(n^2-n)/2 
          // for(int i = 0; i < nums.length; i++){
          //     for(int j = nums.length - 1; j > i; j --){
          //         if(nums[i]+nums[j] == target){
          //            indexs[0] = i;
          //            indexs[1] = j; 
          //            return indexs;
          //         }
          //     }
          // }
          return indexs;
      }
  }
  ```

  

### 两数相加

* ![1587445427401](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1587445427401.png)

  没有处理还有overflow的情况

   ```python
  
   ```



```python
# Definition for singly-linked list.

# class ListNode:

# def __init__(self, x):

# self.val = x

# self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        overflow = 0
        current_digit = l1.val + l2.val
        if current_digit >= 10:
            overflow = 1
            current_digit -= 10
        else:
            overflow = 0
        
    result_list = ListNode(current_digit)
    head = result_list
    while(l1.next!=None or l2.next!= None or overflow != 0):
        if(l1.next!=None):
            l1 = l1.next
        else:
            l1.val = 0
        if (l2.next!=None):
            l2 = l2.next
        else:
            l2.val = 0
        current_digit = l1.val + l2.val + overflow
        if current_digit >= 10:
            overflow = 1
            current_digit -= 10
        else:
            overflow = 0
        the_Node = ListNode(current_digit)
        result_list.next =the_Node
        result_list = the_Node
    return head
```
   ```python

* 大佬的代码：

  class Solution:
      def addTwoNumbers(self, l1, l2):
          """
          :type l1: ListNode
          :type l2: ListNode
          :rtype: ListNode
          """
          re = ListNode(0)
          r=re
          carry=0
          while(l1 or l2):
              x= l1.val if l1 else 0
              y= l2.val if l2 else 0
              s=carry+x+y
              carry=s//10
              r.next=ListNode(s%10)
              r=r.next
              if(l1!=None):l1=l1.next
              if(l2!=None):l2=l2.next
          if(carry>0):
              r.next=ListNode(1)
          return re.next
   ```

  

* 学到的点：
  
* 代码可以写的更加简洁
  
* Java 

* ```java
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
              ListNode result = new ListNode(0);
              ListNode r = result;
              int overflow = 0;
              while(l1 !=null || l2!=null || overflow != 0)
              {
                  int x = l1 != null ? l1.val: 0;
                  int y = l2 != null ? l2.val: 0;
                  int sum = x + y + overflow;
                  overflow = sum / 10;
  
                  ListNode newNode = new ListNode(sum%10);
                  r.next = newNode;
                  r = newNode;
  
                  if(l1!=null)
                  {
                      l1 = l1.next;
                  }
                  if(l2!=null)
                  {
                      l2 = l2.next;
                  }
              }
              return result.next;
      }
  }
  ```

* 复习的点：

  * java中不能直接用 变量本身来当条件判断需要判断变量是否和空相等

  * java中新class对象需要关键字 new, python 则不需要

    
## Apr 22nd

### Q4

* python3 中使用**int**将一个数字转化为整数
* 在同时有两个条件需要满足的时候，可以先假设第一个条件满足了，而后依据第二个条件的要求更改第一个条件中具体变量的取值
* 先考虑一般情况 再考虑特殊情况



### Q5

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        reslut_list = ""
        tmp_list = ""
        if len(s) < 2:
            return s
        for i in range(len(s)-1):
            #考虑字符串里没有相同元素情况
            tmp_list = ""
            tmp_list=tmp_list+s[i]
            #偶回文串
            if s[i] == s[i+1]:
                index = 1
                while((i-index)>-1 and (i+1+index)<len(s)):
                    # if the current character is not same as the corresponding character on the other side
                    if(s[i-index]!=s[i+1+index]):
                        break
                    index +=1
                tmp_list = s[i-index+1:i+1+index]
            #需考虑两种都满足
            if len(reslut_list)<len(tmp_list):
                reslut_list = tmp_list
                
            #奇回文串
            if i+2<len(s):
                if s[i] == s[i+2]:
                    index = 1
                    while((i-index)>-1 and (i+2+index)<len(s)):
                        if(s[i-index]!=s[i+2+index]):
                            break
                        index+=1
                    tmp_list = s[i-index+1:i+2+index]
            if len(reslut_list)<len(tmp_list):
                reslut_list = tmp_list
        result = str(reslut_list)
        if len(reslut_list)==0:
            result = ""          
        return result
```



### Q6

* 我的解法

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows ==1 :
            return s
        z_shape_list = []
        # if cur_mode == 0, then goes down, if cur_mode == 1, then goes diagonal
        cur_mode = 0
        j = 0
        res_str = ""
        for i in range(numRows):
            z_shape_list.append([])
        for i in range(len(s)):
            z_shape_list[j].append(s[i])
            if j == numRows -1 :
                cur_mode = 1
            elif j == 0:
                cur_mode = 0
            if cur_mode == 0:
                j += 1
            else:
                j -= 1
        for i in range(numRows):
            for j in z_shape_list[i]:
                res_str+=j
            #res_str += str(z_shape_list[i])
        return res_str
        
```

* 大佬的解法

* ```java
  class Solution {
      public String convert(String s, int numRows) {
  
          if(numRows == 1) return s;
          int[] rowIdx = new int[numRows];
          char[] chars = new char[s.length()];
          int n = 0;
          int burketSize = numRows * 2 - 2;
          int burketNum = chars.length / burketSize; 
          int rem = chars.length % burketSize;
          for(int i = 1; i < numRows; i ++){
          	int flag = i == 1 ? 1 : 2;
          	n = flag * burketNum + (rem >= i ? ( 1 + (burketSize - rem + 1 < i ? 1 : 0)) : 0);
          	rowIdx[i] = rowIdx[i-1] + n;
          }
          int flag = -1;
          int curRow = 0;
          for(char c : s.toCharArray()){
          	chars[rowIdx[curRow]] = c;
          	rowIdx[curRow] = rowIdx[curRow] + 1;
          	 if (curRow == 0 || curRow == numRows - 1) flag = -flag;
               curRow += flag;
          }
          return new String(chars);
      }
  }
  ```

### Q7

* 我的解法：

```python
class Solution:
    def reverse(self, x: int) -> int:
        tmp_str = str(x)
        print(tmp_str[0])
        cur_str = ""
        i  = len(tmp_str)-1
        while(i>-1):
            if(i == len(tmp_str) and tmp[i] == 0):
                continue
            cur_str+=tmp_str[i]
            i -= 1
            if (tmp_str[i] == "-"):
                #print("minus here")
                cur_str = "-"+ cur_str
                break
        result = int(cur_str)
        if (result >  2147483647 or result < -2147483648 ):
            return 0
        #print(result)
        return result
```

* 大佬的

```java
public int reverse(int x) {
        long n = 0;
        while(x != 0) {
            n = n*10 + x%10;
            x = x/10;
        }
        return (int)n==n? (int)n:0;
    }
```

```c
int reverse(int x)
{
    int max = 0x7fffffff, min = 0x80000000;//int的最大值最小值
    long rs = 0;//用long类型判断溢出
    for(;x;rs = rs*10+x%10,x/=10);//逆序，正负通吃，不用单独考虑负值
    return rs>max||rs<min?0:rs;//超了最大值低于最小值就返回0
}
```



### Question 8

```python
class Solution:
    def myAtoi(self, str: str) -> int:
        #空输入的检测
        tmp_str = ""
        marker_num_start = False
        marker_sign_appear = False
        for i in str:
            #print(i)
            if i ==" " and marker_num_start == True:
                if len(tmp_str) == 0 or self.is_sign(tmp_str) == True:
                    return 0
                break
                return int(tmp_str)
            if i != " ":
                # the string strats with a non number and not a sign symbol
                if self.is_sign(i)==False and self.is_num(i) == False and marker_num_start == False:
                    return 0
                # the string strats with a sign
                if self.is_num(i) == False:
                    if marker_num_start == True:
                        if len(tmp_str) == 0 or self.is_sign(tmp_str) == True:
                            return 0
                        break
                        return int(tmp_str)
                    if self.is_sign(i) and marker_sign_appear == False:
                        marker_sign_appear = True
                    else:
                        if len(tmp_str) == 0 or self.is_sign(tmp_str) == True:
                            return 0
                        break
                        return int(tmp_str)
                # entering this line means that the current character is a number
                marker_num_start = True
                tmp_str+=i
        if len(tmp_str) == 0 or self.is_sign(tmp_str) == True:
            return 0
        tmp_int = int(tmp_str)
        if tmp_int > 2147483647:
            return 2147483647
        if tmp_int < -2147483648:
            return -2147483648
        return tmp_int    
    def is_num(self,x):
        if(x<"0" or x>"9"):
            return False
        return True
    def is_sign(self,x):
        if x!="+" and x!="-":
            return False
        return True
```







### Question 9

```python
def isPalindrome(x):
    if x < 0 :
        return False
    tmp_str = str(x)
    i = len(tmp_str)-1
    j = 0
    #未考虑到i-j=1的情况两个数字不同
    while(i-j>1):
        if tmp_str[i]!=tmp_str[j]:
            return False
        i -= 1
        j += 1
    return True
```


