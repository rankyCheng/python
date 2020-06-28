#关于数组,链表以及双指针的应用场景分析
##数组array:顺序存储结构,在创建数组时必须要指定数组的容量大小，再根据数组容量来分配内存。
    优点:数组的内存是连续的,查询效率很高
    缺点:更新插入需要遍历,因此效率低
    适用场景:数据总量小;对数据的插入和删除操作较少.

##链表linklist:链式存储结构,内存分配可连续或不连续,新增节点可以分布在内存未被占用的任何位置
    优点:插入删除操作不需要将后面的值重新赋值,因此效率高;不需要预先分配内存
    缺点:读取查询效率低,需要遍历.
    适用场景:存储及读取操作较少,对插入和删除操作较多.
 
##双指针:1.快慢指针:定义两根指针，同一侧开始遍历数组,移动的速度一快一慢，由此来制造出差值，让我们找到相应的节点。
         适用: 常用于判断链表等数据结构中是否有环,或者是否存在重复(或同时存在其他特殊条件)
               1. 移除指定元素:
                  快指针寻找与指定元素相同的值,慢指针前进的前提是快指针发现了不同值
                  class Solution:
                    def removeElement(self, nums: List[int], val: int):
                      slow = 0
                      fast = 0
                      while fast < len(nums):
                        if nums[fast] == val:
                          fast += 1
                        else:
                          nums[slow] = nums[fast]
                          fast += 1
                          slow += 1
                      return slow
                      
               2. 移动零: 
                  快指针找到零,当快指针发现非零值时慢指针前进,将零移动到数组最后.
                  class Solution1:
                    def removeElement(self, nums, val):
                      slow = 0
                      fast = 0
                      while fast < len(nums):
                        if nums[fast] == 0:
                          fast += 1
                        else:
                          nums[slow] = nums[fast]
                          slow += 1
                          fast += 1
                      for i in range(slow,len(nums)):
                         nums[i]=0
                         
               3. 判断链表有无环: 
                  快指针前进两步,慢指针前进一步,当二者相遇时,说明有环.
                  class Node:
                    def __init__(self,data):
                      self.data=data
                      self.next=None

                    def is_circle(head):
                      curr=head
                      prev=head
                      while curr and curr.next:
                        prev=prev.next
                        curr=curr.next.next
                        if prev==curr:
                          return True
                      return False
                                          
               4. 有序数组去重: 
                  当快指针与慢指针相等时,快指针进一位,否则慢指针进一位,且将快指针赋值给慢指针.最终取慢指针+1为不重复值数.
                  class Solution:
                    def removeDuplicated(self, nums: List[int]) -> int:
                      slow = 0
                      fast = 1
                      while fast < len(nums):
                        if nums[fast] == nums[slow]:
                          fast += 1
                        else:
                          slow += 1
                          nums[slow] = nums[fast]
                          fast += 1
                      return slow + 1
                      
               5. 删除重复元素(最多允许两次):
                  在数组去重基础上添加一个求和项count,当count=2时,跳过,否则移动i.
                  class Solution:
                    def removeDuplicated(self, nums: List[int]) -> int:
                      count = 1
                      slow = 0
                      fast = 1
                      while fast < len(nums):
                        if nums[slow] == nums[fast]:
                          count += 1
                        if count == 2:
                          slow += 1
                          nums[slow] = nums[fast]
                          fast += 1
                        else:
                          slow += 1
                          nums[slow] = nums[fast]
                          count = 1
                          fast += 1
                      return slow + 1
                      
      2.对撞指针:定义两根指针,一头一尾,从两头向中间遍历.
          适用:涉及到各种数字,重量等等的和
              1. 两数之和: 
                 将数组排序--定义两个指针(left,right)--
                 当二者和小于目标值时,left+1,当二者和大于目标值时,right-1,相等时,返回下标志
                 class Solution:
                   def twosum(self, nums: List, target: int):
                      n1=nums[:]
                      nums.sort()
                      left = 0
                      right = len(nums) - 1
                      while left < right:
                          cur = nums[left] + nums[right]
                        if cur == target:
                            left=nums[left]
                            right=nums[right]
                            break
                        else:
                          if cur < target:
                              left += 1    
                          else:
                              right -= 1
                      x=n1.index(left)
                      y=n1.index(right)

                      if x<y:
                          return [x,y]
                      else:
                          return [y,x]
                          
              2. 三数之和: 
                 数组排序--定义两个指针(left,right)和一个变量i表C位值(表示数组的下标)--
                 在left<right的情况下,当三数相加小于目标数,left+1,当三数相加大于目标数,则right-1,否则返回下标值--
                 去重,如果当前C位数和相邻的数相等,直接移动指针
                 def threeSum(nums):                  
                    nums.sort()
                    result = []
                    for i in range(len(nums) - 2):
                        if i > 0 and nums[i] == nums[i - 1]:  
                            continue
                        else:
                            left = i + 1
                            right = len(nums) - 1
                        while left < right:
                            if nums[i] + nums[left] + nums[right] > 0:
                                right -= 1
                            elif nums[i] + nums[left] + nums[right] < 0:
                                left += 1
                            else:
                                result.append([nums[i], nums[left], nums[right]])
                                while left<right and nums[left]==nums[left+1]:
                                    left+=1
                                while left < right and nums[right] == nums[right-1]:
                                    right-=1
                                left += 1
                                right -= 1
                    return result
                    
              3. 三数之和(最接近): 
                 数组排序--先定义一个最近距离为前三个数之和,目标值--
                 定义两个指针(left,right)和一个变量i表C位值(表示数组的下标)--
	               在left<right的情况下,在当前i位置的数值情况下,搜索最接近的组合--
                 与之前的最近距离比较,如果更近则更新
                 def threeSumClosest(nums: List[int],target: int)->int:
                    nums.sort()
                    min=abs(nums[0]+nums[1]+nums[2]-target)
                    result=nums[0]+nums[1]+nums[2]
                    for i in range(len(nums)-2):
                        left=i+1
                        right=len(nums)-1
                        while left<right:
                            sum=nums[i]+nums[left]+nums[right]
                            if abs(sum-target)<min:
                               min=abs(sum-target)
                               result=sum
                            if sum>target:
                                right-=1
                            elif sum<target:
                                left+=1
                            else:
                                return sum
                    return result
                    
              4. 二分查找: 分析二分查找的一个技巧是：不要出现 else，而是把所有情况用 else if 写清楚，这样可以清楚地展现所有细节。
                  定义左右指针分别为left=-1,right=len(list)--
                  取左右指针的中位整数mid,当mid对应值大于目标值时,将mid指向右指针;当mid对应值小于目标值时,将mid指向左指针--
                  重复操作,当mid等于目标值,返回
                  class Solution:
                    def search(self, nums: List[int], target: int) -> int:
                        left=-1
                        right=len(nums)
                        while left<=right:
                            mid=(left+right)//2
                            if target not in nums:
                                return -1
                            elif target<nums[mid]:
                                right=mid
                            elif target>nums[mid]:
                                left=mid
                            else:
                                return mid
    
      3.分离指针: 不需要额外空间。
          适用: 两个数组间的合并或取交等等操作
              1.合并两个有序数组:
              定义两个指针i=m-1,j=n-1,(m,n分别为两个数组的长度),k用于追踪添加元素的位置--
              比较得到两个数组最大的元素，作为最大值放在数组的最后--
              需要注意处理i或j取到0时,仍然要赋值
              def mergetwo(nums1, m, nums2, n):
                i = m - 1
                j = n - 1
                k = m + n - 1
                while i >= 0 and j >= 0:
                    if nums1[i] >= nums2[j]:
                        nums1[k] = nums1[i]
                        i -= 1
                        # k-=1
                    elif nums1[i] < nums2[j]:
                        nums1[k] = nums2[j]
                        j -= 1
                        # k-=1
                    k -= 1
                while i>=0:    #处理尾巴
                    nums1[k]=nums1[i]
                    i-=1
                    k-=1
                while j>=0:    
                    nums1[k]=nums2[j]
                    j-=1
                    k-=1
                return nums1
                
              2.两个数组的交集:
                两个数组分别排序,同时定义去重集合(避免交集中存在重复数字)--
                定义两个指针i,j分别为0,在i,j都小于各自数组长度的前提下--
                若两个相等,则加入到新的集合中,当i代表的值大于j代表的值,则j+1,否则i+1--
                返回新的集合
                def intersection(nums1: List[int],nums2: List[int]):
                  nums1.sort()
                  nums2.sort()
                  i,j=0,0
                  nums_set=set()  #去重
                  while i<len(nums1) and j<len(nums2):
                      if nums1[i]<nums2[j]:
                          i+=1
                      elif nums1[i]>nums2[j]:
                          j+=1
                      elif nums1[i]==nums2[j]:
                          nums_set.add(nums1[i])
                          i+=1
                          j+=1
                  return list(nums_set)
              
              
              
              
              
              
