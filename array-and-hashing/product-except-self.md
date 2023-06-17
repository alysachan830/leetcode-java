## Topic
- Array and hashing

## Concept
- To calculate products for all number except `nums[i]`, each `num` in `nums` are be used for more than once. For example, the calculation of `[1,2,3,4]` is `[2*3*4,1*3*4,1*2*4,1*2*3]`.
- First, we begin with looping `nums` from start to end. We store each products in the `result` array, starting from the second index. 
The result is `[ ,1,1*2,1*2*3]`
- Then, we loop `nums` from end to the start. We begin with the second-last of `result`, which is `[2*3*4,3*4,3, ]`
- In each iteration, we retrieve the existing products in `result`, calculate it with the products we acculated from the end of `num`, so the `result` is `[2*3*4,1*3*4,1*2*4,1*2*3]`
- Be aware that `result[0]` is empty after step 1. `result[0]` should be the product accumulated from `result[nums.length - 1]` to `result[1]`

## Restrictions
- You must write an algorithm that runs in O(n) time and without using the division operation
- Follow up: Can you solve the problem in O(1) extra space complexity? (The output array does not count as extra space for space complexity analysis.)


## Solution 1
- Time complexity: O(n)
- Space complexity: O(1)
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] result = new int[nums.length];
        if (nums.length == 2) {
            result[0] = nums[1];
            result[1] = nums[0];
            return result;
        }
        int productsAccumulatedFromEnd = nums[nums.length - 1];
        int productsAccumulatedFromStart = nums[0];
        for (int i = 1; i < nums.length; i++) {
            result[i] = productsAccumulatedFromStart;
            productsAccumulatedFromStart *= nums[i];
        }
        for (int i = nums.length - 2; i > 0; i--) {
            result[i] *= productsAccumulatedFromEnd;
            productsAccumulatedFromEnd *= nums[i];
        }
        result[0] = productsAccumulatedFromEnd;
        return result;
    }
}
```

## Solution 2

- Time complexity: O(n)
- Space complexity: O(1)
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] result = new int[nums.length];
        if (nums.length == 2) {
            result[0] = nums[1];
            result[1] = nums[0];
            return result;
        }
        int productsAccumulatedFromStart = nums[0];
        int productsAccumulatedFromEnd = nums[nums.length - 1];
        for(int i = 1; i < nums.length - 1; i++) {
            int rightIndex = nums.length - 1 - i;

            if (i > rightIndex) {
                result[i] = result[i] * productsAccumulatedFromStart;
                result[rightIndex] = result[rightIndex] * productsAccumulatedFromEnd;

            } else if (i == rightIndex) {
                result[i] = productsAccumulatedFromStart;
                result[rightIndex] = result[rightIndex] * productsAccumulatedFromEnd;
            } else {
                result[i] = productsAccumulatedFromStart;
                result[rightIndex] = productsAccumulatedFromEnd;
            }
            productsAccumulatedFromStart = productsAccumulatedFromStart * nums[i];
            productsAccumulatedFromEnd = productsAccumulatedFromEnd * nums[nums.length - 1 - i];
        }
        result[0] = productsAccumulatedFromEnd;
        result[nums.length - 1] = productsAccumulatedFromStart;
        return result;
    }
}
```