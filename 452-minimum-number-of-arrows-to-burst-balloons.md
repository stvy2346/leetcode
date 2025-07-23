## 452 - Minimum Number of Arrows to Burst Balloons

> Difficulty: Medium 

## 🧠 Intuition




## 📝 Approach

[Step-by-step explanation of the algorithm, any edge cases considered, and why this approach works.]


## 💻 Code (C++)

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),[](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });
        int n = points.size(),ans = 1;
        int end = points[0][1];
        for(int i=1;i<n;i++){
            if(points[i][0] > end){
                ans++;
                end = points[i][1];
            }
        }
        return ans;
    }
};
```
## 🧮 Complexity Analysis

| Complexity | Value |
|------------|-------|
| 🕒 Time     | O(n) |
| 💾 Space    | O(1) |

> Explanation:
> - Time: [Explain why the algorithm runs in O(...) time.]
> - Space: [Explain why the algorithm uses O(...) space.]

## 🔁 Alternative Solutions

### 🔹 Approach: [Name of Approach]
- **Idea:** [Brief explanation of the approach]
- **Time Complexity:** O(...)
- **Space Complexity:** O(...)
- **Why not used:** [e.g., Slower, more complex, not optimal for large inputs]

> Note: Include only if you explored other valid methods or want to show trade-offs.
