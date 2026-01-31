## Valid Parentheses
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Use a stack to track open brackets.
2. Use a map to define matching pairs.
3. Iterate through the string: push open brackets, for closing brackets check if the stack top matches.
4. Return true if stack is empty at the end.

**JavaScript Solution:**
```javascript
const isValid = (s) => {
    const stack = [];
    const map = { ')': '(', '}': '{', ']': '[' };

    for (const char of s) {
        if (map[char]) {
            // If it's a closing bracket, check for matching open bracket
            if (stack.pop() !== map[char]) return false;
        } else {
            // If it's an opening bracket, push to stack
            stack.push(char);
        }
    }
    return stack.length === 0;
};
```

## Min Stack
**Complexity:** Time: $O(1)$ for all operations | Space: $O(n)$

**Logic (Pseudocode):**
1. Maintain two stacks: one for data and one for the current minimums.
2. When pushing, calculate the current minimum between the new value and the existing min.

**JavaScript Solution:**
```javascript
class MinStack {
    constructor() {
        this.stack = [];
        this.minStack = [];
    }

    push(val) {
        this.stack.push(val);
        // Current min is either the new value or the existing top of minStack
        const currentMin = this.minStack.length === 0 
            ? val 
            : Math.min(val, this.minStack[this.minStack.length - 1]);
        this.minStack.push(currentMin);
    }

    pop() {
        this.stack.pop();
        this.minStack.pop();
    }

    top() {
        return this.stack[this.stack.length - 1];
    }

    getMin() {
        return this.minStack[this.minStack.length - 1];
    }
}
```

## Evaluate Reverse Polish Notation
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Use a stack to store operands.
2. When an operator is encountered, pop the top two values, apply the operator, and push the result back.

**JavaScript Solution:**
```javascript
const evalRPN = (tokens) => {
    const stack = [];
    const operations = {
        '+': (a, b) => a + b,
        '-': (a, b) => a - b,
        '*': (a, b) => a * b,
        '/': (a, b) => Math.trunc(a / b)
    };

    for (const token of tokens) {
        if (operations[token]) {
            const b = stack.pop();
            const a = stack.pop();
            stack.push(operations[token](a, b));
        } else {
            stack.push(Number(token));
        }
    }
    return stack[0];
};
```

## Generate Parentheses
**Complexity:** Time: $O(\frac{4^n}{\sqrt{n}})$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Use backtracking (recursion) which implicitly uses a call stack.
2. Add an opening bracket if `openCount < n`.
3. Add a closing bracket if `closedCount < openCount`.

**JavaScript Solution:**
```javascript
const generateParenthesis = (n) => {
    const res = [];
    const stack = []; // Used as a buffer for the current string

    const backtrack = (openN, closedN) => {
        if (openN === n && closedN === n) {
            res.push(stack.join(''));
            return;
        }

        if (openN < n) {
            stack.push("(");
            backtrack(openN + 1, closedN);
            stack.pop(); // Clean up for next recursive path
        }

        if (closedN < openN) {
            stack.push(")");
            backtrack(openN, closedN + 1);
            stack.pop();
        }
    };

    backtrack(0, 0);
    return res;
};
```

## Daily Temperatures
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Use a monotonic decreasing stack to store indices.
2. If current temp > temp at stack top, pop and calculate the day difference.

**JavaScript Solution:**
```javascript
const dailyTemperatures = (temperatures) => {
    const res = new Array(temperatures.length).fill(0);
    const stack = []; // stores [temp, index]

    for (let i = 0; i < temperatures.length; i++) {
        const t = temperatures[i];
        // While current temp is warmer than the temperature at the stack top
        while (stack.length > 0 && t > stack[stack.length - 1][0]) {
            const [stackT, stackIdx] = stack.pop();
            res[stackIdx] = i - stackIdx;
        }
        stack.push([t, i]);
    }
    return res;
};
```

## Car Fleet
**Complexity:** Time: $O(n \log n)$ due to sorting | Space: $O(n)$

**Logic (Pseudocode):**
1. Sort cars by position (descending).
2. Calculate time to reach target for each car.
3. Use a stack to track fleet times; if a car's time is $\le$ the car in front, it joins the fleet (no push).

**JavaScript Solution:**
```javascript
const carFleet = (target, position, speed) => {
    const cars = position.map((p, i) => [p, speed[i]]);
    cars.sort((a, b) => b[0] - a[0]); // Sort by position descending

    const stack = [];
    for (const [p, s] of cars) {
        const time = (target - p) / s;
        stack.push(time);
        // If current car arrives faster than the fleet in front, it becomes part of it
        if (stack.length >= 2 && stack[stack.length - 1] <= stack[stack.length - 2]) {
            stack.pop();
        }
    }
    return stack.length;
};
```

## Validate Stack Sequences
**Complexity:** Time: $O(n)$ | Space: $O(n)$

**Logic (Pseudocode):**
1. Simulate the push/pop operations using a stack.
2. For each element pushed, check if it matches the current element in the `popped` array.
3. If it matches, pop it immediately.

**JavaScript Solution:**
```javascript
const validateStackSequences = (pushed, popped) => {
    const stack = [];
    let i = 0; // Pointer for popped array

    for (const x of pushed) {
        stack.push(x);
        // Greedily pop as long as the stack top matches the next popped element
        while (stack.length > 0 && stack[stack.length - 1] === popped[i]) {
            stack.pop();
            i++;
        }
    }
    return stack.length === 0;
};
```
