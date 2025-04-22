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
