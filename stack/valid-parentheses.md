## Topic
- Stack

## Concept
- Immediately return false if:
    - the first character is not an open bracket 
    - length of `s` is not an even number
- Create a stack to store open brackets. If `s.charAt(i)` is a close bracket, pop the item in the stack and check if it matches with the close bracket

## Solution
- Time complexity: O(n)
- Space complexity: O(n)

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 2 != 0) {
            return false;
        }
        Stack<Character> openBrackets = new Stack<>();
        HashMap<Character, Character> pairs = new HashMap<>();
        pairs.put('(', ')');
        pairs.put('[', ']');
        pairs.put('{', '}');

        // The first character cannot be a close bracket
        if (pairs.containsValue(s.charAt(0))) {
            return false;
        }

        for (int i = 0; i < s.length(); i++) {
            if (pairs.containsKey(s.charAt(i))) {
                openBrackets.push(s.charAt(i));
                continue;
            }

            // For the last loop, it should have 1 item left in openBrackets
            if (i == s.length() - 1 && openBrackets.empty()) {
                return false;
            }

            if (!openBrackets.empty()) {
                char lastInputOpenBracket = openBrackets.pop();
                char targetCloseBracket = pairs.get(lastInputOpenBracket);
                if (s.charAt(i) != targetCloseBracket) {
                    return false;
                }
            }
        }
        return openBrackets.empty();
    }
}
```