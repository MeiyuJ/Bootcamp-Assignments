1. [Bus routes](https://leetcode.com/problems/bus-routes/description/)(Hard)

   ```java
   class Solution {
       public int numBusesToDestination(int[][] routes, int source, int target) {
           Map<Integer, List<Integer>> stopToBuses = new HashMap<>();
   
           for (int busId = 0; busId < routes.length; busId++) {
               for (int stop : routes[busId]) {
                   stopToBuses.computeIfAbsent(stop, k -> new ArrayList<>()).add(busId);
               }
           }
   
           if (!stopToBuses.containsKey(source) || !stopToBuses.containsKey(target)) {
               return -1;
           }
   
           if (source == target) {
               return 0;
           }
   
           Queue<Integer> queue = new LinkedList<>();
           Set<Integer> busesTaken = new HashSet<>();
           Set<Integer> stopsVisited = new HashSet<>();
           int res = 0;
   
           queue.offer(source);
   
           while (!queue.isEmpty()) {
               res++;
               int stopsToProcess = queue.size();
   
               for (int i = 0; i < stopsToProcess; i++) {
                   int currentStop = queue.poll();
   
                   for (int busId : stopToBuses.getOrDefault(currentStop, new ArrayList<>())) {
                       if (busesTaken.contains(busId)) {
                           continue;
                       }
   
                       busesTaken.add(busId);
   
                       for (int nextStop : routes[busId]) {
                           if (stopsVisited.contains(nextStop)) {
                               continue;
                           }
   
                           if (nextStop == target) {
                               return res;
                           }
   
                           queue.offer(nextStop);
                           stopsVisited.add(nextStop);
                       }
                   }
               }
           }
   
           return -1;
       }
   }
   ```

   

2. [Decode String](https://leetcode.com/problems/decode-string/description/)(Medium)

   ```java
   class Solution {
       public String decodeString(String s) {
           Stack<Integer>numStack=new Stack<>();
           Stack<StringBuilder>strBuild=new Stack<>();
           StringBuilder str = new StringBuilder();
           int num=0;
           for(char c:s.toCharArray()){
               if(c>='0' && c<='9'){
                   num=num*10 +c -'0';
               }
               else if(c=='['){
                   strBuild.push(str);
                   str=new StringBuilder();
                   numStack.push(num);
                   num=0;
               }else if(c==']'){
                   StringBuilder temp=str;
                   str=strBuild.pop();
                   int count=numStack.pop();
                   while(count-->0){
                       str.append(temp);
                   }
               }else{
                   str.append(c);
               }
           }
           return str.toString();
       }   
   }
   ```
   
   
   
3. [Number of people aware of a secret](https://leetcode.com/problems/number-of-people-aware-of-a-secret/description/)(Medium)

   ```java
   class Solution {
       public int peopleAwareOfSecret(int n, int delay, int forget) {
           long dp[] = new long[n+1];
           dp[1] = 1;
           long people = 0;
           long mod = (long)1e9+7;
           long ans = 0;
           
           for(int day=1;day<=n;day++){
               if(day>1){
                   dp[day] = (people+dp[Math.max(day-delay,0)]-dp[Math.max(day-forget,0)])%mod;
                   people = dp[day];
               }
               
               if(day>=n-forget+1){
                   ans = (ans + dp[day])%mod;
               }
           }
           
           int finalAns = (int)ans;
           if(finalAns<0) finalAns+=(int)mod;
           return finalAns;
       }
   }
   ```
