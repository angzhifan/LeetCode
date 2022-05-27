## My LeetCode Solutions
In this repository, I posted some of my solutions to some LeetCode problems if I think they are interesting and have good running time or space complexity. 
I used the LeetCode-China website, so some descriptions are in Chinese.


### Python3 Code 

287. Find the Duplicate Number

执行用时：
92 ms
, 在所有 Python3 提交中击败了
80.67%
的用户
内存消耗：
25.9 MB
, 在所有 Python3 提交中击败了
74.17%
的用户

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        p1, p2 = nums[0], nums[nums[0]]
        while p1!= p2:
            p1, p2 = nums[p1], nums[nums[p2]]
        p1 = 0
        while p1!= p2:
            p1, p2 = nums[p1], nums[p2]
        return p1
```

程序员面试金典（第 6 版）面试题 08.14. Boolean Evaluation LCCI

执行用时：
48 ms
, 在所有 Python3 提交中击败了
95.15%
的用户
内存消耗：
14.9 MB
, 在所有 Python3 提交中击败了
100.00%
的用户

```python
class Solution:
    def countEval(self, s: str, result: int) -> int:
        def eval(symbol, cnt1, cnt2, cnt):
            if symbol=="^": return [cnt[0]+cnt1[0]*cnt2[0]+cnt1[1]*cnt2[1],cnt[1]+cnt1[0]*cnt2[1]+cnt1[1]*cnt2[0]]
            if symbol=="|": return [cnt[0]+cnt1[0]*cnt2[0],cnt[1]+cnt1[0]*cnt2[1]+cnt1[1]*cnt2[0]+cnt1[1]*cnt2[1]]
            if symbol=="&": return [cnt[0]+cnt1[0]*cnt2[0]+cnt1[0]*cnt2[1]+cnt1[1]*cnt2[0],cnt[1]+cnt1[1]*cnt2[1]]

        n = (len(s)+1)//2
        if n==0: return 0
        table = [[[] for j in range(i,n)] for i in range(n)]
        for i in range(n): table[i][0] = [int(s[i*2])^1, int(s[i*2])^0]
        for k in range(1,n):
            for i in range(n-k):
                cnt = [0, 0]
                for j in range(1, k+1): cnt = eval(s[i*2+j*2-1], table[i][j-1], table[i+j][k-j], cnt)
                table[i][k]=cnt 
        return table[0][n-1][result]
```


324. Wiggle Sort II

执行用时：
76 ms
, 在所有 Python3 提交中击败了
29.57%
的用户
内存消耗：
16.1 MB
, 在所有 Python3 提交中击败了
25.81%
的用户

The popular "three_way_partition" solution to this Leetcode problem has worst case time complexity O(n^2).
My solution always has time complexity O(n), space complexity O(1). So I still posted my solution here although
its time cost and space cost for its test examples only beat 20+% of other submissions.

```python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # Step1. Greedily make a wiggle-sorted list at the beginning of "nums"
        n, p1 = len(nums), 0
        for i in range(n-1):
            if nums[i]==nums[i+1]:
                p1 = max(p1, i+2)
                while p1<n and (nums[p1]==nums[i] or (i%2==0 and nums[i]>nums[p1] and i>0 and nums[p1]==nums[i-1]) or (i%2==1 and nums[i]<nums[p1] and nums[p1]==nums[i-1])): p1 += 1
                if p1<n: nums[i+1], nums[p1] = nums[p1], nums[i+1]
            if (i%2==0 and nums[i]>nums[i+1]) or (i%2==1 and nums[i]<nums[i+1]):
                nums[i], nums[i+1] = nums[i+1], nums[i]
        # Step2. To make the tail of "nums" wiggle-sorted as well, 
        # need to find more numbers which will be used in the next step
        cnt1, cnt2, cnt3, p2, t = 0, 1, 0, n-2, nums[n-1]
        while p2>=0 and (cnt1<cnt2 and cnt3<cnt2 and cnt1+cnt3<=cnt2):
            if nums[p2]==t: cnt2 += 1
            elif nums[p2]<t: cnt1 += 1
            else: cnt3 += 1
            p2 -= 1
        # Step3. Process the tail of "nums". Find positions for those numbers which equal to nums[n-1]
        if p2==-1 and cnt1*cnt3>0: nums[0], nums[-1], p2 = nums[-1], nums[0], 0
        p3, p4, tmp, s = p2+1, n-1, cnt1-1 if (p2+1)%2==0 else cnt3-1, -1 if (p2+1)%2==0 else 1 
        if cnt1==0: t_indices = list(range(p2+1, n, 2)) if s==-1 else list(range(p2+2, n, 2))
        elif cnt3==0: t_indices = list(range(p2+2, n, 2)) if s==-1 else list(range(p2+1, n, 2))
        else:  
            t_indices = list(range(p2+2, p2+2+2*tmp, 2))+list(range(p2+3+2*tmp, n, 2))
        ### Move those numbers to the end
        l_t_indices = len(t_indices)
        while p3<p4:
            while nums[p4]==t: p4 -= 1
            while nums[p3]!=t: p3 += 1
            if p3<p4: nums[p3], nums[p4] = nums[p4], nums[p3]
        ### Then move them to assigned positions
        for i in range(l_t_indices):
            nums[t_indices[i]], nums[-l_t_indices+i] = nums[-l_t_indices+i], nums[t_indices[i]]
        if cnt1*cnt3==0: return nums
        # Step4. If there are both smaller and bigger ones, may need to switch them to complete the process.
        p1, p3 = p2+1, n-1 if n-1!=t_indices[-1] else n-2
        while p1<p3:
            while p1<p2+2+2*tmp and (nums[p1]-t)*s>0: p1 += 2
            while p3>p2+3+2*tmp and (nums[p3]-t)*s<0: p3 -= 2
            if p1<=p2+1+2*tmp and p2+2+2*tmp<=p3: 
                nums[p1], nums[p3] = nums[p3], nums[p1]
            p1, p3 = p1+2, p3-2
        return nums
```

188. Best Time to Buy and Sell Stock IV

执行用时：
40 ms
, 在所有 Python3 提交中击败了
99.25%
的用户
内存消耗：
15.5 MB
, 在所有 Python3 提交中击败了
65.06%
的用户

I thought about dynamic programming, but I later came up with a faster greedy algorithm, which is displayed as follows.

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        import heapq
        n = len(prices)
        if n==0 or k==0: return 0
        # Simplify the "prices" array
        cnt_d, d, old_turn, s, h, profit = 0, dict(), prices[0], 1, [], 0
        for i in range(1,n-1):
            if s==1:
                if prices[i-1]<=prices[i] and prices[i]>prices[i+1] and prices[i]>old_turn:
                    heappush(h, (prices[i]-old_turn, cnt_d, 1))
                    d[cnt_d] = [cnt_d, prices[i]-old_turn, 1]
                    old_turn, cnt_d, s = prices[i], cnt_d+1, -1
                elif prices[i]<old_turn: old_turn = prices[i]
            elif prices[i-1]>=prices[i] and prices[i]<prices[i+1] and prices[i]<old_turn:
                heappush(h, (old_turn-prices[i], cnt_d, 1))
                d[cnt_d] = [cnt_d, prices[i]-old_turn, 1]    
                old_turn, cnt_d, s = prices[i], cnt_d+1, 1
        if prices[n-1]>old_turn:
            heappush(h, (prices[n-1]-old_turn, cnt_d, 1))
            d[cnt_d], cnt_d = [cnt_d, prices[n-1]-old_turn, 1], cnt_d+1
        # Merge consecutive intervals using greedy algorithm
        cnt = (cnt_d+1)//2
        while cnt>k:
            tmp = heappop(h) 
            while d[tmp[1]][2]!=tmp[2]: tmp = heappop(h)            
            right, left = tmp[1]+tmp[2], tmp[1]-1
            if right>=cnt_d or d[right][2]==-1:
                if tmp[1]%2==0: cnt -= 1
                d[tmp[1]][2] = -1
                continue
            while left>=0:
                if left>d[left][0]: left = d[left][0]
                else: break
            if tmp[1]==0 or d[left][2]==-1:
                if tmp[1]%2==0:cnt -= 1
                if tmp[1]==0:d[0][2]=-1
                continue
            d[left] = [left,d[left][1]+d[right][1]+(-tmp[0] if tmp[1]%2 else tmp[0]),right+d[right][2]-left]
            heappush(h, (abs(d[left][1]), left, d[left][2]))
            d[tmp[1]], d[right], cnt = [left, d[tmp[1]][1], -1], [left,d[right][1],-1], cnt-1
        while h:
            tmp = heappop(h)
            if tmp[1]%2==0 and d[tmp[1]][0]==tmp[1] and d[tmp[1]][2]==tmp[2]: profit += d[tmp[1]][1]
        return profit
```


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

