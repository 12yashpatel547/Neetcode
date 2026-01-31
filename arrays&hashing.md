# Senior Software Engineer Study Guide: Arrays & Hashing (2026 Edition)
---

## 1. Contains Duplicate
[LeetCode 217](https://leetcode.com/problems/contains-duplicate/)

### Complexity
* **Time Complexity:** $O(totalElements)$
* **Space Complexity:** $O(totalElements)$

### Key Intuition
Trade memory for speed. By utilizing a `Set`, we achieve $O(1)$ lookups to check if we've encountered a value before, avoiding a nested loop comparison.

### Pseudocode
1. Initialize an empty `Set` named `seenElements`.
2. Iterate through each `currentNumber` in the input array.
3. If `currentNumber` already exists in `seenElements`, return `true`.
4. Otherwise, add `currentNumber` to `seenElements`.
5. If the loop completes, return `false`.

### Edge Cases
* **Empty Input:** Should return `false` as no duplicates can exist.
* **Single Element:** Should return `false`.
* **All Identical Elements:** Should return `true` immediately upon the second element.

### JavaScript Solution
```javascript
const containsDuplicate = (numbers) => {
    const seenElements = new Set();

    for (const currentNumber of numbers) {
        // If the number is already in the set, we found a duplicate
        if (seenElements.has(currentNumber)) {
            return true;
        }
        seenElements.add(currentNumber);
    }

    return false;
};
```

---

## 2. Valid Anagram
[LeetCode 242](https://leetcode.com/problems/valid-anagram/)

### Complexity
* **Time Complexity:** $O(totalCharacters)$
* **Space Complexity:** $O(1)$ — The hash map/object size is capped at 26 for English characters.

### Key Intuition
If two strings are anagrams, their character frequencies must match perfectly. Instead of sorting (which is $O(totalCharacters \log totalCharacters)$), we use a single frequency map to balance counts.

### Pseudocode
1. Check if `stringOne.length` equals `stringTwo.length`. If not, return `false`.
2. Initialize an empty object `characterCounts`.
3. Iterate through `stringOne` and increment the count for each character.
4. Iterate through `stringTwo` and decrement the count for each character.
5. Check if all values in `characterCounts` are zero.

### Edge Cases
* **Different Lengths:** Handled by the initial length check.
* **Empty Strings:** Technically anagrams, return `true`.
* **Case Sensitivity:** Usually, interviewers assume lowercase, but confirm if 'A' === 'a'.

### JavaScript Solution
```javascript
const isAnagram = (stringOne, stringTwo) => {
    if (stringOne.length !== stringTwo.length) {
        return false;
    }

    const characterCounts = {};

    for (let indexCounter = 0; indexCounter < stringOne.length; indexCounter++) {
        const charFromOne = stringOne[indexCounter];
        const charFromTwo = stringTwo[indexCounter];

        characterCounts[charFromOne] = (characterCounts[charFromOne] || 0) + 1;
        characterCounts[charFromTwo] = (characterCounts[charFromTwo] || 0) - 1;
    }

    // Every key must have a balance of 0 if they are anagrams
    for (const character in characterCounts) {
        if (characterCounts[character] !== 0) {
            return false;
        }
    }

    return true;
};
```

---

## 3. Two Sum
[LeetCode 1](https://leetcode.com/problems/two-sum/)



### Complexity
* **Time Complexity:** $O(totalElements)$
* **Space Complexity:** $O(totalElements)$

### Key Intuition
Instead of searching for two numbers, search for the **complement**. For any `currentNumber`, we are looking for `targetSum - currentNumber`. We store previously seen numbers in a Map for instant retrieval.

### Pseudocode
1. Initialize `numberToIndexMap`.
2. For each `currentNumber` and `indexCounter` in the array:
    - Calculate `neededComplement = targetSum - currentNumber`.
    - If `neededComplement` exists in the map, return `[map[neededComplement], indexCounter]`.
    - Otherwise, store `currentNumber` with its `indexCounter` in the map.

### Edge Cases
* **Negative Numbers:** The logic holds as `targetSum - (-number)` correctly calculates the required positive/negative value.
* **Same Number Used Twice:** The map ensures we don't use the same index unless two separate instances of the number exist in the array.

### JavaScript Solution
```javascript
const twoSum = (numbers, targetSum) => {
    const numberToIndexMap = new Map();

    for (let indexCounter = 0; indexCounter < numbers.length; indexCounter++) {
        const currentNumber = numbers[indexCounter];
        const neededComplement = targetSum - currentNumber;

        if (numberToIndexMap.has(neededComplement)) {
            return [numberToIndexMap.get(neededComplement), indexCounter];
        }

        numberToIndexMap.set(currentNumber, indexCounter);
    }

    return [];
};
```

---

## 4. Group Anagrams
[LeetCode 49](https://leetcode.com/problems/group-anagrams/)

### Complexity
* **Time Complexity:** $O(totalStrings \cdot averageStringLength)$
* **Space Complexity:** $O(totalStrings \cdot averageStringLength)$

### Key Intuition
We need a way to generate a **canonical key** for any group of anagrams. In 2026, we avoid sorting each string and instead use a frequency-count string (e.g., "1#0#2#0...") as the Map key.

### Pseudocode
1. Initialize `anagramGroupsMap`.
2. For each `currentString`:
    - Create a frequency array of size 26 for 'a'-'z'.
    - Populate the array by counting characters.
    - Convert that array into a delimited string (the `groupKey`).
    - Append `currentString` to the array at `anagramGroupsMap[groupKey]`.
3. Return all values from the map.

### Edge Cases
* **Empty String in List:** `""` should be grouped with other `""`.
* **Single Character Strings:** Should group correctly based on the character.

### JavaScript Solution
```javascript
const groupAnagrams = (stringList) => {
    const anagramGroupsMap = {};

    for (const currentString of stringList) {
        const characterFrequencies = new Array(26).fill(0);
        for (const character of currentString) {
            const charCodeIndex = character.charCodeAt(0) - 97; // 'a' is 97
            characterFrequencies[charCodeIndex]++;
        }

        // Generate a stable key like "1#0#2..."
        const groupKey = characterFrequencies.join("#");

        if (!anagramGroupsMap[groupKey]) {
            anagramGroupsMap[groupKey] = [];
        }
        anagramGroupsMap[groupKey].push(currentString);
    }

    return Object.values(anagramGroupsMap);
};
```

---

## 5. Top K Frequent Elements
[LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/)



### Complexity
* **Time Complexity:** $O(totalElements)$
* **Space Complexity:** $O(totalElements)$

### Key Intuition
Use **Bucket Sort**. The maximum frequency of any element is equal to the array length. We create "buckets" where the index represents the frequency. This bypasses the need for $O(N \log N)$ sorting.

### Pseudocode
1. Create a `frequencyMap` to count occurrences.
2. Create `buckets` (array of arrays) where `buckets.length` is `numbers.length + 1`.
3. For each `[number, count]` in the map, push `number` into `buckets[count]`.
4. Iterate backward through `buckets` and collect numbers until we have `k` elements.

### Edge Cases
* **k Equals Array Length:** All elements are returned.
* **All Elements have same frequency:** Any `k` elements are valid; the bucket logic handles this.

### JavaScript Solution
```javascript
const topKFrequent = (numbers, k) => {
    const frequencyMap = new Map();
    const buckets = Array.from({ length: numbers.length + 1 }, () => []);

    for (const currentNumber of numbers) {
        frequencyMap.set(currentNumber, (frequencyMap.get(currentNumber) || 0) + 1);
    }

    for (const [number, count] of frequencyMap) {
        buckets[count].push(number);
    }

    const topKResults = [];
    for (let bucketIndex = buckets.length - 1; bucketIndex >= 0; bucketIndex--) {
        for (const element of buckets[bucketIndex]) {
            topKResults.push(element);
            if (topKResults.length === k) {
                return topKResults;
            }
        }
    }
};
```

---

## 6. Product of Array Except Self
[LeetCode 238](https://leetcode.com/problems/product-of-array-except-self/)

### Complexity
* **Time Complexity:** $O(totalElements)$
* **Space Complexity:** $O(1)$ — Excluding the output array per problem constraints.

### Key Intuition
For any index `i`, the product is `(everything to the left) * (everything to the right)`. We can calculate prefix products in one pass and suffix products in another pass, combining them in the output array.

### Pseudocode
1. Initialize `outputProducts` with 1s.
2. Maintain `prefixAccumulator = 1`. Iterate left-to-right:
    - Set `outputProducts[index]` to `prefixAccumulator`.
    - Update `prefixAccumulator` by multiplying it with `numbers[index]`.
3. Maintain `suffixAccumulator = 1`. Iterate right-to-left:
    - Multiply `outputProducts[index]` by `suffixAccumulator`.
    - Update `suffixAccumulator` by multiplying it with `numbers[index]`.

### Edge Cases
* **Array contains one zero:** All products except the zero-index will be 0.
* **Array contains multiple zeros:** All products will be 0.

### JavaScript Solution
```javascript
const productExceptSelf = (numbers) => {
    const totalLength = numbers.length;
    const outputProducts = new Array(totalLength).fill(1);

    let leftProductAccumulator = 1;
    for (let indexCounter = 0; indexCounter < totalLength; indexCounter++) {
        outputProducts[indexCounter] = leftProductAccumulator;
        leftProductAccumulator *= numbers[indexCounter];
    }

    let rightProductAccumulator = 1;
    for (let indexCounter = totalLength - 1; indexCounter >= 0; indexCounter--) {
        outputProducts[indexCounter] *= rightProductAccumulator;
        rightProductAccumulator *= numbers[indexCounter];
    }

    return outputProducts;
};
```

---

## 7. Valid Sudoku
[LeetCode 36](https://leetcode.com/problems/valid-sudoku/)

### Complexity
* **Time Complexity:** $O(1)$ — Grid is always $9 \times 9$.
* **Space Complexity:** $O(1)$ — Memory used for sets is constant.

### Key Intuition
Validate three rules: unique in Row, unique in Column, unique in $3 \times 3$ Box. The challenge is calculating the Box index. A reliable formula is `Math.floor(rowIndex / 3) * 3 + Math.floor(columnIndex / 3)`.

### Pseudocode
1. Create 9 Sets for rows, 9 for columns, and 9 for boxes.
2. Nested loop through all cells `(rowIndex, columnIndex)`.
3. Skip empty cells (".").
4. Identify the `boxIndex`.
5. If the digit is already in the corresponding row/col/box set, return `false`.
6. Add the digit to the sets.
7. Return `true` if finish.

### Edge Cases
* **Empty Board:** Return `true`.
* **Invalid Board with Multiple Violations:** Returns `false` at the first one found.

### JavaScript Solution
```javascript
const isValidSudoku = (board) => {
    const rows = Array.from({ length: 9 }, () => new Set());
    const cols = Array.from({ length: 9 }, () => new Set());
    const boxes = Array.from({ length: 9 }, () => new Set());

    for (let rowIndex = 0; rowIndex < 9; rowIndex++) {
        for (let colIndex = 0; colIndex < 9; colIndex++) {
            const currentDigit = board[rowIndex][colIndex];
            
            if (currentDigit === ".") continue;

            const boxIndex = Math.floor(rowIndex / 3) * 3 + Math.floor(colIndex / 3);

            if (rows[rowIndex].has(currentDigit) || 
                cols[colIndex].has(currentDigit) || 
                boxes[boxIndex].has(currentDigit)) {
                return false;
            }

            rows[rowIndex].add(currentDigit);
            cols[colIndex].add(currentDigit);
            boxes[boxIndex].add(currentDigit);
        }
    }

    return true;
};
```

---

## 8. Encode and Decode Strings
[LintCode 659](https://www.lintcode.com/problem/659/)

### Complexity
* **Time Complexity:** $O(totalCharacters)$
* **Space Complexity:** $O(totalCharacters)$

### Key Intuition
We cannot use a single delimiter because any character could be part of the string. The standard Senior approach is **Length-Prefixing**. We store `length + delimiter + string`.

### Pseudocode
- **Encode**: For each string, append its length, a "#" symbol, and the string to a final builder string.
- **Decode**: 
    1. Read the string until you hit "#" to get the `length`.
    2. Slice the next `length` characters as the string.
    3. Repeat from the new position.

### Edge Cases
* **Strings containing the delimiter:** `4#3#hi` (the string is `3#hi`). The length prefix `4` prevents confusion.
* **Empty string in list:** Encoded as `0#`.

### JavaScript Solution
```javascript
class StringCodec {
    /**
     * @param {string[]} stringList
     * @return {string}
     */
    encode(stringList) {
        let encodedResult = "";
        for (const currentString of stringList) {
            encodedResult += `${currentString.length}#${currentString}`;
        }
        return encodedResult;
    }

    /**
     * @param {string} encodedString
     * @return {string[]}
     */
    decode(encodedString) {
        const decodedList = [];
        let indexPointer = 0;

        while (indexPointer < encodedString.length) {
            let delimiterIndex = encodedString.indexOf("#", indexPointer);
            let stringLength = parseInt(encodedString.substring(indexPointer, delimiterIndex));
            
            let stringStart = delimiterIndex + 1;
            let stringEnd = stringStart + stringLength;
            
            decodedList.push(encodedString.substring(stringStart, stringEnd));
            indexPointer = stringEnd;
        }

        return decodedList;
    }
}
```

---

## 9. Longest Consecutive Sequence
[LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/)

### Complexity
* **Time Complexity:** $O(totalElements)$
* **Space Complexity:** $O(totalElements)$

### Key Intuition
To achieve $O(N)$, we must avoid sorting. We use a Set for fast lookups. The trick is to only start counting a sequence if the current number is the **start** of a sequence (i.e., `num - 1` is not in the set).

### Pseudocode
1. Insert all numbers into `numberSet`.
2. For each `currentNumber` in the set:
    - If `currentNumber - 1` is NOT in the set:
        - We found a potential start.
        - While `currentNumber + 1` exists in the set, increment `currentStreak`.
        - Update `maxStreak`.
3. Return `maxStreak`.

### Edge Cases
* **Empty Input:** Return 0.
* **Duplicate Numbers:** Set handles duplicates automatically.

### JavaScript Solution
```javascript
const longestConsecutive = (numbers) => {
    const numberSet = new Set(numbers);
    let maxSequenceLength = 0;

    for (const currentNumber of numberSet) {
        // Only start a sequence if currentNumber is the beginning
        if (!numberSet.has(currentNumber - 1)) {
            let sequenceTracker = currentNumber;
            let currentSequenceLength = 1;

            while (numberSet.has(sequenceTracker + 1)) {
                sequenceTracker++;
                currentSequenceLength++;
            }

            maxSequenceLength = Math.max(maxSequenceLength, currentSequenceLength);
        }
    }

    return maxSequenceLength;
};
```

---

## 10. Sort Colors
[LeetCode 75](https://leetcode.com/problems/sort-colors/)



### Complexity
* **Time Complexity:** $O(totalElements)$
* **Space Complexity:** $O(1)$

### Key Intuition
This is the **Dutch National Flag** algorithm. We use three pointers to partition the array into 0s, 1s, and 2s in a single pass. 

### Pseudocode
1. `lowPointer = 0`, `currentPointer = 0`, `highPointer = length - 1`.
2. While `currentPointer <= highPointer`:
    - If `colors[currentPointer]` is 0: swap with `lowPointer`, increment both.
    - If `colors[currentPointer]` is 2: swap with `highPointer`, decrement `highPointer`. (Do NOT increment `currentPointer` because the swapped value must be inspected).
    - If `colors[currentPointer]` is 1: increment `currentPointer`.

### Edge Cases
* **All same colors:** Pointers will reach the end without swapping or with logical swaps.
* **Already sorted:** Logic remains $O(N)$.

### JavaScript Solution
```javascript
const sortColors = (colors) => {
    let lowBoundary = 0;
    let currentInspector = 0;
    let highBoundary = colors.length - 1;

    while (currentInspector <= highBoundary) {
        if (colors[currentInspector] === 0) {
            // Swap 0 to the front
            [colors[lowBoundary], colors[currentInspector]] = [colors[currentInspector], colors[lowBoundary]];
            lowBoundary++;
            currentInspector++;
        } else if (colors[currentInspector] === 2) {
            // Swap 2 to the back
            [colors[highBoundary], colors[currentInspector]] = [colors[currentInspector], colors[highBoundary]];
            highBoundary--;
            // Note: We don't increment currentInspector here
        } else {
            // It's a 1, just move forward
            currentInspector++;
        }
    }
};
```

---

## 11. [LATEST 2026 TREND] First Completely Painted Row or Column
[LeetCode 2661](https://leetcode.com/problems/first-completely-painted-row-or-column/)

### Complexity
* **Time Complexity:** $O(rows \cdot columns)$
* **Space Complexity:** $O(rows \cdot columns)$

### Key Intuition
This problem tests **Coordinate Hashing**. You map every matrix value to its coordinates. Then, treat the "painting" as incrementing frequency counters for the specific row and column.

### Pseudocode
1. Map every value in `matrix` to its `[rowIndex, colIndex]` in a Map.
2. Initialize `rowCountTracking` and `colCountTracking` arrays with 0.
3. Iterate through `paintingOrder` by `orderIndex`:
    - Get the coordinates for the current value.
    - Increment the count for that row and that column.
    - If `rowCountTracking[row] === totalCols` or `colCountTracking[col] === totalRows`, return `orderIndex`.

### Edge Cases
* **1x1 Matrix:** Returns index 0 immediately.
* **Large Matrix, Short Sequence:** Ensure memory doesn't overflow (though $O(N)$ is expected).

### JavaScript Solution
```javascript
const firstCompleteIndex = (paintingOrder, matrix) => {
    const totalRows = matrix.length;
    const totalCols = matrix[0].length;
    const valueToPositionMap = new Map();

    // Map values to their matrix coordinates
    for (let rowIndex = 0; rowIndex < totalRows; rowIndex++) {
        for (let colIndex = 0; colIndex < totalCols; colIndex++) {
            valueToPositionMap.set(matrix[rowIndex][colIndex], [rowIndex, colIndex]);
        }
    }

    const paintedInRow = new Array(totalRows).fill(0);
    const paintedInCol = new Array(totalCols).fill(0);

    for (let orderIndex = 0; orderIndex < paintingOrder.length; orderIndex++) {
        const [row, col] = valueToPositionMap.get(paintingOrder[orderIndex]);

        paintedInRow[row]++;
        paintedInCol[col]++;

        // Check if row or column is fully painted
        if (paintedInRow[row] === totalCols || paintedInCol[col] === totalRows) {
            return orderIndex;
        }
    }

    return -1;
};
```

---

## 12. [LATEST 2026 TREND] Divide Array Into Arrays With Max Difference
[LeetCode 2966](https://leetcode.com/problems/divide-array-into-arrays-with-max-difference/)

### Complexity
* **Time Complexity:** $O(totalElements \log totalElements)$
* **Space Complexity:** $O(totalElements)$

### Key Intuition
This modern hybrid requires sorting followed by grouping. Sorting guarantees that for any trio of elements to satisfy `max - min <= k`, they must be adjacent. We use index-based grouping to validate the constraint.

### Pseudocode
1. Sort the `numbers` array numerically.
2. Initialize `dividedResults`.
3. Iterate through `numbers` in steps of 3:
    - Compare the third element of the group with the first.
    - If `third - first > maxDifference`, return `[]` (impossible).
    - Otherwise, push the trio into `dividedResults`.
4. Return the result list.

### Edge Cases
* **k is 0:** Only identical numbers can be grouped.
* **Array length not divisible by 3:** (Problem usually guarantees divisible length, but handle if needed).

### JavaScript Solution
```javascript
const divideArray = (numbers, maxDifference) => {
    // Sorting is necessary to find the closest elements
    numbers.sort((a, b) => a - b);
    const dividedResults = [];

    for (let indexCounter = 0; indexCounter < numbers.length; indexCounter += 3) {
        const firstElement = numbers[indexCounter];
        const thirdElement = numbers[indexCounter + 2];

        // Because it's sorted, if (third - first) > k, the condition is impossible
        if (thirdElement - firstElement > maxDifference) {
            return [];
        }

        dividedResults.push([
            numbers[indexCounter],
            numbers[indexCounter + 1],
            numbers[indexCounter + 2]
        ]);
    }

    return dividedResults;
};
```
