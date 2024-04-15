1. [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)(Medium)

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   class Solution {
       public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
           if (root == null) return root;
   
           TreeNode left = lowestCommonAncestor(root.left, p, q);
           TreeNode right = lowestCommonAncestor(root.right, p, q);
   
           // case1: find p and q separately in left subtree and right subtree, common ancestor is current node
           if (left != null && right != null){
               return root;
           }
   
           // case2: find current node is p or q, return current node
           if (root == p || root == q){
               return root;
           }
   
           // case3: one of the left and right is not null, which means that node is already the common ancestor
           //or only find one node is find, another one is not on this tree, need to go up to a bigger tree
           if (left != null){
               return left;
           }
           if (right != null){
               return right;
           }
   
           // case4: both p q are not on this tree, need to go up to a bigger tree
           return null;
       }
   }
   // Time complexity: O(n), need to traverse all the nodes in tree in worst case.
   // Space complexity: O(n), in the worst case, the recursion depth reaches n, and the system uses an additional space of size O(n)
   ```

   

2. [Word Break](https://leetcode.com/problems/word-break/description/)(Medium)

   ```java
   class Solution {
       public boolean wordBreak(String s, List<String> wordDict) {
           Set<String> wordDictSet = new HashSet(wordDict);
           boolean dp[] = new boolean[s.length() + 1];
           dp[0] = true;
           // dp[i] represent if the first i chars s[0..i-1] can be word break
           // dp[s.length()] is the final answer
           for (int i = 1; i <= s.length(); i++){
               for (int j = 0; j < i; j++){
                   if (dp[j] && wordDictSet.contains(s.substring(j, i))){
                       dp[i] = true;
                       break;
                   }
               }
           }
           return dp[s.length()];
       }
   }
   // Time complexity: O(n^2), we have n dp states, need to compute at most n segmentation for every state.
   // Space complexity: O(n), we need O(n) for both dp table and HashSet
   ```

   

3. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)(Medium)

   ```java
   class Solution {
       public int partition(int[] nums, int s, int t){
           int i = s, j = t;
           int temp = nums[i];
           while (i < j){
               while (j > i && nums[j] >= temp){
                   j--;
               }
               nums[i] = nums[j];
               while (i < j && nums[i] <= temp){
                   i++;
               }
               nums[j] = nums[i];
           }
           nums[i] = temp;
           return i;
       }
   
       public int func(int[] nums, int k){
           int l = 0;
           int r = nums.length -1;
           
           while(true){
               int p = partition(nums, l, r);
               
               if(p == nums.length-k) return nums[p];
               if(p < nums.length - k){
                   l = p + 1;
               }else{
                   r = p - 1;
               }
           }
       }
       public int findKthLargest(int[] nums, int k) {
           return func(nums, k);
       }
   }
   // Time complexity: O(n), prove can be found at Introduction to Algorithm Chapter 9.2
   // Space complexity: O(logn), average recursion depth logn
   ```

   