1. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)(Easy)

   ```java
   class Solution {
       public boolean containsDuplicate(int[] nums) {
           boolean result = false;
           HashMap<Integer, Integer> seen = new HashMap<>();
           for (int num: nums){
               if (seen.containsKey(num) && seen.get(num) >= 1){
                   result = true;
                   break;
               }
               seen.put(num, seen.getOrDefault(num, 0) + 1);
           }
           return result;
       }
   }
   // Time complexity: O(n), since we’re only traversing the array once.
   // Space complexity: O(1), since no extra space is used.
   ```

   

2. [Video Stitching](https://leetcode.com/problems/video-stitching/)(Medium)

   ```java
   class Solution {
       public int videoStitching(int[][] clips, int time) {
           // sort the clips by ending time descending
           Arrays.sort(clips, (a, b) -> {return b[1]-a[1];});
           boolean flag = true;
           int cur = 0;
           int result = 0;
           // greedily choose the latest ending clip that overlaps current time period
           while (cur < time){
               flag = true;
               for (int[] clip : clips){
                   if (clip[1] > cur && clip[0] <= cur){
                       result++;
                       cur = clip[1];
                       flag = false;
                       break;
                   }
               }
               // cannot find a clip, return error
               // case1: all clips searched but still cannot reach the time
               // case2: some time is not covered in all clips
               if (flag){
                   return -1;                
               }
           }
           return result;
       }
   }
   // Time complexity: O(nlogn) for sorting, worst case O(n^2) for the search part.
   // Space complexity: O(1), since no extra space is used.
   ```
   
   
   
3. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)(Medium)

   ```java
   class Solution {
       public int maxArea(int[] height) {
           int i = 0, j = height.length-1;
           int result = 0;
           while (i < j){
               result = Math.max(result, Math.min(height[i], height[j]) * (j-i));
               if (height[i] < height[j]){
                   i++;
               }else if (height[i] > height[j]){
                   j--;
               }else{
                   i++;
                   j--;
               }  
           }
           return result;
       }
   }
   // Time complexity: O(n), since we’re only traversing the array once with two pointers.
   // Space complexity: O(1), since no extra space is used.
   ```

   