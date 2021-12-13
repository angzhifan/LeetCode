# LeetCode
My own solutions to some LeetCode problems

42. Trapping Rain Water

'''Python
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
'''

