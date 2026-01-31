# Technical Interview Guide: The Two-Pointer Pattern
**Engineer's Note:** This file covers the "Squeeze" or "Converging Pointers" technique. This is the gold standard for optimizing space complexity from $O(n)$ down to $O(1)$ when dealing with sorted data or boundary-based problems.

---

## Valid Palindrome
**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Logic (Pseudocode):**
1. Normalize the input string by removing non-alphanumeric characters and converting to lowercase.
2. Initialize `left` at the start and `right` at the end.
3. Compare characters at both pointers; if they don't match, return `false`.
4. Move pointers toward each other until they meet.

**JavaScript Solution:**
```javascript
const isPalindrome = (s) => {
    const cleanS = s.replace(/[^a-z0-9]/gi, '').toLowerCase();
    let left = 0, right = cleanS.length - 1;
    while (left < right) {
        if (cleanS[left] !== cleanS[right]) return false;
        left++;
        right--;
    }
    return true;
};
```

---

## Two Sum II
**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Logic (Pseudocode):**
1. Set `left` to the first index and `right` to the last.
2. If `sum` of values is greater than target, move `right` left.
3. If `sum` is less than target, move `left` right.
4. Return indices once `sum === target`.

**JavaScript Solution:**
```javascript
const twoSum = (numbers, target) => {
    let left = 0, right = numbers.length - 1;
    while (left < right) {
        const sum = numbers[left] + numbers[right];
        if (sum === target) return [left + 1, right + 1];
        sum < target ? left++ : right--;
    }
    return [];
};
```

---

## 3Sum
**Complexity:** Time: $O(n^2)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Sort the array.
2. Loop through with index `i`. Skip duplicates.
3. For each `i`, use Two Pointers on the remaining sub-array to find a sum of `-nums[i]`.
4. Skip duplicates for `left` and `right` pointers after finding a valid triplet.

**JavaScript Solution:**
```javascript
const threeSum = (nums) => {
    const res = [];
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        let left = i + 1, right = nums.length - 1;
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                res.push([nums[i], nums[left], nums[right]]);
                while (nums[left] === nums[left + 1]) left++;
                while (nums[right] === nums[right - 1]) right--;
                left++; right--;
            } else if (sum < 0) left++;
            else right--;
        }
    }
    return res;
};
```

---

## Container with Most Water
**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Logic (Pseudocode):**
1. Use pointers at both ends.
2. Area is `width * min(height[left], height[right])`.
3. Keep track of `maxArea`.
4. Move the pointer pointing to the shorter line to potentially find a taller one.

**JavaScript Solution:**
```javascript
const maxArea = (height) => {
    let maxA = 0, left = 0, right = height.length - 1;
    while (left < right) {
        const h = Math.min(height[left], height[right]);
        maxA = Math.max(maxA, h * (right - left));
        height[left] < height[right] ? left++ : right--;
    }
    return maxA;
};
```

---

## 4Sum
**Complexity:** Time: $O(n^3)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Sort the array.
2. Nest two loops (`i` and `j`) to fix the first two numbers. Skip duplicates for both.
3. Use two pointers for the remaining two numbers to reach the `target`.

**JavaScript Solution:**
```javascript
const fourSum = (nums, target) => {
    nums.sort((a, b) => a - b);
    const res = [];
    for (let i = 0; i < nums.length - 3; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        for (let j = i + 1; j < nums.length - 2; j++) {
            if (j > i + 1 && nums[j] === nums[j - 1]) continue;
            let left = j + 1, right = nums.length - 1;
            while (left < right) {
                const sum = nums[i] + nums[j] + nums[left] + nums[right];
                if (sum === target) {
                    res.push([nums[i], nums[j], nums[left], nums[right]]);
                    while (nums[left] === nums[left + 1]) left++;
                    while (nums[right] === nums[right - 1]) right--;
                    left++; right--;
                } else if (sum < target) left++;
                else right--;
            }
        }
    }
    return res;
};
```
};

