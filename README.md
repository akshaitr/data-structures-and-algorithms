# Topics

1. [Two Pointers Pattern](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#two-pointers-pattern)
2. [Two Sum](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#two-sum)
3. [Kadane's Algorithm](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#kadanes-algorithm)
4. [Best Time to Buy and Sell Stock](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#best-time-to-buy-and-sell-stock)
5. [Hash Map](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#hash-map)
6. [Valid Anagram](https://github.com/akshaitr/data-structures-and-algorithms?tab=readme-ov-file#valid-anagram)

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

# Kadane's Algorithm

Kadane’s Algorithm finds the largest sum of a contiguous part of a list by keeping a running total as you scan the list. Whenever this running sum becomes negative, it is reset to zero, and at each step, the algorithm tracks the maximum sum seen so far.

📢 NOTES:

> It’s basically: "accumulate as long as it helps, reset if it hurts, remember the best."

We are given a list of numbers (some may be negative, some positive).<br/>
We need to find the biggest sum we can get from a contiguous (side-by-side) part of the list.

```javascript
function maxSubarraySum(nums) {
  let currentSum = 0;
  let maxSum = nums[0];
  for (let num of nums) {
    currentSum += num;
    if (currentSum > maxSum) {
      maxSum = currentSum;
    }
    if (currentSum < 0) {
      currentSum = 0;
    }
  }
  return maxSum;
}
```

Example:
```javascript
const nums = [4, -2, 3, -1];
console.log(maxSubarraySum(nums)); // Output: 5
```

# Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a stock on day `i`.<br/>
You want to maximize your profit by choosing one day to buy and one day in the future to sell. You can only make one transaction.<br/>
Return the maximum profit you can achieve. If no profit is possible, return `0`.

Example:
```javascript
Input: prices = [7, 1, 5, 3, 6, 4]
Output: 5
Explanation: Buy on day 2 (price=1) and sell on day 5 (price=6), profit = 6-1 = 5
```

```javascript
function maxProfit(prices) {
  let minPrice = prices[0];
  let maxProfit = 0;
  for (let i = 1; i < prices.length; i++) {
    if (prices[i] < minPrice) {
      minPrice = prices[i];
    }
    let profit = prices[i] - minPrice; // profit if sold today
    if (profit > maxProfit) {
      maxProfit = profit;
    }
  }
  return maxProfit;
}
```

# Hash Map

```javascript
// Create a new Map
const map = new Map();

// Add key-value pairs
map.set('name', 'Akshai');
map.set('age', 29);
map.set(1, 'one'); // keys can be numbers too

// Access a value
console.log(map.get('name')); // Akshai

// Check if a key exists
console.log(map.has('age')); // true

// Remove a key
map.delete('age');

// Get the size
console.log(map.size); // 2

// Iterate over map
for (let [key, value] of map) {
  console.log(key, value);
}

map.keys(); // Returns a new Iterator object that contains the keys for each element in the Map object in insertion order.

map.values(); // Returns a new Iterator object that contains the values for each element in the Map object in insertion order.

// Clear all entries
map.clear();
```

- Keys can be any type (objects, functions, numbers, etc.).
- Maintains insertion order.
- Has built-in size property.

# Valid Anagram

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

Example 1:
```javascript
Input: s = "anagram", t = "nagaram"
Output: true
```

Example 2:
```javascript
Input: s = "rat", t = "car"
Output: false
```

```javascript
function isAnagram(s, t) {
  if (s.length !== t.length) {
    return false;
  }
  const freq = new Map();
  for (let i = 0; i < s.length; i++) {
    freq.set(s[i], (freq.get(s[i]) || 0) + 1);
    freq.set(t[i], (freq.get(t[i]) || 0) - 1);
  }
  for (const count of freq.values()) {
    if (count !== 0) {
      return false;
    }
  }
  return true;
}
```

📢 NOTES:

> What if the inputs contain Unicode characters? How would you adapt your solution to such a case?
>
> The above Map solution works for Unicode too because Map keys can be any string, not just ASCII.
> - Works for any Unicode characters.
> - Still O(n) time, O(n) space.
> - No assumptions about character range.

### Even faster for lowercase a–z

If input is guaranteed to be lowercase English letters, an integer array of length 26 is fastest (less overhead than Map):

```javascript
function isAnagram(s, t) {
  if (s.length !== t.length) {
    return false;
  }
  const counts = new Array(26).fill(0);
  const base = "a".charCodeAt(0);
  for (let i = 0; i < s.length; i++) {
    counts[s.charCodeAt(i) - base]++;
    counts[t.charCodeAt(i) - base]--;
  }
  return counts.every((c) => c === 0);
}
```

