## 673 - Number of longest increasing subsequence

> Difficulty:  Medium

## 🧠 Intuition

Since we need to count the number of ways in which the **Longest Increasing Subsequence (LIS)** occurs, we must use **dynamic programming**. To compute these counts, we need to identify a pattern or relationship within the `dp` table used for calculating the LIS length.

We can approach this in two ways:
- Either **modify** the standard LIS algorithm, or  
- **Add additional logic** to track the number of ways each LIS can be formed.

To do this, we'll maintain two arrays:
- One to store the **length** of the LIS ending at each index.
- Another to store the **number of ways** to form such LIS.

By combining length updates with count tracking, we can efficiently determine both the LIS length and the number of such sequences in a single pass using nested loops.

---

## 📝 Approach

1. **Initialize Two Arrays**:  
   - `dp[i]`: Stores the length of the LIS ending at index `i`. Initialize all values to `1`.  
   - `ways[i]`: Stores the number of LIS ending at index `i`. Initialize all values to `1`.

2. **Nested Loops**:  
   For each index `i`, loop through all previous indices `j < i`:
   - If `nums[i] > nums[j]`, we can extend the subsequence ending at `j`.
     - If `dp[j] + 1 > dp[i]`:  
       We've found a **longer** subsequence ending at `i`. Update `dp[i] = dp[j] + 1` and set `ways[i] = ways[j]`.
     - If `dp[j] + 1 == dp[i]`:  
       We've found **another way** to form the same-length LIS. Add `ways[j]` to `ways[i]`.

3. **Determine the Length of LIS**:  
   - The length of the LIS is the **maximum value** in the `dp` array.

4. **Count All LIS**:  
   - Sum all `ways[i]` for which `dp[i] == LIS length`. This gives the **total number of LIS** in the array.


## 💻 Code (C++)

```cpp
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n,1);
        vector<int> ways(n,1);
        for(int i=0;i<n;i++){
            for(int j = 0;j<i;j++){
                if(nums[i] > nums[j] && dp[i] < dp[j] + 1){
                    dp[i] = dp[j] + 1;
                    ways[i] = 0;
                }
                if(dp[i] == dp[j]+1){
                    ways[i] += ways[j];
                }
            }
        }
        int lis = *max_element(dp.begin(),dp.end());
        int ans = 0;
        for(int i=0;i<n;i++){
            if(dp[i] == lis) ans += ways[i];
        }
        return ans;
    }
};
```
## 🧮 Complexity Analysis

| Complexity | Value |
|------------|-------|
| 🕒 Time     | O(n<sup>2</sup>) |
| 💾 Space    | O(n) |

> Explanation:
> - Time: We use nested loops to go through the list nums. The outer loop runs n times, and for each iteration i (from 0 to n - 1), the inner loop runs up to i times.
> - Space: We use two vectors (dp and ways) of length n each.

## 🔁 Alternative Solutions

### 🔹 Approach: Top-down Dynamic Programming
- **Idea:** Change the order in which we calculate the values of the DP table.
- **Time Complexity:** O(n<sup>2</sup>)
- **Space Complexity:** O(n)

