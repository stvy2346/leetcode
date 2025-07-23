## 452 - Minimum Number of Arrows to Burst Balloons

> Difficulty: Medium 

## 🧠 Intuition
Since an arrow can burst any number of overlapping balloons, we want to maximize the number of balloons each arrow hits. If we sort by the end position of each balloon, we can greedily choose to shoot an arrow at the earliest possible moment that still hits the current balloon. This gives us the best chance to cover as many subsequent overlapping balloons as possible, minimizing the number of arrows used.

## 📝 Approach  
1. **Sort the Balloons**:  
   Sort the `points` array based on the ending coordinate of each balloon. This allows us to always consider the balloon that ends earliest and try to burst as many overlapping balloons as possible with one arrow.

2. **Initialize**:  
   Start with one arrow and point it at the end of the first balloon.

3. **Iterate Through Balloons**:  
   For each subsequent balloon:
   - If the **start** of the current balloon is greater than the **end** of the last balloon we shot, it means there's no overlap, so we need a new arrow.
   - Update the arrow position to the **end** of this new balloon.

4. **Return the Arrow Count**:  
   After traversing all balloons, the arrow count will represent the minimum number of arrows required.


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
| 🕒 Time     | O(n log n) |
| 💾 Space    | O(1) |

> Explanation:
> - **Time:** The dominant operation is sorting the `points` array, which takes **O(n log n)** time, where `n` is the number of balloons. The subsequent loop through all the points takes **O(n)** time. So the total time complexity is **O(n log n)**.
> - **Space:** The sorting is done in-place, and we only use a few extra variables (`end`, `ans`), so the extra space usage is **O(1)** (constant space).

