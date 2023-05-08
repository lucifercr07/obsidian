- Ref: https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c
- 
```python
class Solution:
    def minFlips(self, a: int, b: int, c: int) -> int:
        result = 0

        # 4 -> 0100
        # 2 -> 0010
        # 7 -> 0111
        for i in range(0, 32):
            x = (a >> i) & 1
            y = (b >> i) & 1
            z = (c >> i) & 1

            # The logic here's that when z = 0 to achieve that we need to 
            # flip which all bits so that it's | operation will get us a 0.
            if z == 0:
                if x == 0 and y == 0:
                    continue
                else:
                    if x == 1 and y == 1:
                        result += 2
                    else:
                        result += 1
           # Similarly when z = 1, we need to switch which all bits to get us to 1
            else:
                if x == 1 or y == 1:
                    continue
                else:
                    result += 1

        return result

```