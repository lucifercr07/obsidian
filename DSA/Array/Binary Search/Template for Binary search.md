- Link: https://leetcode.com/problems/koko-eating-bananas/solutions/769702/python-clear-explanation-powerful-ultimate-binary-search-template-solved-many-problems/
```java
def binary_search(array) -> int:
    def condition(value) -> bool:
        pass

    left, right = 0, len(array)
    while left < right:
        mid = left + (right - left) // 2
        if condition(mid):
            right = mid
        else:
            left = mid + 1
    return left
```
-   Correctly initialize the boundary variables `left` and `right`. Only one rule: set up the boundary to **include all possible elements**;
-   Decide return value. Is it `return left` or `return left - 1`? Remember this: **after exiting the while loop, `left` is the minimal k​ satisfying the `condition` function**;
-   Design the `condition` function. This is the most difficult and most beautiful part. Needs lots of practice.