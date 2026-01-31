# Technical Interview Guide: The Two-Pointer Pattern
**Engineer's Note:** This file covers the "Squeeze" or "Converging Pointers" technique. This is the gold standard for optimizing space complexity from $O(n)$ down to $O(1)$ when dealing with sorted data or boundary-based problems.

---

## 1. Valid Palindrome
**Problem:** [LeetCode 125](https://leetcode.com/problems/valid-palindrome/)

### The Logic
Instead of wasting $O(n)$ space by creating a reversed string, we use two pointers: one at the very beginning and one at the end. We move them toward each other, skipping characters that aren't letters or numbers. If the characters at the pointers ever mismatch (ignoring case), it's not a palindrome.



### Solution
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    let left = 0;
    let right = s.length - 1;
    
    while (left < right) {
        // Skip non-alphanumeric characters from the left
        while (left < right && !/[a-zA-Z0-9]/.test(s[left])) {
            left++;
        }
        // Skip non-alphanumeric characters from the right
        while (left < right && !/[a-zA-Z0-9]/.test(s[right])) {
            right--;
        }
        
        if (s[left].toLowerCase() !== s[right].toLowerCase()) {
            return false;
        }
        
        left++;
        right--;
    }
    
    return true;
};

