# Topics

1. [Two Pointers Pattern](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#two-pointers-pattern)
2. [Two Sum](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#two-sum)

# Two Pointers Pattern

The two pointers pattern involves using two indices (pointers) to traverse a data structure in order to achieve a certain goal efficiently, often avoiding nested loops.

- One pointer usually starts at the beginning.
- Another pointer may start at the end or sometimes at the same start but moves differently based on the problem.

It’s mainly used for reducing time complexity. Without two pointers, some problems would require nested loops, giving O(n²) complexity. With two pointers, you can often reduce it to O(n).

# Two Sum

You are given:

An array of integers (e.g., [2, 7, 11, 15]) <br/>
A target number (e.g., 9)

You need to find two numbers in the array whose sum equals the target.
Return their indices (not the numbers themselves).

Example:
```javascript
nums = [2, 7, 11, 15], target = 9
Output: [0, 1]   // Because nums[0] + nums[1] = 2 + 7 = 9
```

### Brute-force (simple & correct, but slow)

Idea: try every pair (i, j).
```javascript
/**
 * Brute-force two-sum
 * Returns [i, j] or null if no pair found.
 */
function twoSumBrute(nums, target) {
  const n = nums.length;
  for (let i = 0; i < n - 1; i++) {
    for (let j = i + 1; j < n; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
  return null;
}
```

Time complexity: O(n²) — two nested loops.<br/>
Space complexity: O(1) extra space.<br/>
When to use: tiny arrays, or when simplicity matters. Not good for large n.

### Hash table / Map — optimal average solution (one-pass)

Idea: iterate once, keep a map from number → its index. For each number x, check if target - x (the complement) is already in the map. If yes, we found a pair. If not, store x in the map and continue.

This is the most common and fastest method in practice.
```javascript
/**
 * One-pass hash map (optimal average)
 * Returns [i, j] or null.
 */
function twoSumHash(nums, target) {
  const map = new Map(); // num -> index
  for (let i = 0; i < nums.length; i++) {
    const x = nums[i];
    const need = target - x;
    if (map.has(need)) {
      return [map.get(need), i]; // index of need, current index
    }
    // store current number and index for future complements
    map.set(x, i);
  }
  return null;
}
```

Time complexity: O(n) on average (each element accessed once).<br/>
Space complexity: O(n) additional memory for the map.<br/>
Why one-pass works: When you visit nums[i], you check if you've already seen a number that pairs with it. If not, you save nums[i] for future checks.

📢 NOTES:

> ## Why is Hash Map “better” than Nested Loops?
>
> **⚡ Speed**
> - Nested loops: `O(n²)` → grows *quadratically*  
> - Hash map: `O(n)` → grows *linearly*  
> - For large inputs, hash map can be **thousands or millions of times faster**
>
> **📈 Scalability**
> - Nested loops become unusable for big arrays (too slow)  
> - Hash map handles big arrays easily (as long as memory is available)
>
> **⚖️ Trade-off**
> - Nested loops use less memory  
> - Hash map sacrifices memory for speed

