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

    

