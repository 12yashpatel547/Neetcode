# Senior Software Engineer Study Guide: Arrays & Hashing (2026 Masterclass)

This study guide focuses on the **Arrays & Hashing** pattern. In 2026, high-tier technical interviews (Google, Meta, OpenAI) have shifted beyond simple map lookups. They now prioritize **space-efficiency**, **one-pass algorithms**, and the ability to map complex state into flattened structures.

---

## 1. Contains Duplicate
[LeetCode 217](https://leetcode.com/problems/contains-duplicate/)

### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

### Key Intuition
Use a `Set` to provide $O(1)$ average-time lookups. By trading space for time, we avoid the $O(n^2)$ brute-force approach.

### Pseudocode
1. Initialize an empty `Set` called `seenElements`.
2. Iterate through each `currentNumber` in the input array.
3. If `seenElements` already contains `currentNumber`, return `true` (duplicate found).
4. Otherwise, add `currentNumber` to `seenElements`.
5. If the loop finishes without a match, return `false`.

### Edge Cases
* **Empty Array:** Return `false`.
* **Single Element:** Return `false`.

### JavaScript Solution
```javascript
const containsDuplicate = (numbers) => {
    const seenElements = new Set();
    for (const currentNumber of numbers) {
        if (seenElements.has(currentNumber)) return true;
        seenElements.add(currentNumber);
    }
    return false;
};
```

---

## 2. Valid Anagram
[LeetCode 242](https://leetcode.com/problems/valid-anagram/)

### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$ — Bound by the alphabet size (26).

### Key Intuition
Instead of sorting, we use a frequency map. If two strings are anagrams, their character counts must be identical across the entire alphabet.

### Pseudocode
1. If lengths of `stringOne` and `stringTwo` differ, return `false`.
2. Initialize a `characterFrequencyMap` (object or array of size 26).
3. Loop through the strings:
    - Increment the count for the character in `stringOne`.
    - Decrement the count for the character in `stringTwo`.
4. Iterate through the map: if any count is not zero, return `false`.
5. Return `true`.

### Edge Cases
* **Different Lengths:** Immediate `false`.
* **Empty Strings:** Technically anagrams, return `true`.

### JavaScript Solution
```javascript
const isAnagram = (stringOne, stringTwo) => {
    if (stringOne.length !== stringTwo.length) return false;
    const characterFrequencyMap = {};

    for (let indexCounter = 0; indexCounter < stringOne.length; indexCounter++) {
        characterFrequencyMap[stringOne[indexCounter]] = (characterFrequencyMap[stringOne[indexCounter]] || 0) + 1;
        characterFrequencyMap[stringTwo[indexCounter]] = (characterFrequencyMap[stringTwo[indexCounter]] || 0) - 1;
    }

    return Object.values(characterFrequencyMap).every(count => count === 0);
};
```

---

## 3. Two Sum
[LeetCode 1](https://leetcode.com/problems/two-sum/)



### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

### Key Intuition
Use a "Complementary Lookup" strategy. For every number, calculate what value is needed to reach the target. If we've seen that value before, we have a pair.

### Pseudocode
1. Initialize an empty Map `valueToIndexMap`.
2. Iterate through `numbersArray` with `currentIndex`.
3. Calculate `requiredComplement = targetSum - currentNumber`.
4. Check if `valueToIndexMap` contains `requiredComplement`.
5. If it does, return `[valueToIndexMap.get(requiredComplement), currentIndex]`.
6. Else, store `currentNumber` and `currentIndex` in the map.

### JavaScript Solution
```javascript
const twoSum = (numbers, targetSum) => {
    const valueToIndexMap = new Map();
    for (let indexCounter = 0; indexCounter < numbers.length; indexCounter++) {
        const currentNumber = numbers[indexCounter];
        const requiredComplement = targetSum - currentNumber;
        if (valueToIndexMap.has(requiredComplement)) {
            return [valueToIndexMap.get(requiredComplement), indexCounter];
        }
        valueToIndexMap.set(currentNumber, indexCounter);
    }
};
```

---

## 4. Group Anagrams
[LeetCode 49](https://leetcode.com/problems/group-anagrams/)

### Complexity
* **Time Complexity:** $O(n \cdot m)$ where $m$ is avg string length.
* **Space Complexity:** $O(n \cdot m)$

### Key Intuition
Generate a unique key for each group of anagrams. In 2026, high-performance solutions use a frequency-count string (e.g., "1#0#2...") as the key to avoid $O(m \log m)$ sorting.

### Pseudocode
1. Initialize `anagramGroupsMap` (Object).
2. For each `currentString` in the input:
    - Create a frequency array of size 26.
    - Count each character in `currentString`.
    - Join the array with a delimiter (e.g., "#") to create a `uniqueKey`.
    - Append `currentString` to the list at `anagramGroupsMap[uniqueKey]`.
3. Return the values of the map.

### JavaScript Solution
```javascript
const groupAnagrams = (stringList) => {
    const groupsMap = {};
    for (const currentString of stringList) {
        const countsArray = new Array(26).fill(0);
        for (const character of currentString) {
            countsArray[character.charCodeAt(0) - 97]++;
        }
        const uniqueKey = countsArray.join("#");
        if (!groupsMap[uniqueKey]) groupsMap[uniqueKey] = [];
        groupsMap[uniqueKey].push(currentString);
    }
    return Object.values(groupsMap);
};
```

---

## 5. Top K Frequent Elements
[LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/)



### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

### Key Intuition
Bucket Sort. The maximum frequency an element can have is the length of the array. Use an array of lists where the index is the frequency.

### Pseudocode
1. Count the frequency of each number using `frequencyMap`.
2. Create `bucketList`, an array of empty arrays, with length `inputArray.length + 1`.
3. For each `number` in `frequencyMap`, place it in `bucketList[frequency]`.
4. Iterate through `bucketList` from the end (highest frequency) to the beginning.
5. Collect elements until the result array has `k` elements.

### JavaScript Solution
```javascript
const topKFrequent = (numbers, k) => {
    const frequencyMap = new Map();
    const buckets = Array.from({ length: numbers.length + 1 }, () => []);
    
    for (const number of numbers) {
        frequencyMap.set(number, (frequencyMap.get(number) || 0) + 1);
    }
    
    for (const [number, count] of frequencyMap) {
        buckets[count].push(number);
    }
    
    const topElementsResult = [];
    for (let indexCounter = buckets.length - 1; indexCounter >= 0; indexCounter--) {
        for (const element of buckets[indexCounter]) {
            topElementsResult.push(element);
            if (topElementsResult.length === k) return topElementsResult;
        }
    }
};
```

---

## 6. Product of Array Except Self
[LeetCode 238](https://leetcode.com/problems/product-of-array-except-self/)

### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$ (excluding output array)

### Key Intuition
An element's result is the product of all elements to its left multiplied by all elements to its right. We can calculate these in two passes.

### Pseudocode
1. Initialize `resultArray` filled with 1s.
2. Initialize `prefixProduct` to 1.
3. Pass 1 (Left to Right): Store `prefixProduct` in `resultArray[i]`, then multiply `prefixProduct` by `numbers[i]`.
4. Initialize `suffixProduct` to 1.
5. Pass 2 (Right to Left): Multiply `resultArray[i]` by `suffixProduct`, then multiply `suffixProduct` by `numbers[i]`.

### JavaScript Solution
```javascript
const productExceptSelf = (numbers) => {
    const resultArray = new Array(numbers.length).fill(1);
    let cumulativeProduct = 1;

    for (let indexCounter = 0; indexCounter < numbers.length; indexCounter++) {
        resultArray[indexCounter] = cumulativeProduct;
        cumulativeProduct *= numbers[indexCounter];
    }

    cumulativeProduct = 1;
    for (let indexCounter = numbers.length - 1; indexCounter >= 0; indexCounter--) {
        resultArray[indexCounter] *= cumulativeProduct;
        cumulativeProduct *= numbers[indexCounter];
    }

    return resultArray;
};
```

---

## 7. Valid Sudoku
[LeetCode 36](https://leetcode.com/problems/valid-sudoku/)

### Complexity
* **Time Complexity:** $O(1)$ (Fixed grid)
* **Space Complexity:** $O(1)$

### Key Intuition
Use sets to track digits for each row, column, and $3 \times 3$ sub-box. The box index is calculated as `(row/3)*3 + (col/3)`.

### Pseudocode
1. Create 9 Sets each for `rows`, `cols`, and `boxes`.
2. Iterate through every cell in the $9 \times 9$ grid.
3. If cell is empty ("."), skip it.
4. Check if the value exists in `rows[r]`, `cols[c]`, or `boxes[boxIndex]`.
5. If exists, return `false`.
6. Else, add the value to all three sets.
7. Return `true` if loop completes.

### JavaScript Solution
```javascript
const isValidSudoku = (board) => {
    const rowSets = Array.from({ length: 9 }, () => new Set());
    const colSets = Array.from({ length: 9 }, () => new Set());
    const boxSets = Array.from({ length: 9 }, () => new Set());

    for (let rowIndex = 0; rowIndex < 9; rowIndex++) {
        for (let colIndex = 0; colIndex < 9; colIndex++) {
            const digit = board[rowIndex][colIndex];
            if (digit === '.') continue;

            const boxIndex = Math.floor(rowIndex / 3) * 3 + Math.floor(colIndex / 3);

            if (rowSets[rowIndex].has(digit) || 
                colSets[colIndex].has(digit) || 
                boxSets[boxIndex].has(digit)) return false;

            rowSets[rowIndex].add(digit);
            colSets[colIndex].add(digit);
            boxSets[boxIndex].add(digit);
        }
    }
    return true;
};
```

---

## 8. Encode and Decode Strings
[LintCode 659](https://www.lintcode.com/problem/659/)

### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

### Key Intuition
Prefix each string with its length and a delimiter (e.g., `length + "#" + string`). This handles cases where the delimiter itself is part of the original string.

### Pseudocode
- **Encode**: For each string, append its length, a "#" character, and the string itself to a `resultString`.
- **Decode**: 
    1. While `index` is less than string length:
    2. Find the index of the next "#".
    3. Parse the integer before "#" as `stringLength`.
    4. Slice the string from `hashIndex + 1` to `hashIndex + 1 + stringLength`.
    5. Append to result and update `index`.

### JavaScript Solution
```javascript
class Codec {
    encode(stringList) {
        return stringList.map(item => `${item.length}#${item}`).join("");
    }

    decode(encodedString) {
        const decodedList = [];
        let indexCounter = 0;
        while (indexCounter < encodedString.length) {
            let hashIndex = encodedString.indexOf("#", indexCounter);
            let stringLength = parseInt(encodedString.substring(indexCounter, hashIndex));
            let actualString = encodedString.substring(hashIndex + 1, hashIndex + 1 + stringLength);
            decodedList.push(actualString);
            indexCounter = hashIndex + 1 + stringLength;
        }
        return decodedList;
    }
}
```

---

## 9. Longest Consecutive Sequence
[LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/)

### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

### Key Intuition
Find the start of a sequence. A number `x` is the start of a sequence only if `x - 1` is not in the set.

### Pseudocode
1. Put all numbers into a `numberSet`.
2. Initialize `maxStreak` to 0.
3. For each `number` in the set:
    - If `number - 1` is NOT in the set:
        - This is the start of a sequence.
        - Count how many consecutive numbers exist (`number + 1`, `number + 2`...).
        - Update `maxStreak`.
4. Return `maxStreak`.

### JavaScript Solution
```javascript
const longestConsecutive = (numbers) => {
    const numberSet = new Set(numbers);
    let maxStreak = 0;

    for (const number of numberSet) {
        if (!numberSet.has(number - 1)) {
            let currentNumber = number;
            let currentStreak = 1;

            while (numberSet.has(currentNumber + 1)) {
                currentNumber++;
                currentStreak++;
            }
            maxStreak = Math.max(maxStreak, currentStreak);
        }
    }
    return maxStreak;
};
```

---

## 10. Sort Colors
[LeetCode 75](https://leetcode.com/problems/sort-colors/)

### Complexity
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$

### Key Intuition
Dutch National Flag algorithm. Maintain three pointers: `low` (boundary for 0s), `high` (boundary for 2s), and `current` (the scanner).

### Pseudocode
1. Initialize `lowPointer = 0`, `currentPointer = 0`, `highPointer = length - 1`.
2. While `currentPointer <= highPointer`:
    - If `array[currentPointer]` is 0: Swap with `lowPointer`, increment both.
    - If `array[currentPointer]` is 2: Swap with `highPointer`, decrement `highPointer` (don't increment `current` yet).
    - If `array[currentPointer]` is 1: Just increment `currentPointer`.

### JavaScript Solution
```javascript
const sortColors = (colors) => {
    let lowPointer = 0;
    let currentPointer = 0;
    let highPointer = colors.length - 1;

    while (currentPointer <= highPointer) {
        if (colors[currentPointer] === 0) {
            [colors[lowPointer], colors[currentPointer]] = [colors[currentPointer], colors[lowPointer]];
            lowPointer++;
            currentPointer++;
        } else if (colors[currentPointer] === 2) {
            [colors[highPointer], colors[currentPointer]] = [colors[currentPointer], colors[highPointer]];
            highPointer--;
        } else {
            currentPointer++;
        }
    }
};
```

---

## 11. [TRENDING 2026] First Completely Painted Row or Column
[LeetCode 2661](https://leetcode.com/problems/first-completely-painted-row-or-column/)

### Complexity
* **Time Complexity:** $O(rows \cdot cols)$
* **Space Complexity:** $O(rows \cdot cols)$

### Key Intuition
Pre-calculate the coordinates of every value in the matrix. As you iterate through the painting order, increment row and column counters.

### Pseudocode
1. Create a `positionMap` mapping each value to its `[row, col]` index.
2. Initialize `rowCountArray` and `colCountArray` to zeros.
3. Iterate through `paintingOrder` with `indexCounter`:
    - Retrieve `[r, c]` for the current value from the map.
    - Increment `rowCountArray[r]` and `colCountArray[c]`.
    - If `rowCountArray[r] === totalColumns` or `colCountArray[c] === totalRows`, return `indexCounter`.

### JavaScript Solution
```javascript
const firstCompleteIndex = (paintOrder, matrix) => {
    const rowCount = matrix.length;
    const colCount = matrix[0].length;
    const positionMap = new Map();

    for (let r = 0; r < rowCount; r++) {
        for (let c = 0; c < colCount; c++) {
            positionMap.set(matrix[r][c], [r, c]);
        }
    }

    const rowsPainted = new Array(rowCount).fill(0);
    const colsPainted = new Array(colCount).fill(0);

    for (let index = 0; index < paintOrder.length; index++) {
        const [r, c] = positionMap.get(paintOrder[index]);
        rowsPainted[r]++;
        colsPainted[c]++;
        if (rowsPainted[r] === colCount || colsPainted[c] === rowCount) return index;
    }
};
```

---

## 12. [TRENDING 2026] Divide Array Into Arrays With Max Difference
[LeetCode 2966](https://leetcode.com/problems/divide-array-into-arrays-with-max-difference/)

### Complexity
* **Time Complexity:** $O(n \log n)$ (due to sorting)
* **Space Complexity:** $O(n)$

### Key Intuition
In 2026, many "Hashing" problems are actually "Sorting + Hashing/Grouping" hybrids. Sorting the array ensures that the three closest elements are always adjacent.

### Pseudocode
1. Sort the input array `numbers`.
2. Initialize `resultGroups`.
3. Loop through `numbers` in steps of 3:
    - If `numbers[i+2] - numbers[i] > maxDifference`, return an empty array (impossible).
    - Otherwise, push `[numbers[i], numbers[i+1], numbers[i+2]]` to `resultGroups`.
4. Return `resultGroups`.

### JavaScript Solution
```javascript
const divideArray = (numbers, maxDifference) => {
    numbers.sort((numA, numB) => numA - numB);
    const resultGroups = [];

    for (let indexCounter = 0; indexCounter < numbers.length; indexCounter += 3) {
        const firstValue = numbers[indexCounter];
        const thirdValue = numbers[indexCounter + 2];
        
        if (thirdValue - firstValue > maxDifference) return [];
        
        resultGroups.push([numbers[indexCounter], numbers[indexCounter + 1], numbers[indexCounter + 2]]);
    }
    return resultGroups;
};
```
