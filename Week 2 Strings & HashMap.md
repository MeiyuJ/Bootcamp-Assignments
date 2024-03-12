1. [String to integer atoi](https://leetcode.com/problems/string-to-integer-atoi/description/)(Medium)

   ```java
   class Solution {
       public int myAtoi(String s) {
           int result = 0;
           boolean positive = true;
           int i = 0;
           // trim the leading possible spaces
           while (i < s.length() && s.charAt(i) == ' '){ 
               i++;
           }
           
           // check possible sign, default positive
           if (i < s.length() && s.charAt(i) == '+'){
               positive = true;
               i++;
           }else if (i < s.length() && s.charAt(i) == '-'){
               positive = false;
               i++;            
           }
   
           // read all the digits
           while (i < s.length() && Character.isDigit(s.charAt(i))){
               int digit = s.charAt(i) - '0';
               // detect if the number is out of bound
               if (result > (Integer.MAX_VALUE - digit) / 10){
                   return positive ? Integer.MAX_VALUE : Integer.MIN_VALUE;
               }
               result = result * 10 + digit;
               i++;
           }
           return positive? result : -result; //return the number based on its sign
       }
   }
   // Time complexity: O(n), since we only read the string once.
   // Space complexity: O(1), since no extra space is used.
   ```

   

2. [Longest repeating character replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/)(Medium)

   ```java
   class Solution {
       public int characterReplacement(String s, int k) {
           int result = 0;
           int mostLetterFreq = 0;
           int[] freq = new int[26];
           int left = 0, right = 0;
           for (; right < s.length(); right++){
               // detect the substring ending at right and has s[right] as the most freq letter 
               freq[s.charAt(right) - 'A']++;
               mostLetterFreq = Math.max(mostLetterFreq, freq[s.charAt(right) - 'A']);
   
               // (right-left+1-mostLetterFreq) is the number of letters to change, it should be less or equal to k
               while (right-left+1 - mostLetterFreq > k){
                   freq[s.charAt(left) - 'A']--;
                   left++;
               }
               result = Math.max(result, right-left+1);
           }
           return result;
       }
   }
   // Time complexity: average O(n), since both the right and left pointer only traversing the array once.
   // Space complexity: O(1), since we only use a constant size array freq.
   ```

   

3. [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/description/)(Hard)

   ```java
   class Solution {
       private boolean isPalindrome(String s, int i, int j){
       // check if s[i:j] (i,j included) is a palindrome
       // time complexity average O(k) where k is the average length of words
           while (i < j){
               if (s.charAt(i) != s.charAt(j)){
                   return false;
               }
               i++; j--;
           }
           return true;
       }
   
       public List<List<Integer>> palindromePairs(String[] words) {
           Map<String, Integer> mymap = new HashMap<>();
           List<List<Integer>> result = new ArrayList<>();
   
           for (int i = 0; i < words.length; i++){
               mymap.put(words[i], i);
           }
   
           for (int i = 0; i < words.length; i++){
               // if a word is blank "", then ("", palindrome) and (palindrome, "") are two palindrome pairs
               if (words[i].equals("")){
                   for (int j = 0; j < words.length; j++){
                       if (isPalindrome(words[j], 0, words[j].length()-1) && j != i){
                           result.add(List.of(i, j));
                           result.add(List.of(j, i));
                       }
                   }
                   continue;
               }
   
               // Build the reverseword of a word i
               StringBuilder sb = new StringBuilder(words[i]);
               String reverseword = sb.reverse().toString();
   
               // If word j equals the reverseword of i, the (i, j) is a palindrome pair
               // (j, i) will be counted when we iterate on word j
               if (mymap.containsKey(reverseword)){
                   int j = mymap.get(reverseword);
                   if (j != i){
                       result.add(List.of(i, j));
                   }
               }
               
               // if the suffix of word i (prefix of reverseword) is a palindrome, it could be paired with (i, j) if j == prefix of i
               // if the prefix of word i (suffix of reverseword) is a palindrome, it could be paired with (j, i) if j == suffix of i
               for (int j = 1; j < reverseword.length(); j++){
                   if (isPalindrome(reverseword, 0, j-1)){
                       String subreverse = reverseword.substring(j, reverseword.length());
                       if (mymap.containsKey(subreverse)){
                           result.add(List.of(i, mymap.get(subreverse)));
                       }
                   }
                   if (isPalindrome(reverseword, j, reverseword.length()-1)){
                       String subreverse = reverseword.substring(0, j);
                       if (mymap.containsKey(subreverse)){
                           result.add(List.of(mymap.get(subreverse), i));
                       }
                   }
               }
           }
           return result;
       }
   }
   // Time complexity: average O(n * k^2), since we iterate through the words (O(n)), scan every possible prefix and suffix (O(k) times) and check if it is palindrome(O(k)).
   // where k is the average length of the words
   // Space complexity: O(n), since we only use a hashmap to store the words.
   ```

   