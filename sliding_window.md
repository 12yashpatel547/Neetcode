# Sliding Window Patterns (2026 Edition)
---

## 1. Best Time to Buy & Sell Stock
[LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Complexity
* **Time Complexity:** $O(n)$ — Single pass through the pricing array.
* **Space Complexity:** $O(1)$ — Only tracking two scalar variables.

### Key Intuition
While often categorized as "Greedy," this is a **Two-Pointer Sliding Window** problem where the "window" represents the time between buying and selling. We expand the right pointer to find a higher price, but if we find a new minimum (price at right < price at left), we "slide" the left pointer immediately to that new low to reset our potential profit base.

### Pseudocode
1. Initialize `leftPointer` at day 0 (buy day) and `rightPointer` at day 1 (sell day).
2. Maintain a `maxProfit` variable at 0.
3. While `rightPointer` is within the array bounds:
   - If the price at `rightPointer` is greater than `leftPointer`, calculate current profit and update `maxProfit`.
   - If the price at `rightPointer` is lower than `leftPointer`, move `leftPointer` to the `rightPointer` position (found a better buy day).
   - Increment `rightPointer`.

### Edge Cases
* **Monotonically Decreasing Prices:** The window slides constantly, and `maxProfit` remains 0.
* **Single Day/Empty Array:** Window cannot form; should return 0.

### JavaScript Solution
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
const maxProfit = (prices) => {
    let leftPointer = 0; // Buy day
    let rightPointer = 1; // Sell day
    let maxProfitValue = 0;

    while (rightPointer < prices.length) {
        if (prices[leftPointer] < prices[rightPointer]) {
            const currentProfit = prices[rightPointer] - prices[leftPointer];
            maxProfitValue = Math.max(maxProfitValue, currentProfit);
        } else {
            // Found a lower buying point, shift the window start
            leftPointer = rightPointer;
        }
        rightPointer++;
    }

    return maxProfitValue;
};
```

---

## 2. Longest Substring Without Repeating Characters
[LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### Complexity
* **Time Complexity:** $O(n)$ — Each character is visited at most twice (once by each pointer).
* **Space Complexity:** $O(min(m, n))$ — Where $m$ is the character set size (e.g., 26 for English, or total Unicode).

### Key Intuition
We use a **Dynamic Sliding Window**. The `rightPointer` expands the window by adding characters to a `Set`. If we encounter a duplicate, the window is "invalid." We must shrink the window from the `leftPointer` until the duplicate is removed, restoring the "no-repeat" invariant.

### Pseudocode
1. Initialize `leftPointer` at 0 and a `seenCharacters` Set.
2. Iterate through the string with `rightPointer`.
3. If `string[rightPointer]` is already in the set, delete `string[leftPointer]` from the set and increment `leftPointer` until the duplicate is gone.
4. Add `string[rightPointer]` to the set.
5. Update `maxSubstringLength` with the current window size (`rightPointer - leftPointer + 1`).

### Edge Cases
* **String with all identical characters:** Window size stays 1 throughout.
* **Empty String:** Return 0 immediately.

### JavaScript Solution
```javascript
const lengthOfLongestSubstring = (inputString) => {
    const seenCharacters = new Set();
    let leftPointer = 0;
    let maxSubstringLength = 0;

    for (let rightPointer = 0; rightPointer < inputString.length; rightPointer++) {
        const currentCharacter = inputString[rightPointer];

        // Shrink window from the left until duplicate is removed
        while (seenCharacters.has(currentCharacter)) {
            seenCharacters.delete(inputString[leftPointer]);
            leftPointer++;
        }

        seenCharacters.add(currentCharacter);
        const currentWindowSize = rightPointer - leftPointer + 1;
        maxSubstringLength = Math.max(maxSubstringLength, currentWindowSize);
    }

    return maxSubstringLength;
};
```

---

## 3. Longest Repeating Character Replacement
[LeetCode 424](https://leetcode.com/problems/longest-repeating-character-replacement/)

### Complexity
* **Time Complexity:** $O(n)$ — Linear scan with a constant-time frequency map update.
* **Space Complexity:** $O(1)$ — The frequency map stores at most 26 uppercase English letters.

### Key Intuition
The window is valid if: `(Window Length - Frequency of Most Frequent Character) <= k`. This formula represents the number of characters we *must* replace to make the entire window uniform. If this exceeds $k$, we slide the `leftPointer` to shrink the window.

### Pseudocode
1. Create a `frequencyMap` to track character counts in the current window.
2. Maintain `maxCharacterFrequency` (the count of the most frequent char ever seen in *any* window).
3. Expand `rightPointer`. Update `frequencyMap` and `maxCharacterFrequency`.
4. While `(windowLength - maxCharacterFrequency) > k`:
   - Decrement `frequencyMap[string[leftPointer]]`.
   - Increment `leftPointer`.
5. Update the result with the max window size found.

### Edge Cases
* **k is larger than string length:** Entire string can be replaced; return string length.
* **k is 0:** Problem becomes "Find longest substring of identical characters."

### JavaScript Solution
```javascript
const characterReplacement = (inputString, k) => {
    const characterFrequency = {};
    let leftPointer = 0;
    let maxCharacterFrequency = 0;
    let maxSubstringLength = 0;

    for (let rightPointer = 0; rightPointer < inputString.length; rightPointer++) {
        const rightChar = inputString[rightPointer];
        characterFrequency[rightChar] = (characterFrequency[rightChar] || 0) + 1;
        
        // Track the most frequent character in the current/past windows
        maxCharacterFrequency = Math.max(maxCharacterFrequency, characterFrequency[rightChar]);

        // If window is invalid (too many replacements needed), shrink it
        const currentWindowLength = rightPointer - leftPointer + 1;
        if (currentWindowLength - maxCharacterFrequency > k) {
            const leftChar = inputString[leftPointer];
            characterFrequency[leftChar]--;
            leftPointer++;
        }

        maxSubstringLength = Math.max(maxSubstringLength, rightPointer - leftPointer + 1);
    }

    return maxSubstringLength;
};
```

---

## 4. Permutation in String
[LeetCode 567](https://leetcode.com/problems/permutation-in-string/)

### Complexity
* **Time Complexity:** $O(n)$ — Specifically $O(length(s1) + length(s2))$.
* **Space Complexity:** $O(1)$ — Two frequency arrays of size 26.

### Key Intuition
This is a **Fixed-Size Sliding Window**. The window size is always `s1.length`. We compare the character frequency of the current window in `s2` against the frequency of `s1`. In 2026 interviews, using an array of size 26 is preferred over a Hash Map for $O(1)$ space optimization.

### Pseudocode
1. If `s1` is longer than `s2`, return false.
2. Initialize two frequency arrays (`s1Counts`, `s2Counts`) for 'a'-'z'.
3. Populate `s1Counts` and the first window of `s2Counts`.
4. Iterate from `s1.length` to `s2.length`:
   - If frequencies match, return true.
   - Slide window: Add `s2[rightPointer]` and remove `s2[leftPointer]`.
5. Return final frequency comparison.

### Edge Cases
* **s1 is length 1:** Checks if char exists in s2.
* **s1 and s2 are identical:** Returns true.

### JavaScript Solution
```javascript
const checkInclusion = (searchString, targetString) => {
    if (searchString.length > targetString.length) return false;

    const searchFrequencies = new Array(26).fill(0);
    const windowFrequencies = new Array(26).fill(0);
    const charCodeOffset = 'a'.charCodeAt(0);

    // Initial window population
    for (let index = 0; index < searchString.length; index++) {
        searchFrequencies[searchString.charCodeAt(index) - charCodeOffset]++;
        windowFrequencies[targetString.charCodeAt(index) - charCodeOffset]++;
    }

    let matchesCount = 0;
    for (let i = 0; i < 26; i++) {
        if (searchFrequencies[i] === windowFrequencies[i]) matchesCount++;
    }

    for (let rightPointer = searchString.length; rightPointer < targetString.length; rightPointer++) {
        if (matchesCount === 26) return true;

        const leftPointer = rightPointer - searchString.length;
        
        // Add new character to window
        updateFrequencies(targetString[rightPointer], 1);
        // Remove old character from window
        updateFrequencies(targetString[leftPointer], -1);
    }

    function updateFrequencies(char, delta) {
        const index = char.charCodeAt(0) - charCodeOffset;
        const prevMatch = windowFrequencies[index] === searchFrequencies[index];
        windowFrequencies[index] += delta;
        const nowMatch = windowFrequencies[index] === searchFrequencies[index];
        
        if (!prevMatch && nowMatch) matchesCount++;
        else if (prevMatch && !nowMatch) matchesCount--;
    }

    return matchesCount === 26;
};
```

---

## 5. [NEW 2026 TREND] Maximum Sum of Distinct Subarrays With Length K
[LeetCode 2461](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

### Complexity
* **Time Complexity:** $O(n)$ — Single pass using a fixed-size window.
* **Space Complexity:** $O(k)$ — Set/Map to store up to $k$ distinct elements.

### Key Intuition
This combines **Fixed-Size Sliding Window** with **Distinctness Constraints**. Modern interviews use this to test if you can manage multiple state variables (sum + count + set) simultaneously without $O(n^2)$ overhead.

### Pseudocode
1. Use a `Set` to track uniqueness and a `currentSum` variable.
2. Slide a window of size `k`.
3. If the window has exactly `k` elements AND the `Set.size` is `k`, update `maxSum`.
4. When moving the window: subtract the element exiting the left and add the element entering the right.

### JavaScript Solution
```javascript
const maximumSubarraySum = (nums, k) => {
    let maxTotalSum = 0;
    let currentWindowSum = 0;
    let leftPointer = 0;
    const distinctElements = new Map();

    for (let rightPointer = 0; rightPointer < nums.length; rightPointer++) {
        const incomingValue = nums[rightPointer];
        currentWindowSum += incomingValue;
        distinctElements.set(incomingValue, (distinctElements.get(incomingValue) || 0) + 1);

        // Once window size reaches K
        if (rightPointer - leftPointer + 1 === k) {
            // Check if all elements in the map are distinct
            if (distinctElements.size === k) {
                maxTotalSum = Math.max(maxTotalSum, currentWindowSum);
            }

            // Shrink window from left to prepare for next slide
            const outgoingValue = nums[leftPointer];
            currentWindowSum -= outgoingValue;
            
            const currentFreq = distinctElements.get(outgoingValue);
            if (currentFreq === 1) {
                distinctElements.delete(outgoingValue);
            } else {
                distinctElements.set(outgoingValue, currentFreq - 1);
            }
            leftPointer++;
        }
    }

    return maxTotalSum;
};
```

---

## 6. [NEW 2026 TREND] Minimum Window Substring
[LeetCode 76](https://leetcode.com/problems/minimum-window-substring/)

### Complexity
* **Time Complexity:** $O(n + m)$ — Where $n$ is length of string $s$ and $m$ is length of $t$.
* **Space Complexity:** $O(m)$ — To store frequency of characters in $t$.

### Key Intuition
The "Hard" evolution of the pattern. You expand until the window is "saturated" (contains all required characters), then shrink from the left as much as possible while maintaining saturation to find the *minimum* length.

### JavaScript Solution
```javascript
const minWindow = (sourceString, targetString) => {
    if (!targetString.length) return "";

    const targetFrequency = {};
    for (const char of targetString) {
        targetFrequency[char] = (targetFrequency[char] || 0) + 1;
    }

    const windowFrequency = {};
    let requiredMatches = Object.keys(targetFrequency).length;
    let currentMatches = 0;
    
    let resultRange = [-1, -1];
    let minWindowLength = Infinity;
    let leftPointer = 0;

    for (let rightPointer = 0; rightPointer < sourceString.length; rightPointer++) {
        const char = sourceString[rightPointer];
        windowFrequency[char] = (windowFrequency[char] || 0) + 1;

        if (targetFrequency[char] && windowFrequency[char] === targetFrequency[char]) {
            currentMatches++;
        }

        // Try to shrink the window while it's still valid
        while (currentMatches === requiredMatches) {
            const currentWindowSize = rightPointer - leftPointer + 1;
            if (currentWindowSize < minWindowLength) {
                minWindowLength = currentWindowSize;
                resultRange = [leftPointer, rightPointer];
            }

            const leftChar = sourceString[leftPointer];
            windowFrequency[leftChar]--;
            if (targetFrequency[leftChar] && windowFrequency[leftChar] < targetFrequency[leftChar]) {
                currentMatches--;
            }
            leftPointer++;
        }
    }

    const [start, end] = resultRange;
    return minWindowLength === Infinity ? "" : sourceString.substring(start, end + 1);
};
```
