## My LeetCode Solutions
In this repository, I posted some of my solutions to some LeetCode problems if I think they are interesting and have good running time or space complexity. 
I used the LeetCode-China website, so some descriptions are in Chinese.


### Python3 Code 





剑指 Offer 51. 数组中的逆序对  LCOF


执行用时：
1120 ms
, 在所有 Python3 提交中击败了
70.89%
的用户
内存消耗：
26 MB
, 在所有 Python3 提交中击败了
41.12%
的用户


```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        def merge(start, end):
            if end-start<=1:
                return 0
            n1 = int((end-start)/2)
            n2 = end-start-n1
            cnt = merge(start, start+n1)+merge(start+n1, end)
            i1, i2, nums1 = 0, 0, nums[start:start+n1]
            while i1<n1 and i2<n2:
                if nums1[i1]<=nums[start+n1+i2]:
                    nums[start+i1+i2] = nums1[i1]
                    i1 += 1
                else:
                    nums[start+i1+i2] = nums[start+n1+i2]
                    cnt += (n1-i1)
                    i2 += 1
            if i1<n1:
                nums[(start+i1+n2):end] = nums1[i1:n1]
            return cnt
        return merge(0,len(nums))
```


260. Single Number III


执行用时：
24 ms
, 在所有 Python3 提交中击败了
99.09%
的用户
内存消耗：
16.1 MB
, 在所有 Python3 提交中击败了
36.87%
的用户

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> List[int]:
        diff = reduce(lambda x,y: x^y, nums)
        i = (diff&(-diff)).bit_length()-1
        num1 = reduce(lambda x,y: x^y if (y>>i)&1 else x, nums) 
        if not (nums[0]>>i)&1:
            num1 = num1^nums[0]
        return [num1, diff^num1]
```


1567. Maximum Length of Subarray With Positive Product

执行用时：
84 ms
, 在所有 Python3 提交中击败了
99.52%
的用户
内存消耗：
24.6 MB
, 在所有 Python3 提交中击败了
97.61%
的用户

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        output, l0, l1 = 0, 0, 0
        for i in nums:
            if i>0:
                l0, l1 = l0+1, (l1+1) if l1>0 else 0
            elif i<0:
                l0, l1 = (l1+1) if l1>0 else 0, l0+1 
            else:
                l0, l1 = 0, 0
            if l0>output: output = l0
        return output
```

105. Construct Binary Tree from Preorder and Inorder Traversal

执行用时：
28 ms
, 在所有 Python3 提交中击败了
99.71%
的用户
内存消耗：
15.6 MB
, 在所有 Python3 提交中击败了
98.64%
的用户
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        n, i1, i2, root = len(preorder), 1, 0, TreeNode(preorder[0])
        stack, node = [root], root
        while i1<n:
            if stack and stack[-1].val==inorder[i2]:
                while stack and stack[-1].val==inorder[i2]: node, i2 = stack.pop(), i2+1
                node.right = TreeNode(preorder[i1])
                node = node.right
            else:
                node.left = TreeNode(preorder[i1])
                node = node.left
            stack.append(node)
            i1 += 1
        return root
```



10. Regular Expression Matching

执行用时：
40 ms
, 在所有 Python3 提交中击败了
94.11%
的用户
内存消耗：
15 MB
, 在所有 Python3 提交中击败了
77.69%
的用户
```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        n, m = len(s), len(p)
        p1, p2 = 0, [-1]
        while p1<n and p2:
            temp = []
            for i in p2:
                while i<m-2 and p[i+2]=="*":
                    temp.append(i+2)
                    i += 2
            p2.extend(temp)      
            new_p2 = []
            for i in p2:
                if i>=1 and p[i]=="*" and (p[i-1]=="." or p[i-1]==s[p1]):
                    new_p2.append(i)
                if i<m-1 and (p[i+1]=="." or p[i+1]==s[p1]):
                    if i<m-2 and p[i+2]=="*":
                        new_p2.append(i+2)
                    else:
                        new_p2.append(i+1)
            p2 = list(set(new_p2))
            p1 += 1
        temp = []
        for i in p2:
            while i<m-2 and p[i+2]=="*":
                temp.append(i+2)
                i += 2
        p2.extend(temp) 
        if p1 == n and m-1 in p2:
            return True 
        else:
            return False
```

42. Trapping Rain Water

执行用时：
32 ms
, 在所有 Python3 提交中击败了
97.76%
的用户
内存消耗：
15.8 MB
, 在所有 Python3 提交中击败了
53.46%
的用户

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)
        if n<=2:
            return 0 
        if height[0]<=height[1]:
            i = 0
            while i<n-1 and height[i]<=height[i+1]:
                i += 1
            return self.trap(height[i:])
        if height[n-1]<=height[n-2]:
            i = n-1
            while i>0 and height[i]<=height[i-1]:
                i -= 1 
            return self.trap(height[:(i+1)])
        if max(height[:int(n/2)])>max(height[(n-int(n/2)):]):
            return self.trap(height[::-1])
        high = [height[0], 0]
        for i in range(1,n):
            if height[i]>=high[0]:
                if i <int(n/2):
                    high = [height[i],i]
                else:
                    temp = high[0]*(i-high[1]-1)-sum(height[(high[1]+1):i])
                    return temp+self.trap(height[:(high[1]+1)])+self.trap(height[i:])
```

