# Technical Interview Study Guide: Stack Pattern

## Valid Parentheses
**LeetCode Link:** [https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/)

**Complexity:** Time: $O(totalCharacters)$ | Space: $O(totalCharacters)$

**Key Intuition:** Use a stack to track pending open brackets and ensure the last bracket opened is the first one closed by a matching pair.



**Pseudocode:**
1. Initialize an empty `bracketStack`.
2. Define a `bracketMap` where keys are closing brackets and values are corresponding opening brackets.
3. Iterate through each `currentCharacter` in the string:
    - If it is an opening bracket, push it to the `bracketStack`.
    - If it is a closing bracket:
        - Pop the `topElement` from the stack.
        - If the popped element does not match the `bracketMap` value for the current character, return false.
4. Return true if the `bracketStack` is empty after the loop.

**Edge Cases:**
- String starts with a closing bracket.
- String contains only opening brackets.
- Empty string (though usually not provided per constraints).

**JavaScript Solution:**
```javascript
/**
 * @param {string} inputString
 * @return {boolean}
 */
const isValid = (inputString) => {
    const bracketStack = [];
    const bracketMap = {
        ')': '(',
        '}': '{',
        ']': '['
    };

    for (const currentCharacter of inputString) {
        // If the character is a key in the map, it is a closing bracket
        if (bracketMap[currentCharacter]) {
            const lastOpenedBracket = bracketStack.pop();
            if (lastOpenedBracket !== bracketMap[currentCharacter]) {
                return false;
            }
        } else {
            // Otherwise, it is an opening bracket
            bracketStack.push(currentCharacter);
        }
    }

    return bracketStack.length === 0;
};
```

---

## Min Stack
**LeetCode Link:** [https://leetcode.com/problems/min-stack/](https://leetcode.com/problems/min-stack/)

**Complexity:** Time: $O(1)$ for all operations | Space: $O(totalElements)$

**Key Intuition:** Maintain a parallel stack that tracks the minimum value present at every level of the main stack.

**Pseudocode:**
1. Maintain a `primaryStack` to store all pushed values.
2. Maintain a `minimumValueStack` to store the minimum value encountered up to that point.
3. On `push(currentValue)`:
    - Push to `primaryStack`.
    - Compare `currentValue` with the top of `minimumValueStack`. Push the smaller of the two to `minimumValueStack`.
4. On `pop()`: Pop from both stacks to maintain synchronization.
5. On `getMin()`: Return the top of `minimumValueStack`.

**Edge Cases:**
- Popping the last element.
- Pushing duplicate minimum values.

**JavaScript Solution:**
```javascript
class MinStack {
    constructor() {
        this.primaryDataStack = [];
        this.minimumHistoryStack = [];
    }

    /**
     * @param {number} newValue
     * @return {void}
     */
    push(newValue) {
        this.primaryDataStack.push(newValue);
        
        const currentMinimum = this.minimumHistoryStack.length === 0 
            ? newValue 
            : Math.min(newValue, this.minimumHistoryStack[this.minimumHistoryStack.length - 1]);
            
        this.minimumHistoryStack.push(currentMinimum);
    }

    /**
     * @return {void}
     */
    pop() {
        this.primaryDataStack.pop();
        this.minimumHistoryStack.pop();
    }

    /**
     * @return {number}
     */
    top() {
        return this.primaryDataStack[this.primaryDataStack.length - 1];
    }

    /**
     * @return {number}
     */
    getMin() {
        return this.minimumHistoryStack[this.minimumHistoryStack.length - 1];
    }
}
```

---

## Evaluate Reverse Polish Notation
**LeetCode Link:** [https://leetcode.com/problems/evaluate-reverse-polish-notation/](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

**Complexity:** Time: $O(totalTokens)$ | Space: $O(totalTokens)$

**Key Intuition:** Use a stack to hold operands and immediately apply any operator encountered to the top two elements of the stack.

**Pseudocode:**
1. Initialize an `operandStack`.
2. Iterate through `tokensList`.
3. If `currentToken` is a number, push it to the stack.
4. If `currentToken` is an operator:
    - Pop the second operand (`rightValue`).
    - Pop the first operand (`leftValue`).
    - Apply the operation and push the result back.
5. Note: Division must truncate toward zero.

**Edge Cases:**
- Division resulting in negative decimals (must truncate correctly).
- Tokens list with only one number.

**JavaScript Solution:**
```javascript
/**
 * @param {string[]} tokensList
 * @return {number}
 */
const evalRPN = (tokensList) => {
    const operandStack = [];
    const mathematicalOperations = {
        '+': (leftValue, rightValue) => leftValue + rightValue,
        '-': (leftValue, rightValue) => leftValue - rightValue,
        '*': (leftValue, rightValue) => leftValue * rightValue,
        '/': (leftValue, rightValue) => Math.trunc(leftValue / rightValue)
    };

    for (const currentToken of tokensList) {
        if (mathematicalOperations[currentToken]) {
            const secondOperand = operandStack.pop();
            const firstOperand = operandStack.pop();
            const operationResult = mathematicalOperations[currentToken](firstOperand, secondOperand);
            operandStack.push(operationResult);
        } else {
            operandStack.push(Number(currentToken));
        }
    }

    return operandStack.pop();
};
```

---

## Generate Parentheses
**LeetCode Link:** [https://leetcode.com/problems/generate-parentheses/](https://leetcode.com/problems/generate-parentheses/)

**Complexity:** Time: $O(\frac{4^{n}}{\sqrt{n}})$ (Catalan number) | Space: $O(n)$

**Key Intuition:** Use backtracking to explore all valid combinations, ensuring the count of closing brackets never exceeds the count of opening brackets at any step.



**Pseudocode:**
1. Initialize a `validCombinationsList`.
2. Define a recursive `backtrack` function with `currentString`, `openCount`, and `closedCount`.
3. If `currentString.length` equals `pairCount * 2`, add it to the results.
4. If `openCount < pairCount`, add an opening bracket and recurse.
5. If `closedCount < openCount`, add a closing bracket and recurse.

**Edge Cases:**
- `pairCount` is 1.
- Maximum constraints (usually $n=8$).

**JavaScript Solution:**
```javascript
/**
 * @param {number} pairCount
 * @return {string[]}
 */
const generateParenthesis = (pairCount) => {
    const validCombinationsList = [];

    const generateCombinations = (currentString, openCount, closedCount) => {
        // Base case: String is complete
        if (currentString.length === pairCount * 2) {
            validCombinationsList.push(currentString);
            return;
        }

        // Logic: You can always add an opening bracket if you have not used them all
        if (openCount < pairCount) {
            generateCombinations(currentString + "(", openCount + 1, closedCount);
        }

        // Logic: You can only add a closing bracket if there is an open one to match it
        if (closedCount < openCount) {
            generateCombinations(currentString + ")", openCount, closedCount + 1);
        }
    };

    generateCombinations("", 0, 0);
    return validCombinationsList;
};
```

---

## Daily Temperatures
**LeetCode Link:** [https://leetcode.com/problems/daily-temperatures/](https://leetcode.com/problems/daily-temperatures/)

**Complexity:** Time: $O(totalDays)$ | Space: $O(totalDays)$

**Key Intuition:** Maintain a monotonic decreasing stack to store indices of days whose next warmer temperature has not yet been found.



**Pseudocode:**
1. Initialize a `waitingDaysResult` array filled with zeros.
2. Initialize an empty `monotonicIndexStack`.
3. Iterate through `temperatureList` with `currentDayIndex`:
    - While the current temperature is warmer than the temperature at the index on top of the stack:
        - Pop the `previousDayIndex`.
        - Calculate the difference (`currentDayIndex - previousDayIndex`) and store it in the result.
    - Push the `currentDayIndex` onto the stack.

**Edge Cases:**
- Temperatures are in strictly decreasing order.
- Temperatures are all the same.
- Warmer temperature is only found on the very last day.

**JavaScript Solution:**
```javascript
/**
 * @param {number[]} temperatureList
 * @return {number[]}
 */
const dailyTemperatures = (temperatureList) => {
    const totalDays = temperatureList.length;
    const waitingDaysResult = new Array(totalDays).fill(0);
    const monotonicIndexStack = [];

    for (let currentDayIndex = 0; currentDayIndex < totalDays; currentDayIndex++) {
        const currentTemperature = temperatureList[currentDayIndex];

        while (
            monotonicIndexStack.length > 0 && 
            currentTemperature > temperatureList[monotonicIndexStack[monotonicIndexStack.length - 1]]
        ) {
            const previousDayIndex = monotonicIndexStack.pop();
            waitingDaysResult[previousDayIndex] = currentDayIndex - previousDayIndex;
        }

        monotonicIndexStack.push(currentDayIndex);
    }

    return waitingDaysResult;
};
```

---

## Car Fleet
**LeetCode Link:** [https://leetcode.com/problems/car-fleet/](https://leetcode.com/problems/car-fleet/)

**Complexity:** Time: $O(totalCars \log totalCars)$ | Space: $O(totalCars)$

**Key Intuition:** Sort cars by starting position and calculate their arrival times; a car becomes part of a fleet if its arrival time is less than or equal to the car ahead of it.

**Pseudocode:**
1. Combine position and speed into `carDataList` and sort by position in descending order (closest to target first).
2. For each car, calculate `timeToReachTarget = (target - position) / speed`.
3. Use a stack to store arrival times of fleet leaders.
4. If a current car's arrival time is greater than the top of the stack, it cannot catch up, so it becomes a new fleet leader.

**Edge Cases:**
- Cars starting at the same position (not allowed by constraints, but good to check).
- Cars with significantly different speeds but far apart.

**JavaScript Solution:**
```javascript
/**
 * @param {number} targetDistance
 * @param {number[]} positionList
 * @param {number[]} speedList
 * @return {number}
 */
const carFleet = (targetDistance, positionList, speedList) => {
    const carMetadata = positionList.map((positionValue, carIndex) => {
        return {
            position: positionValue,
            timeToArrival: (targetDistance - positionValue) / speedList[carIndex]
        };
    });

    // Sort cars by starting position: descending (closest to target first)
    carMetadata.sort((firstCar, secondCar) => secondCar.position - firstCar.position);

    const fleetArrivalTimesStack = [];

    for (const currentCar of carMetadata) {
        const currentArrivalTime = currentCar.timeToArrival;
        const fleetCount = fleetArrivalTimesStack.length;

        // If the stack is empty or this car takes longer than the fleet in front
        if (fleetCount === 0 || currentArrivalTime > fleetArrivalTimesStack[fleetCount - 1]) {
            fleetArrivalTimesStack.push(currentArrivalTime);
        }
        // Otherwise, this car catches up and joins the fleet in front (no action needed)
    }

    return fleetArrivalTimesStack.length;
};
```

---

## Validate Stack Sequences
**LeetCode Link:** [https://leetcode.com/problems/validate-stack-sequences/description/](https://leetcode.com/problems/validate-stack-sequences/description/)

**Complexity:** Time: $O(totalOperations)$ | Space: $O(totalOperations)$

**Key Intuition:** Simulate the process greedily: push elements into a stack and pop them immediately if the top of the stack matches the next required element in the popped sequence.

**Pseudocode:**
1. Initialize an empty `simulationStack` and a `poppedPointer` at 0.
2. For each `pushedElement` in the `pushedSequence`:
    - Push it onto the `simulationStack`.
    - While the stack is not empty and the top element matches `poppedSequence[poppedPointer]`:
        - Pop from the stack.
        - Increment the `poppedPointer`.
3. Return true if the stack is empty at the end.

**Edge Cases:**
- All elements pushed then all popped.
- One element pushed then popped immediately.
- Mismatched sequences.

**JavaScript Solution:**
```javascript
/**
 * @param {number[]} pushedSequence
 * @param {number[]} poppedSequence
 * @return {boolean}
 */
const validateStackSequences = (pushedSequence, poppedSequence) => {
    const simulationStack = [];
    let poppedPointer = 0;

    for (const pushedElement of pushedSequence) {
        simulationStack.push(pushedElement);

        // Greedily pop as long as the top of the stack matches the current popped element
