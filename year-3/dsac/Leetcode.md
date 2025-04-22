# Add two numbers
```python
# Definition for singly-linked list.
# class ListNode(object):
# def __init__(self, val=0, next=None):
# self.val = val
# self.next = next
class Solution(object):
	def addTwoNumbers(self, l1, l2):
	"""
	:type l1: Optional[ListNode]
	:type l2: Optional[ListNode]
	:rtype: Optional[ListNode]
	"""
		dummy = ListNode()
		res = dummy
		total = carry = 0

		while l1 or l2 or carry:
			total = carry
			if l1:
				total += l1.val
				l1 = l1.next
			if l2:
				total += l2.val
				l2 = l2.next

			num = total % 10
			carry = total // 10
			dummy.next = ListNode(num)
			dummy = dummy.next

		return res.next
```


# Reverse Number
```python
class Solution(object):
	def reverse(self, x):
	"""
	:type x: int
	:rtype: int
	"""
		rev = 0
		sign = -1 if x < 0 else 1
		x = x * -1 if x < 0 else x
		while x != 0:
			digit = x % 10
			x = x // 10
			if rev > (2**31 - 1) // 10:
				return 0
			rev = rev * 10 + digit

		return sign * rev
```

# ATOI
```python
class Solution(object):
    def myAtoi(self, s):
        """
        :type s: str
        :rtype: int
        """
        s = s.strip()
        sign = 1
        num = 0
		if not s:
			return 0


        if s[0] == "-":
            sign = -1
            s = s[1:]
        elif s[0] == "+":
            sign = 1
            s = s[1:]

        for i in s:
            if i.isdigit():
                num = num * 10 + int(i)
            else:
                break

        num *= sign

        if num > 2**31 - 1:
            return 2**31 - 1
        elif num < -(2**31):
            return -(2**31)

        return num

```


# Water problem
```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        l, r = 0, len(height) - 1
        max_water = 0

        while l < r:
            w = r - l
            h = min(height[l], height[r])
            max_water = max(max_water, w * h)

            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        return max_water

```

# Delete nth node from a linked list
```python
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: Optional[ListNode]
        :type n: int
        :rtype: Optional[ListNode]
        """

        dummy = ListNode(0)
        dummy.next = head
        first = dummy
        second = dummy

        for _ in range(n + 1):
            print(second.val)
            first = first.next

        while first is not None:
            first = first.next
            second = second.next

        second.next = second.next.next

        return dummy.next

```

# Generate parens
```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        def generate(left, right, s):
            if len(s) == n * 2:
                parens.append(s)
                return
            
            if left < n:
                generate(left + 1, right, s + "(")

            if right < left:
                generate(left, right + 1, s + ")")

        parens = []
        generate(0,0, "")
        return parens

```

# Swap adjacent nodes in linked list
```python
class Solution(object):
    def swapPairs(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        
        if not head or not head.next:
            return head
        
        dummy = head.next
        head.next = self.swapPairs(dummy.next)
        dummy.next = head

        return dummy
```

# Find the starting and ending index of a value in an array
```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """

        def binarySearch(nums, x):
            left = 0
            right = len(nums)

            while left < right:
                mid = (left + right) // 2

        
                if nums[mid] < x:
                    left = mid + 1
                else:
                    right = mid 

            return left

        low = binarySearch(nums, target)
        high = binarySearch(nums, target+1) - 1

        if low <= high:
            return [low, high]

        return [-1, -1]
```

# Check if BST is valid
```python
class Solution(object):
    def isValidBST(self, root):
        """
        :type root: Optional[TreeNode]
        :rtype: bool
        """

        def valid(node, left, right):
            if not node:
                return True
            
            if not (node.val < right and node.val > left):
                return False

            return (valid(node.left, left, node.val) and valid(node.right, node.val, right))

        return valid(root, float("-inf"), float("inf"))
```

# Word Break
## Intuition

When you first see the problem, it feels natural to try to build the string piece by piece using words from the dictionary. The idea is:

“Can I match a word at the beginning? If so, can I recursively break the rest of the string too?”

This leads us to a recursive solution, where at each step we try all words in the dictionary and check whether they match the current position in the string.

However, since there can be many overlapping subproblems (e.g., checking the same substring multiple times), we should cache the results using memoization to avoid redundant work.

## Approach

Convert wordDict to a set for O(1) lookups.

Use a helper recursive function backtrack(index):

It returns True if the substring s[index:] can be broken into valid words.

At each recursive step:

Loop through each word in the dictionary.

If the word matches the current position (s.startswith(word, index)), recurse on the remaining string from index + len(word).

Use a memo dictionary to cache results for each starting index.

Base case: if index == len(s), return True (we've used the entire string successfully).
```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        word_set = set(wordDict)
        memo = {}

        def backtrack(index):
            if index == len(s):
                return True
            if index in memo:
                return memo[index]

            for word in word_set:
                if s.startswith(word, index):
                    if backtrack(index + len(word)):
                        memo[index] = True
                        return True

            memo[index] = False
            return False
        return backtrack(0)
```

# Integer replacement
