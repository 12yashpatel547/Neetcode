# Technical Interview Study Guide: Two Pointers Pattern

## Valid Palindrome
**LeetCode Link:** [https://leetcode.com/problems/valid-palindrome/](https://leetcode.com/problems/valid-palindrome/)

**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Key Intuition:** Use two pointers starting at opposite ends to verify symmetry while skipping non-alphanumeric characters.

**Pseudocode:**
1. Initialize `left` at 0 and `right` at the last index.
2. While `left < right`:
    - If `left` character is not alphanumeric, increment `left`.
    - Else if `right` character is not alphanumeric, decrement `right`.
    - Else if characters at `left` and `right` (lowercase) don't match, return `false`.
    - Otherwise, move both pointers inward.
3. Return `true` if loop completes.

**Edge Cases:**
- String with only special characters (e.g., "###") should be `true`.
- Empty string is considered a valid palindrome.
- Strings with mixed casing.

**JavaScript Solution:**
```javascript
const isPalindrome = (s) => {
    let l = 0, r = s.length - 1;
    
    while (l < r) {
        // Skip non-alphanumeric characters using a regex check
        if (!/[a-zA-Z0-9]/.test(s[l])) {
            l++;
        } else if (!/[a-zA-Z0-9]/.test(s[r])) {
            r--;
        } else {
            // Compare characters case-insensitively
            if (s[l].toLowerCase() !== s[r].toLowerCase()) return false;
            l++;
            r--;
        }
    }
    return true;
};
```

---

## Two Sum II - Input Array Is Sorted
**LeetCode Link:** [https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Key Intuition:** Leverage the sorted property by moving pointers inward based on whether the current sum is too high or too low.

**Pseudocode:**
1. Place `left` at start, `right` at end.
2. Calculate `sum = nums[left] + nums[right]`.
3. If `sum === target`, return `[left + 1, right + 1]`.
4. If `sum < target`, we need a larger value, so `left++`.
5. If `sum > target`, we need a smaller value, so `right--`.

**Edge Cases:**
- Array with exactly two elements.
- Target sum involving negative numbers.
- Sums that exceed integer limits (though not an issue in JS).

**JavaScript Solution:**
```javascript
const twoSum = (numbers, target) => {
    let l = 0, r = numbers.length - 1;
    
    while (l < r) {
        const sum = numbers[l] + numbers[r];
        if (sum === target) return [l + 1, r + 1];
        
        // Move pointers based on target comparison
        sum < target ? l++ : r--;
    }
};
```

---

## 3Sum
**LeetCode Link:** [https://leetcode.com/problems/3sum/](https://leetcode.com/problems/3sum/)

**Complexity:** Time: $O(n^2)$ | Space: $O(1)$ or $O(n)$ depending on sorting implementation.

**Key Intuition:** Fix one number and solve the "Two Sum II" problem for the rest, ensuring you skip duplicates at every step.

**Pseudocode:**
1. Sort the array.
2. Iterate through `nums` with index `i`. If `nums[i] === nums[i-1]`, skip to avoid duplicates.
3. Set `target = -nums[i]`.
4. Use two pointers (`left = i + 1`, `right = length - 1`) to find pairs that sum to `target`.
5. When a triplet is found, push to results and move pointers past duplicates.

**Edge Cases:**
- Array with fewer than 3 elements.
- Array with all zeros (e.g., `[0, 0, 0, 0]`).
- No triplets exist that sum to zero.

**JavaScript Solution:**
```javascript
const threeSum = (nums) => {
    const res = [];
    nums.sort((a, b) => a - b);
    
    for (let i = 0; i < nums.length - 2; i++) {
        // Never reuse the same 'first' number to avoid duplicate triplets
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        
        let l = i + 1, r = nums.length - 1;
        while (l < r) {
            const sum = nums[i] + nums[l] + nums[r];
            if (sum === 0) {
                res.push([nums[i], nums[l], nums[r]]);
                // Skip duplicates for the left pointer
                while (l < r && nums[l] === nums[l + 1]) l++;
                // Skip duplicates for the right pointer
                while (l < r && nums[r] === nums[r - 1]) r--;
                l++; r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return res;
};
```

---

## Container with Most Water
**LeetCode Link:** [https://leetcode.com/problems/container-with-most-water/](https://leetcode.com/problems/container-with-most-water/)

**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Key Intuition:** The area is limited by the shorter bar; moving the taller bar can only decrease area, so always move the shorter one.

**Pseudocode:**
1. Initialize `maxArea = 0`, `left = 0`, `right = length - 1`.
2. While `left < right`:
    - `currentArea = min(height[left], height[right]) * (right - left)`.
    - Update `maxArea`.
    - Move the pointer pointing to the smaller height inward.

**Edge Cases:**
- Array with only two bars.
- Bars with zero height.
- Array with bars of descending or ascending heights.

**JavaScript Solution:**
```javascript
const maxArea = (height) => {
    let max = 0, l = 0, r = height.length - 1;
    
    while (l < r) {
        // Calculate area based on the shorter wall
        const currentArea = Math.min(height[l], height[r]) * (r - l);
        max = Math.max(max, currentArea);
        
        // Greedily move the shorter pointer
        height[l] < height[r] ? l++ : r--;
    }
    return max;
};
```

---

## 4Sum
**LeetCode Link:** [https://leetcode.com/problems/4sum/](https://leetcode.com/problems/4sum/)

**Complexity:** Time: $O(n^3)$ | Space: $O(n)$

**Key Intuition:** Reduce the problem to 3Sum (and then to 2Sum) by nesting loops, while maintaining strict duplicate checks.

**Pseudocode:**
1. Sort the array.
2. Use nested loops `i` and `j` to fix the first two numbers.
3. For each pair, use Two Pointers (`left` and `right`) to find the remaining two.
4. Calculate `sum = nums[i] + nums[j] + nums[left] + nums[right]`.
5. If `sum === target`, add to results and skip duplicates for `left` and `right`.
6. Else adjust `left` or `right` based on the sum comparison.

**Edge Cases:**
- Target is a very large positive or negative number.
- Array contains many duplicate values.
- Empty array or array length < 4.

**JavaScript Solution:**
```javascript
const fourSum = (nums, target) => {
    nums.sort((a, b) => a - b);
    const res = [];
    
    for (let i = 0; i < nums.length - 3; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        for (let j = i + 1; j < nums.length - 2; j++) {
            if (j > i + 1 && nums[j] === nums[j - 1]) continue;
            
            let l = j + 1, r = nums.length - 1;
            while (l < r) {
                const sum = nums[i] + nums[j] + nums[l] + nums[r];
                if (sum === target) {
                    res.push([nums[i], nums[j], nums[l], nums[r]]);
                    while (l < r && nums[l] === nums[l + 1]) l++;
                    while (l < r && nums[r] === nums[r - 1]) r--;
                    l++; r--;
                } else if (sum < target) l++;
                else r--;
            }
        }
    }
    return res;
};
```
