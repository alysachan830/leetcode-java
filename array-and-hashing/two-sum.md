## Topic
- Array and hashing

## Concept
- Loop through `nums`. In each loop, calculate the difference between `target` and `nums[i]`, set the result as the key in a hashmap
- If `nums[i]` can be retreived in the hashmap, it means that we find the pair

## Restrictions
- Only one valid answer exists.
- Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?

## Solution 1
- Time complexity: O(n)
- Space complexity: O(n)
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> pairs = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (pairs.get(nums[i]) != null) {
                return new int[]{i, pairs.get(nums[i])};
            }
            pairs.put(target - nums[i], i);
        }
        return new int[]{};
    }
}
```

## Solution 2
- Time complexity: O(n^2)
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i+1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i,j};
                }
            }
        }
        return new int[]{};
    }
}
```