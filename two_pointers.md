# Technical Interview Study Guide: Two Pointers Pattern

## Valid Palindrome
**LeetCode Link:** [https://leetcode.com/problems/valid-palindrome/](https://leetcode.com/problems/valid-palindrome/)

**Complexity:** Time: $O(totalLength)$ | Space: $O(1)$

**Key Intuition:** Use two pointers starting at opposite ends to compare characters while ignoring non-alphanumeric noise.

**Pseudocode:**
1. Initialize `startIndex` at the beginning and `endIndex` at the end of the string.
2. Iterate until pointers meet:
    - Skip characters that are not alphanumeric by moving the respective pointer.
    - Convert both characters to lowercase for comparison.
    - If characters differ, the string is not a palindrome.
3. If all valid comparisons match, return true.

**Edge Cases:**
- Empty strings or strings with only whitespace/punctuation (should return true).
- Single character strings.
- Mixed case alphanumeric characters.

**JavaScript Solution:**
```javascript
/**
 * @param {string} inputString
 * @return {boolean}
 */
const isPalindrome = (inputString) => {
    let startIndex = 0;
    let endIndex = inputString.length - 1;

    while (startIndex < endIndex) {
        const startChar = inputString[startIndex].toLowerCase();
        const endChar = inputString[endIndex].toLowerCase();

        // Check if characters are alphanumeric using a helper or regex
        const isStartAlphanumeric = /[a-z0-9]/.test(startChar);
        const isEndAlphanumeric = /[a-z0-9]/.test(endChar);

        if (!isStartAlphanumeric) {
            startIndex++;
        } else if (!isEndAlphanumeric) {
            endIndex--;
        } else {
            if (startChar !== endChar) {
                return false;
            }
            startIndex++;
            endIndex--;
        }
    }

    return true;
};
```

---

## Two Sum II - Input Array Is Sorted
**LeetCode Link:** [https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

**Complexity:** Time: $O(totalCount)$ | Space: $O(1)$

**Key Intuition:** Since the array is sorted, we can control the sum by moving the left pointer to increase it or the right pointer to decrease it.

**Pseudocode:**
1. Initialize `leftPointer` at 0 and `rightPointer` at the last index.
2. Calculate the sum of elements at both pointers.
3. If `currentSum` equals `targetValue`, return the 1-based indices.
4. If `currentSum` is less than `targetValue`, increment `leftPointer`.
5. If `currentSum` is greater than `targetValue`, decrement `rightPointer`.

**Edge Cases:**
- Negative numbers in the array.
- Target value being the sum of the first two or last two elements.
- Array with minimum size of 2.

**JavaScript Solution:**
```javascript
/**
 * @param {number[]} sortedNumbers
 * @param {number} targetValue
 * @return {number[]}
 */
const twoSum = (sortedNumbers, targetValue) => {
    let leftPointer = 0;
    let rightPointer = sortedNumbers.length - 1;

    while (leftPointer < rightPointer) {
        const currentSum = sortedNumbers[leftPointer] + sortedNumbers[rightPointer];

        if (currentSum === targetValue) {
            // LeetCode requires 1-based indexing for this specific problem
            return [leftPointer + 1, rightPointer + 1];
        }

        if (currentSum < targetValue) {
            leftPointer++;
        } else {
            rightPointer--;
        }
    }

    return [];
};
```

---

## 3Sum
**LeetCode Link:** [https://leetcode.com/problems/3sum/](https://leetcode.com/problems/3sum/)

**Complexity:** Time: $O(totalNumbers^2)$ | Space: $O(totalNumbers)$ or $O(\log totalNumbers)$ for sorting.

**Key Intuition:** Sort the array and iterate through, fixing one number and using Two Pointers to find the remaining pair that sums to zero.

**Pseudocode:**
1. Sort the input array numerically.
2. Iterate through the array with `currentIndex`.
3. If `currentIndex` value is the same as the previous, skip to avoid duplicates.
4. For each `currentIndex`, set `leftPointer` to `currentIndex + 1` and `rightPointer` to the end.
5. While `leftPointer < rightPointer`:
    - Calculate `totalSum`.
    - If sum is 0, add triplet to `resultsList` and move both pointers past duplicates.
    - If sum is too low, move `leftPointer` right; if too high, move `rightPointer` left.

**Edge Cases:**
- All zeros in the array.
- Array with fewer than 3 elements.
- Multiple duplicate triplets available in the input.

**JavaScript Solution:**
```javascript
/**
 * @param {number[]} numbers
 * @return {number[][]}
 */
const threeSum = (numbers) => {
    const resultsList = [];
    // Sort numbers in ascending order
    numbers.sort((firstValue, secondValue) => firstValue - secondValue);

    for (let currentIndex = 0; currentIndex < numbers.length - 2; currentIndex++) {
        // Avoid duplicate triplets by checking the previous element
        if (currentIndex > 0 && numbers[currentIndex] === numbers[currentIndex - 1]) {
            continue;
        }

        let leftPointer = currentIndex + 1;
        let rightPointer = numbers.length - 1;

        while (leftPointer < rightPointer) {
            const currentTotal = numbers[currentIndex] + numbers[leftPointer] + numbers[rightPointer];

            if (currentTotal === 0) {
                resultsList.push([numbers[currentIndex], numbers[leftPointer], numbers[rightPointer]]);
                
                // Skip identical values for the left and right pointers
                while (leftPointer < rightPointer && numbers[leftPointer] === numbers[leftPointer + 1]) {
                    leftPointer++;
                }
                while (leftPointer < rightPointer && numbers[rightPointer] === numbers[rightPointer - 1]) {
                    rightPointer--;
                }
                
                leftPointer++;
                rightPointer--;
            } else if (currentTotal < 0) {
                leftPointer++;
            } else {
                rightPointer--;
            }
        }
    }

    return resultsList;
};
```

---

## Container with Most Water
**LeetCode Link:** [https://leetcode.com/problems/container-with-most-water/](https://leetcode.com/problems/container-with-most-water/)

**Complexity:** Time: $O(totalHeights)$ | Space: $O(1)$

**Key Intuition:** The volume is limited by the shorter wall; move the shorter wall inward to seek
