1. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)(Medium)

   ```java
   class Solution {
       public List<Integer> rightSideView(TreeNode root) {
           Map<Integer, Integer> rightmostValueAtDepth = new HashMap<Integer, Integer>();
           int max_depth = -1;
   
           Queue<TreeNode> nodeQueue = new LinkedList<TreeNode>();
           Queue<Integer> depthQueue = new LinkedList<Integer>();
           nodeQueue.add(root);
           depthQueue.add(0);
   
           while (!nodeQueue.isEmpty()) {
               TreeNode node = nodeQueue.remove();
               int depth = depthQueue.remove();
   
               if (node != null) {
                   max_depth = Math.max(max_depth, depth);
   
                   rightmostValueAtDepth.put(depth, node.val);
   
                   nodeQueue.add(node.left);
                   nodeQueue.add(node.right);
                   depthQueue.add(depth + 1);
                   depthQueue.add(depth + 1);
               }
           }
   
           List<Integer> rightView = new ArrayList<Integer>();
           for (int depth = 0; depth <= max_depth; depth++) {
               rightView.add(rightmostValueAtDepth.get(depth));
           }
   
           return rightView;
       }
   }
   // Time complexity: O(n), need to traverse all the nodes in tree.
   // Space complexity: O(n), every node enqueue once, so the queue is at most O(n) size.
   ```

   

2. [Longest increasing path in a matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/description/)(Hard)

   ```java
   class Solution {
       public int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
       public int rows, columns;
   
       public int longestIncreasingPath(int[][] matrix) {
           if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
               return 0;
           }
           rows = matrix.length;
           columns = matrix[0].length;
           int[][] memo = new int[rows][columns];
           int ans = 0;
           for (int i = 0; i < rows; ++i) {
               for (int j = 0; j < columns; ++j) {
                   ans = Math.max(ans, dfs(matrix, i, j, memo));
               }
           }
           return ans;
       }
   
       public int dfs(int[][] matrix, int row, int column, int[][] memo) {
           if (memo[row][column] != 0) {
               return memo[row][column];
           }
           ++memo[row][column];
           for (int[] dir : dirs) {
               int newRow = row + dir[0], newColumn = column + dir[1];
               if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns && matrix[newRow][newColumn] > matrix[row][column]) {
                   memo[row][column] = Math.max(memo[row][column], dfs(matrix, newRow, newColumn, memo) + 1);
               }
           }
           return memo[row][column];
       }
   }
   // Time complexity: O(mn), the size of the matrix.
   // Space complexity: O(mn), recursion depth
   ```

   

3. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)(Medium)

   ```java
   class Solution {
       public int kthSmallest(TreeNode root, int k) {
           Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
           while (root != null || !stack.isEmpty()) {
               while (root != null) {
                   stack.push(root);
                   root = root.left;
               }
               root = stack.pop();
               --k;
               if (k == 0) {
                   break;
               }
               root = root.right;
           }
           return root.val;
       }
   }
   // Time complexity: O(H+k), worst case traverse all the tree
   // Space complexity: O(H), store at most H elements in the stack
   ```

   