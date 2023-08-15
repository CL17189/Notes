# DS
## 递归和列表
1. two sum
>way1:扫描 o($n^2$)
>way2:hash table o(n)   找到另外一个数的方法从扫描找值变成按值找位置（hash table）
``` python
class Solution:

    def twoSum(self, nums: List[int], target: int) -> List[int]:

        dic={}

        for i, num in enumerate(nums):

            diff=target-num

            if diff in dic:

                return [dic[diff],i]

            dic[num]=i
```
88. merge list
>way1: 合并+排序 o(nlogn)
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        nums1[m:] = nums2[:n]
        nums1.sort()
```
>way2:三个指针 o(n+m)
```PYTHON
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        i, j, k = 0, 0, 0
        while i < m and j < n: #一般情况
            if nums1[i] < nums2[j]:
                nums1[k] = nums1[i]
                i += 1
            else:
                nums1[k] = nums2[j]
                j += 1
            k += 1 # k只记录循环的次数
		# 别忘了两种退化情况
        while i < m: # n=0
            nums1[k] = nums1[i]
            i += 1
            k += 1

        while j < n: # m=0
            nums1[k] = nums2[j]
            j += 1
            k += 1
```

283. 移动0
>way1:单指针 循环+重排 o($n^2$) o(n)
>way2:双指针+交换
>用指针j去获取第一个0的位置，逐步把0交换到后面去（因为0没有顺序）

```PYTHON
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        j = 0 
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[i], nums[j] = nums[j], nums[i]
                j += 1

```

## 链表
21. 合并有序链表
>way1：双指针（省空间）

141. 检测链表环
>way1：记录轨迹  o($n^2$) o(n)
>way2：快慢指针  o(n) o（1）

```PYTHON
class Solution:
	# 记录轨迹
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        trace=[]
        while head:
            for i in trace:
                if i is head:
                    return True
            trace.append(head)
            head=head.next

    #不用担心会死循环，因为两种情况都会有终止条件

    #快慢指针
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if not head:
            return False
        slow = head
        fast = head.next
        while fast and fast.next:
            if slow == fast:
                return True
            slow = slow.next
            fast = fast.next.next
        return False
```
160. 链表交点
Floyd 的龟兔算法不再适用，它并没有考虑链表A和链表B的长度差，这可能导致在长度差很大时，算法会在第一次循环就结束而无法找到交点。
>way：首位相连消除步差

```PYTHON
class Solution:

    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        if not headA or not headB:
            return None

        pa, pb = headA, headB
        while pa is not pb:
            pa = pa.next if pa else headB
            pb = pb.next if pb else headA
            #相当于取最大公倍数，校正步进差
        return pa
```
234. 回文链表
>way：快慢指针找中点+链表翻转+对比

```python
class Solution:

    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if not head or not head.next:
            return True
        #快慢指针找中点
        slow=fast=head
        while fast and fast.next:
            slow=slow.next
            fast=fast.next.next
        #翻转
        pre=None
        cur=slow
        while cur:
            temp=cur.next
            cur.next=pre
            pre=cur
            cur=temp
        #对比
        first=head
        second=pre
        while second:
            if first.val!=second.val:
                return False
            first=first.next
            second=second.next
        return True
```

