# Technical Interview Study Guide: Arrays & Hashing

## Contains Duplicate
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Initialize an empty Set to store numbers we have already seen.
2. Iterate through each number in the input array.
3. Check if the current number exists in the Set.
4. If it exists, return `true` (a duplicate is found).
5. If it doesn't exist, add the number to the Set.
6. If the loop finishes, return `false`.

**JavaScript Solution:**
```javascript
const containsDuplicate = (nums) => {
    const seen = new Set();
    for (const num of nums) {
        if (seen.has(num)) return true;
        seen.add(num);
    }
    return false;
};
```

---

## Valid Anagram
**Complexity:** Time: $O(n)$ | Space: $O(1)$ (Space is $O(1)$ because the character set is fixed at 26)

**Logic (Pseudocode):**
1. If lengths of string `s` and `t` differ, return `false`.
2. Create a frequency map (object) to count characters in `s`.
3. Iterate through `s`, incrementing counts in the map.
4. Iterate through `t`, decrementing counts in the map.
5. If a character in `t` is not in the map or its count reaches below zero, return `false`.
6. Return `true` if all counts are zero.

**JavaScript Solution:**
```javascript
const isAnagram = (s, t) => {
    if (s.length !== t.length) return false;
    const count = {};
    for (let char of s) {
        count[char] = (count[char] || 0) + 1;
    }
    for (let char of t) {
        if (!count[char]) return false;
        count[char]--;
    }
    return true;
};
```

---

## Two Sum
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Create a Map to store `value: index`.
2. Iterate through the array.
3. Calculate the `complement` needed to reach the target (`target - currentNum`).
4. If the `complement` exists in the Map, return the index from the Map and the current index.
5. Otherwise, save the current number and its index to the Map.

**JavaScript Solution:**
```javascript
const twoSum = (nums, target) => {
    const map = new Map();
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        map.set(nums[i], i);
    }
};
```

---

## Group Anagrams
**Complexity:** Time: $O(n \cdot k \log k)$ where $k$ is the max length of a string | Space: $O(n \cdot k)$

**Logic (Pseudocode):**
1. Initialize a Map where the key is a sorted string and the value is an array of anagrams.
2. For each string in the input:
    - Sort the characters of the string to create a "key".
    - If the key exists in the Map, push the original string into the associated array.
    - Otherwise, create a new entry in the Map with the sorted key.
3. Return all values from the Map.

**JavaScript Solution:**
```javascript
const groupAnagrams = (strs) => {
    const map = {};
    for (let s of strs) {
        const key = s.split('').sort().join('');
        if (!map[key]) map[key] = [];
        map[key].push(s);
    }
    return Object.values(map);
};
```

---

## Top K Frequent Elements
**Complexity:** Time: $O(n)$ | Space: $O(n)$ (Using Bucket Sort approach)

**Logic (Pseudocode):**
1. Count the frequency of each element using a Map.
2. Create an array of buckets where the index represents the frequency.
3. Iterate through the Map and place the elements into the bucket corresponding to their frequency.
4. Iterate through the buckets from right to left (highest frequency to lowest) and collect elements until we have $k$ items.

**JavaScript Solution:**
```javascript
const topKFrequent = (nums, k) => {
    const count = new Map();
    const buckets = Array.from({ length: nums.length + 1 }, () => []);
    
    for (let n of nums) count.set(n, (count.get(n) || 0) + 1);
    for (let [n, freq] of count) buckets[freq].push(n);

    const res = [];
    for (let i = buckets.length - 1; i >= 0 && res.length < k; i--) {
        if (buckets[i].length > 0) res.push(...buckets[i]);
    }
    return res.slice(0, k);
};
```

---

## Product of Array Except Self
**Complexity:** Time: $O(n)$ | Space: $O(1)$ (Result array does not count toward complexity)

**Logic (Pseudocode):**
1. Create a result array initialized with 1s.
2. Perform a left-to-right pass: each index in the result stores the product of all elements to its left.
3. Perform a right-to-left pass: keep a running `rightProduct` and multiply the current value in the result array by this variable.
4. Return the result array.

**JavaScript Solution:**
```javascript
const productExceptSelf = (nums) => {
    const res = new Array(nums.length).fill(1);
    let leftProduct = 1;
    for (let i = 0; i < nums.length; i++) {
        res[i] = leftProduct;
        leftProduct *= nums[i];
    }
    let rightProduct = 1;
    for (let i = nums.length - 1; i >= 0; i--) {
        res[i] *= rightProduct;
        rightProduct *= nums[i];
    }
    return res;
};
```

---

## Valid Sudoku
**Complexity:** Time: $O(1)$ (Grid is fixed at 9x9) | Space: $O(1)$

**Logic (Pseudocode):**
1. Use Sets to track numbers seen in each row, each column, and each 3x3 sub-box.
2. Iterate through every cell (9x9).
3. If the cell is not empty ('.'):
    - Calculate the box index using `Math.floor(r/3) * 3 + Math.floor(c/3)`.
    - Check if the number already exists in the current row's Set, column's Set, or box's Set.
    - If it does, return `false`.
    - If not, add it to all three relevant Sets.
4. If no conflicts found, return `true`.

**JavaScript Solution:**
```javascript
const isValidSudoku = (board) => {
    const rows = Array.from({ length: 9 }, () => new Set());
    const cols = Array.from({ length: 9 }, () => new Set());
    const boxes = Array.from({ length: 9 }, () => new Set());

    for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
            const val = board[r][c];
            if (val === '.') continue;
            const b = Math.floor(r / 3) * 3 + Math.floor(c / 3);
            if (rows[r].has(val) || cols[c].has(val) || boxes[b].has(val)) return false;
            rows[r].add(val); cols[c].add(val); boxes[b].add(val);
        }
    }
    return true;
};
```

---

## Encode and Decode Strings
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. **Encode:** Join strings using a delimiter that includes the length of the string + a special character (e.g., `"length#word"`).
2. **Decode:** Read the string until the `#`. Parse the length, then slice the following `length` characters as the word. Repeat until the string is exhausted.

**JavaScript Solution:**
```javascript
class Solution {
    encode(strs) {
        return strs.map(s => `${s.length}#${s}`).join('');
    }

    decode(str) {
        const res = [];
        let i = 0;
        while (i < str.length) {
            let j = i;
            while (str[j] !== '#') j++;
            let length = parseInt(str.slice(i, j));
            res.push(str.slice(j + 1, j + 1 + length));
            i = j + 1 + length;
        }
        return res;
    }
}
```

---

## Longest Consecutive Sequence
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Convert the array into a Set for $O(1)$ lookups.
2. Iterate through each number in the Set.
3. Check if it's the start of a sequence (i.e., `num - 1` is not in the Set).
4. If it is the start, count how many consecutive numbers exist (`num + 1, num + 2...`).
5. Update the `maxLength` found so far.

**JavaScript Solution:**
```javascript
const longestConsecutive = (nums) => {
    const set = new Set(nums);
    let maxLen = 0;
    for (let n of set) {
        if (!set.has(n - 1)) {
            let curr = n;
            let count = 1;
            while (set.has(curr + 1)) {
                curr++;
                count++;
            }
            maxLen = Math.max(maxLen, count);
        }
    }
    return maxLen;
};
```

---

## Sort Colors
**Complexity:** Time: $O(n)$ | Space: $O(1)$ (Dutch National Flag Algorithm)

**Logic (Pseudocode):**
1. Use three pointers: `low` (boundary for 0s), `curr` (current element), and `high` (boundary for 2s).
2. While `curr <= high`:
    - If `nums[curr]` is 0, swap with `low`, increment `low` and `curr`.
    - If `nums[curr]` is 2, swap with `high`, decrement `high` (don't increment `curr` yet as the swapped value needs checking).
    - If `nums[curr]` is 1, just increment `curr`.

**JavaScript Solution:**
```javascript
const sortColors = (nums) => {
    let low = 0, curr = 0, high = nums.length - 1;
    while (curr <= high) {
        if (nums[curr] === 0) {
            [nums[low], nums[curr]] = [nums[curr], nums[low]];
            low++; curr++;
        } else if (nums[curr] === 2) {
            [nums[high], nums[curr]] = [nums[curr], nums[high]];
            high--;
        } else {
            curr++;
        }
    }
};
```
