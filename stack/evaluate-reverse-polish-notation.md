## Topic
- Stack

## Concept
- In stack, the last added item is the first item that we can retreive
- For the question, in each loop, we need to calculate with:
    - the latest result we calculated in the previous loop
    - the latest number we skipped in the previous loop
- Hence, we store those results / numbers in a stack and retreive value from it for calculation in each loop


## Solution
- Time complexity: O(n)
- Space complexity: O(n)
```java
package stacks;
import java.util.*;

class Solution {
    public static int evalRPN(String[] tokens) {
        if (tokens.length == 1) {
            return Integer.parseInt(tokens[0]);
        }
        Stack<Integer> valuesToBeCalculated = new Stack<>();
        for (String token: tokens) {
            int r;
            int l;
            int result;
            switch (token) {
                case "+" -> result = valuesToBeCalculated.pop() + valuesToBeCalculated.pop();
                case "-" -> {
                    r = valuesToBeCalculated.pop();
                    l = valuesToBeCalculated.pop();
                    result = l - r;
                }
                case "*" -> result = valuesToBeCalculated.pop() * valuesToBeCalculated.pop();
                case "/" -> {
                    r = valuesToBeCalculated.pop();
                    l = valuesToBeCalculated.pop();
                    result = l / r;
                }
                default -> result = Integer.parseInt(token);
            }
            valuesToBeCalculated.push(result);
        }
        return valuesToBeCalculated.pop();
    }v
```