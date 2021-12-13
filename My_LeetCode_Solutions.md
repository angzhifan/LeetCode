# LeetCode
My own solutions to some LeetCode problems

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
```
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

```c
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

