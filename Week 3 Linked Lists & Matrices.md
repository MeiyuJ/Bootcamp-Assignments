1. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/description/)(Easy)

   ```java
   class Solution {
       public ListNode middleNode(ListNode head) {
           ListNode slow = head, fast = head;
           while (fast != null && fast.next != null) {
               slow = slow.next;
               fast = fast.next.next;
           }
           return slow;
       }
   }
   // Time complexity: O(n), since we only read the string once.
   // Space complexity: O(1), since no extra space is used.
   ```
   
   
   
2. [Reverse nodes in k-group](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)(Hard)

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode() {}
    *     ListNode(int val) { this.val = val; }
    *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
    * }
    */
   class Solution {
       public ListNode reverseKGroup(ListNode head, int k) {
   
           ListNode preTail = null;        // stores the tail node of previous LL.
           ListNode curHead = head;        // stores the head node of current LL
           ListNode curTail = head;        // stores the tail node of current LL
   
           ListNode nextHead = null;       // stores the head node of next LL
   
           while(curHead != null)
           {
               // initialize count from 1
               int count = 1;
   
               // iterate the LL untill count becomes k or we reach the last node.
               while(curTail.next != null && count < k)
               {
                   curTail = curTail.next;
                   count++;
               }
   
               if(count != k)
               {
                   break;
               }
               
               // set the nextHead pointer to the head of the next LL.
               nextHead = curTail.next;
   
   
               // detach the RHS of the current LL.
               curTail.next = null;
   
               // detach the LHS of the current LL.
               if(preTail != null)
               {
                   preTail.next = null;
               }
   
               // after reversing the current LL, curHead becomes the new tail.
               // and curTail becomes the new head.
   
               curTail = reverse(curHead);
   
               // attach the LHS
               if(preTail != null)
               {
                   preTail.next = curTail;
               }
               else
               {
                   // if preTail is null then we have reversed the first LL
                   // so store the reference of curHead in original head pointer.
                   head = curTail;
               }
   
               // attach the RHS
               curHead.next = nextHead;
               
               // update the pointer
               preTail = curHead;
               curHead = nextHead;
               curTail = nextHead;
           }
   
           return head;
           
       }
       private ListNode reverse(ListNode head) {
           ListNode preNode = null;
           ListNode curNode = head;
   
           ListNode nextNode = head;
   
           while(curNode != null)
           {
               nextNode = nextNode.next;
               
               curNode.next = preNode;
   
               preNode = curNode;
               curNode = nextNode;
           }
           return preNode;
       }
   }
   // Time complexity: average O(n).
   // Space complexity: O(1).
   ```

   

3. [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/description/)(Medium)

   ```java
   class Solution {
       public boolean isValidSudoku(char[][] board) {
           Set seen = new HashSet();
           for (int i = 0; i < 9; i++){
               for (int j = 0; j < 9; j++){
                   if (board[i][j] != '.'){
                       char c = board[i][j];
                       if (!seen.add(c + " in row " + i) ||
                       !seen.add(c + " in col " + j) ||
                       !seen.add(c + "in block " + i/3 + " " + j/3)){
                           return false;
                       }
                   }
               }
           }
           return true;
       }
   }
   // Time complexity: O(n^2)?.
   // Space complexity: O(n^2)?.
   ```
