## Topic
- Two pointers

## Concept
- Convert `s` to an array. Set pointer `startPointer` at the start and `endPointer` at end of the array
- In each while loop, if we find that `s[startPointer]` or `s[endPointer]` is non-alphabet, non-numbers or empty string, skip that string
- If not, then we increment both `startPointer` and `endPointer`, for checking the next pair of strings in the next loop
- We keep looping this checking process, until we find: 
    - an unmatched pair of strings, we stop the `while` loop and return false
    - OR
    - We finish checking all strings in the array (i.e: `endPointer <= startPointer`)

## Solution 1
- Time complexity: O(n)
- Space complexity: O(n)

```java
    public static boolean isPalindrome(String s) {

        if (s.isBlank()) {
            return true;
        }
        String[] arr = s.split("");
        int endPointer = arr.length - 1;
        int startPointer = 0;
        while (endPointer > startPointer) {

            String leftS = arr[startPointer];
            String rightS = arr[endPointer];

            if (leftS.isBlank() || !leftS.matches("[a-zA-Z0-9]+")) {
                startPointer++;
                continue;
            }

            if (rightS.isBlank() || !rightS.matches("[a-zA-Z0-9]+")) {
                endPointer--;
                continue;
            }

            leftS = leftS.toLowerCase();
            rightS = rightS.toLowerCase();

            if (!leftS.equals(rightS)) {
                return false;
            }

            startPointer++;
            endPointer--;
        }

        return true;
    }
```