# Technical Interview Study Guide: Sliding Window & Fast/Slow Pointers

## Best Time to Buy and Sell Stock
**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Logic (Pseudocode):**
1. Initialize `left` (buy day) at 0 and `right` (sell day) at 1.
2. Initialize `max_profit` at 0.
3. While `right` is within the array bounds:
    - If the price at `left` is lower than at `right`, calculate profit.
    - Update `max_profit` if the current profit is higher.
    - If the price at `right` is lower than at `left`, move the `left` pointer to `right` (we found a cheaper buying point).
    - Increment `right` to check the next day.

**JavaScript Solution:**
```javascript
const maxProfit = (prices) => {
    let left = 0; // Buy
    let right = 1; // Sell
    let maxP = 0;

    while (right < prices.length) {
        if (prices[left] < prices[right]) {
            let profit = prices[right] - prices[left];
            maxP = Math.max(maxP, profit);
        } else {
            left = right;
        }
        right++;
    }
    return maxP;
};
```

---

## Linked List Cycle
**Complexity:** Time: $O(n)$ | Space: $O(1)$

**Logic (Pseudocode):**
1. Initialize two pointers, `slow` and `fast`, both at the head of the list.
2. Move `slow` forward by 1 step and `fast` forward by 2 steps.
3. If there is a cycle, the `fast` pointer will eventually "lap" the `slow` pointer and they will meet (`slow === fast`).
4. If `fast` reaches `null` or `fast.next` is `null`, there is no cycle.



**JavaScript Solution:**
```javascript
const hasCycle = (head) => {
    let slow = head;
    let fast = head;

    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow === fast) return true;
    }
    return false;
};
```

---

## Longest Substring Without Repeating Characters
**Complexity:** Time: $O(n)$ | Space: $O(min(m, n))$ where $m$ is the character set size.

**Logic (Pseudocode):**
1. Use a `Set` to track characters in the current window.
2. Initialize `left` pointer at 0 and `maxLen` at 0.
3. Iterate through the string with a `right` pointer.
4. If `s[right]` is already in the `Set`, remove `s[left]` from the set and increment `left` until the duplicate is gone.
5. Add `s[right]` to the set and update `maxLen` with the current window size (`right - left + 1`).



**JavaScript Solution:**
```javascript
const lengthOfLongestSubstring = (s) => {
    let charSet = new Set();
    let left = 0;
    let maxLen = 0;

    for (let right = 0; right < s.length; right++) {
        while (charSet.has(s[right])) {
            charSet.delete(s[left]);
            left++;
        }
        charSet.add(s[right]);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
};
```
# Technical Interview Study Guide: Advanced Sliding Window

## Longest Repeating Character Replacement
**Complexity:** Time: $O(n)$ | Space: $O(26) \approx O(1)$ since we only store counts for English uppercase letters.

**Logic (Pseudocode):**
1. Use a frequency Map (or object) to track the count of characters in the current window.
2. Maintain a variable `maxFreq` to track the frequency of the most common character currently in the window.
3. Iterate with a `right` pointer:
    - Update the frequency of `s[right]` and update `maxFreq`.
    - Check if the number of characters to replace (`windowLength - maxFreq`) exceeds `k`.
    - If it does, the window is invalid: decrement the frequency of `s[left]` and move the `left` pointer forward.
4. The maximum window size found during the iteration is the result.

**JavaScript Solution:**
```javascript
/**
 * @param {string} s
 * @param {number} k
 * @return {number}
 */
const characterReplacement = (s, k) => {
    let count = {};
    let left = 0;
    let maxFreq = 0;
    let maxLength = 0;

    for (let right = 0; right < s.length; right++) {
        // Update frequency for the new character
        count[s[right]] = (count[s[right]] || 0) + 1;
        
        // Track the most frequent character in the current window
        maxFreq = Math.max(maxFreq, count[s[right]]);

        // If (total characters in window - most frequent char count) > k, 
        // it means we need more than k replacements to make the window uniform.
        while ((right - left + 1) - maxFreq > k) {
            count[s[left]] -= 1;
            left++;
        }

        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
};
```

---

## Permutation in String
**Complexity:** Time: $O(n)$ where $n$ is the length of `s2` | Space: $O(26) \approx O(1)$ for the frequency arrays.

**Logic (Pseudocode):**
1. A permutation means the window must have the exact same character counts as `s1`.
2. If `s1` is longer than `s2`, return `false`.
3. Create two frequency arrays (size 26) to store character counts for `s1` and the current window in `s2`.
4. Initialize the first window (length of `s1`) and count "matches" (how many characters have identical frequencies in both arrays).
5. Slide the window across `s2` one character at a time:
    - Add a new character on the right, update its count, and update the "matches" tally.
    - Remove the character on the left, update its count, and update the "matches" tally.
    - If matches ever reach 26, a permutation exists.
6. Return `false` if the end of `s2` is reached without 26 matches.

**JavaScript Solution:**
```javascript
/**
 * @param {string} s1
 * @param {string} s2
 * @return {boolean}
 */
const checkInclusion = (s1, s2) => {
    if (s1.length > s2.length) return false;

    const s1Count = new Array(26).fill(0);
    const s2Count = new Array(26).fill(0);
    const getIdx = (char) => char.charCodeAt(0) - 'a'.charCodeAt(0);

    // Initialize counts for s1 and the first window of s2
    for (let i = 0; i < s1.length; i++) {
        s1Count[getIdx(s1[i])]++;
        s2Count[getIdx(s2[i])]++;
    }

    let matches = 0;
    for (let i = 0; i < 26; i++) {
        if (s1Count[i] === s2Count[i]) matches++;
    }

    let left = 0;
    for (let right = s1.length; right < s2.length; right++) {
        if (matches === 26) return true;

        // Slide Right: Add new character
        let rIdx = getIdx(s2[right]);
        s2Count[rIdx]++;
        if (s1Count[rIdx] === s2Count[rIdx]) {
            matches++;
        } else if (s1Count[rIdx] + 1 === s2Count[rIdx]) {
            matches--;
        }

        // Slide Left: Remove old character
        let lIdx = getIdx(s2[left]);
        s2Count[lIdx]--;
        if (s1Count[lIdx] === s2Count[lIdx]) {
            matches++;
        } else if (s1Count[lIdx] - 1 === s2Count[lIdx]) {
            matches--;
        }
        left++;
    }

    return matches === 26;
};
```
