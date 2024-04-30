1. [Partition equal subset sum](https://leetcode.com/problems/partition-equal-subset-sum/)(Medium)

   ```java
   class Solution {
       public boolean canPartition(int[] nums) {
           int n = nums.length;
           if (n < 2) {
               return false;
           }
           int sum = 0, maxNum = 0;
           for (int num : nums) {
               sum += num;
               maxNum = Math.max(maxNum, num);
           }
           if (sum % 2 != 0) {
               return false;
           }
           int target = sum / 2;
           if (maxNum > target) {
               return false;
           }
           boolean[] dp = new boolean[target + 1];
           dp[0] = true;
           for (int i = 0; i < n; i++) {
               int num = nums[i];
               for (int j = target; j >= num; --j) {
                   dp[j] |= dp[j - num];
               }
           }
           return dp[target];
       }
   }
   // Time complexity: O(n×target), need to calculate all the state in the dp array.
   // Space complexity: O(target), target equals to half of the sum of the array elements.
   ```

   

2. [Coin Change](https://leetcode.com/problems/coin-change/)(Medium)

   ```java
   public class Solution {
       public int coinChange(int[] coins, int amount) {
           int max = amount + 1;
           int[] dp = new int[amount + 1];
           Arrays.fill(dp, max);
           dp[0] = 0;
           for (int i = 1; i <= amount; i++) {
               for (int j = 0; j < coins.length; j++) {
                   if (coins[j] <= i) {
                       dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                   }
               }
           }
           return dp[amount] > amount ? -1 : dp[amount];
       }
   }
   // Time complexity: O(Sn), S is amount, n is the coins[] size. We need to calculate n different situation for S states.
   // Space complexity: O(S), the size of the dp array.
   ```

   

3. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray)(Medium)

   ```java
   class Solution {
       public int maxSubArray(int[] nums) {
           int result = nums[0];
           int currentmax = 0;
           for (int n : nums){
               currentmax = Math.max(currentmax + n, n);
               result = Math.max(currentmax, result);
           }
           return result;
       }
   }
   // Time complexity: O(n), we need to traverse the array once.
   // Space complexity: O(1), no extra space needed.
   ```

   