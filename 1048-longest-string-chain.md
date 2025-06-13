## 1048 - Longest String Chain

> Difficulty:  Medium

## 🧠 Intuition

If we observe carefully, in any valid word chain, each word is formed by adding a single character to the previous word. This means that for two consecutive words in the chain, the shorter word is a **subsequence** of the longer one, and its length is exactly one less.

Given that the maximum possible length of any word is 16 (as per the constraints), we can take advantage of this by **grouping words based on their lengths**. Then, instead of checking every possible pair, we can try to build the chain by moving from longer words to shorter ones — specifically looking for words that are one character shorter and are valid subsequences.

To keep track of already computed results and avoid recalculating chains for the same words, we use **memoization with a 2D DP table**. Here, `dp[length][index]` represents the longest chain starting from the word at position `index` in the list of words of a specific `length`.  
We use two dimensions because:
- One axis (`length`) helps us track chains based on word size.
- The other axis (`index`) distinguishes between different words of the same length.

This structure allows us to efficiently look up and store results for overlapping subproblems, reducing the time complexity significantly.



## 📝 Approach

1. **Group Words by Length**:  
   Since we know the word length ranges from 1 to 16, we create a list of vectors where each vector contains words of a particular length.

2. **Dynamic Programming (DP)**:  
   We use memoization to avoid recomputation. The state `dp[start][idx]` stores the longest chain starting from the `idx`-th word in the list of words of length `start`.

3. **Recursive Traversal**:  
   For each word, we look at all words that are one character shorter and check if any of them is a valid subsequence. If so, we try to extend the chain using that word.

4. **Subsequence Check**:  
   We define a helper function `lis(t, s)` that returns true if `s` is a subsequence of `t` and `t.length == s.length + 1`.

5. **Compute the Maximum Chain**:  
   Finally, we check all possible starting points and return the maximum chain length we can get.


## 💻 Code (C++)

```cpp
class Solution {
public:
    int dp[20][1010];
    bool lis(string& t,string& s){
        if (s.size() + 1 != t.size()) return false;
        int i = 0, j = 0;
        while (i < t.size() && j < s.size()) {
            if (t[i] == s[j]) {
                i++; j++;
            } else {
                i++;
            }
        }
        return j == s.size();
    }
    int rec(int start,int idx,vector<vector<string>>& nums){
        if(start <= 0) return 0;
        if(dp[start][idx] != -1) return dp[start][idx];
        int res = 1;
        string curr = nums[start][idx];
        for(int i=0;i<nums[start - 1].size();i++){
            if(lis(curr,nums[start-1][i])){
                res = max(res,1+rec(start-1,i,nums));
            }
        }
        return dp[start][idx] = res;
    }
    int longestStrChain(vector<string>& words) {
        vector<vector<string>> nums(17);
        for(auto& word : words){
            nums[word.size()].push_back(word);
        }
        memset(dp,-1,sizeof(dp));
        int ans = 0;
        for(int i=1;i<17;i++){
            for(int j=0;j<nums[i].size();j++){
                ans = max(ans,rec(i,j,nums));
            }
        }
        return ans;
    }
};
```
## 🧮 Complexity Analysis

| Complexity | Value        |
|------------|--------------|
| 🕒 Time     | O(n² · ℓ)     |
| 💾 Space    | O(n)         |

> Explanation:
> - Time: We compare each word with words that are one character shorter. In the worst case, this leads to O(n²) comparisons, and each subsequence check takes O(ℓ) time where ℓ ≤ 16 (max word length). Hence, overall time complexity is O(n² · ℓ).
> - Space: We use O(n) space to store words grouped by length and for the 2D DP table. The recursion stack uses O(ℓ) = O(1) space since the max depth is 16.

## 🔁 Alternative Solutions

### 🔹 Approach: Bottom-Up DP with Hash Map (Predecessor Removal)

- **Idea:**  
  Instead of checking subsequences, for each word, we generate all possible predecessors by removing one character at every position. We then look up these predecessors in a hash map that stores the longest chain length ending with that word. By processing words in ascending order of length, we ensure all possible predecessors are already processed. This way, we build up the longest chain lengths efficiently without expensive subsequence checks.

- **Time Complexity:** O(n * ℓ²)  
  For each of the n words, we generate up to ℓ predecessors, each taking O(ℓ) time to create.

- **Space Complexity:** O(n * ℓ)  
  Storing all words and their chain lengths in a hash map.
