## Topic
- Two pointers

## Concept
- Let the triplets be `x`, `y`, `z` and `x + y + z = 0`. If we fix one of them, such as `x`, we only have to find 2 sum of `y` and `z`. If 2 sum + `x` = 0, we find the triplets
- To implement this concept, first, we need to sort the array. The benefit will be shown later
- Then, we can create a for loop to loop through `nums`, which means `nums[i]` is fixed in each loop
- Since we need to find the 2 sum of `y` and `z`, from `nums[i+1]` to the end of `nums`, we create a while loop nested inside the for loop we created before.
- In the nested loop, we set a pointer `l`, on `nums[i+1]` and a pointer `r` at the end of `nums`. Then we create a while loop to find the 2 sum, in order to find `nums[i] + nums[l] + nums[r] = 0`.
- Since the array is sorted, we can assume `nums[l] <= nums[r]`. If: 
1. `nums[l] + nums[r]` + `a fixed number` > 0
    - We should reduce the value of `nums[l] + nums[r]`, by moving `r` a step forward to the start of `nums`
2. `nums[l] + nums[r]` + `a fixed number` < 0
    - We should increase the value of `nums[l] + nums[r]`, by moving `l` a step forward to the end of `nums`
3. `nums[l] + nums[r]` + `a fixed number` = 0
    - We find the triplets
- When `l` and `r` become the same value, that means we have searched all the items, from `nums[i+1]` to the end of `nums`. Now the while loop should be finished and the outer for loop moves to the next item
- Noted that we should avoid duplicate triplets, e.g:
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```
- To skip duplicate triplets, we should skip duplicate nummber in our sorted array. Example 1
```
Input nums = [-1,0,1,2,-1,-4]
Sorted nums = [-4,-1,-1,0,1,2]
Expected result = [[-1,0,1], [-1,-1,2]]

The array has two -1 in the array. In the outer for loop, we skip the second -1
```
Example 2
```
Input nums = [-2,0,0,2,2]
Sorted nums = [-2,0,0,2,2]
Expected result = [[0,2,-2]]

In the first round of for loop, we can find the triplets [-2,0,2]:

[-2,0,0,2,2]
 ^--^-----^

After that, because the while loop is not finished yet, so it moves l and r:

[-2,0,0,2,2]
 ^----^-^--

Because we looped from 0 to 2 before, we should skip these 2 numbers. To achieve this concept, we can add another while loop to move l, 

if:
nums[l] == nums[l-1] && l < nums.length

and also move r, if:
nums[r] == nums[r+1] && r < nums.length

Hence, l and r are not pointing to the number we have looped before
```

## Solution 1 (Two pointer solution)
- Time complexity: O(nlogn) + O(n^2) = O(n^2)
    - Arrays.sort(nums) = O(nlogn)
    - A for loop with a nested for loop = O(n^2)
    - Note: while loop inside the nested for loop is counted, because it is only moving the pointer, for reducing the range of number we need to loop in the nested for loop
- Space complexity: O(1)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 0; i < nums.length - 2; i++) {
            // Skip duplicate number
            if (i > 0 && nums[i-1] == nums[i]) {
                continue;
            }
            int l = i + 1;
            int r = nums.length - 1;
            while (l < r) {
                int sum = nums[l] + nums[r] + nums[i];
                if (sum == 0) {
                    result.add(List.of(nums[l], nums[r], nums[i]));
                    // Don't add break here to stop the while loop,
                    // because there are still other possible pairs
                    // For example: [-4,-1,-1,0,1,2]
                    // If we stop after finding [-1,-1,2], we will miss [-1,0,1]
                    l++;
                    r--;
                    // Skip duplicate number
                    // For example: [-2,0,0,2,2]
                    while (nums[l] == nums[l - 1] && l < r) {
                        l++;
                    }
                    while (nums[r] == nums[r + 1] && l < r) {
                        r--;
                    }
                    continue;
                }
                if (sum > 0) {
                    r--;
                } else {
                    l++;
                }
            }
        }
        return result;
    }
}
```

## Note
- Brute force solution will cause time limit exceeded problem. Because brute force needs `O(n^3)` time complexity with 3 for loops
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < nums.length; j++) {
                int target = 0 - (nums[i] + nums[j]);
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                if (target > nums[nums.length - 1] || target < nums[0]) {
                    continue;
                }
                for (int k = j + 1; k < nums.length; k++) {
                    if (k > j + 1 && nums[k] == nums[k - 1]) {
                        continue;
                    }
                    if (nums[k] == target) {
                        List<Integer> pair = new ArrayList<>(
                                List.of(nums[i], nums[j], nums[k])
                        );
                        result.add(pair);
                        continue;
                    }
                }
            }
        }
        return result;
    }
}
```