## 2566 - Maximum Difference by Remapping a Digit

> Difficulty: Easy

## 🧠 Intuition

We are given a number `num` and asked to find the **maximum difference** we can get by remapping **one digit** in the number to **any other digit (0–9)**. The rule is that **all occurrences** of the chosen digit must be changed.

To get the **maximum** number:
- Replace the **first digit** (from left to right) that is **not '9'** with **'9'**.
- This increases the number the most since digits on the left have a higher place value.

To get the **minimum** number:
- Replace the **first digit** that is **not '0'** with **'0'**.
- This creates a **leading zero**, which is **intentional** here to reduce the number significantly.
- The built-in `stoi()` function will handle the leading zero correctly by removing it when converting back to an integer.



## 📝 Approach

1. Convert the number to a string.
2. For the **maximum** number:
   - Find the first digit that is not `'9'`.
   - Replace all occurrences of it with `'9'`.
3. For the **minimum** number:
   - Find the first digit that is not `'0'`.
   - Replace all occurrences of it with `'0'`.
4. Convert both strings back to integers using `stoi()`.
5. Return the difference.


## 💻 Code (C++)

```cpp
class Solution {
public:
    int minMaxDifference(int num) {
        string s1 = to_string(num);
        char c1 = '9';
        for(auto c : s1){
            if(c == '9') continue;
            else {
                c1 = c;
                break;
            }
        }
        for(auto& c : s1){
            if(c == c1) c = '9';
        }

        string s2 = to_string(num);
        char c2 = '0';
        for(auto c : s2){
            if(c == '0') continue;
            else{
                c2 = c;
                break;
            }
        }
        for(auto& c : s2){
            if(c == c2) c = '0';
        }
        return stoi(s1) - stoi(s2);
    }
};
```
## 🧮 Complexity Analysis

| Complexity | Value |
|------------|-------|
| 🕒 Time     | O(n) |
| 💾 Space    | O(n) |

> Explanation:
> - Time: We iterate over the digits a few times, where n is the number of digits in num.
> - Space: Temporary string copies of the number take O(n) space.

## 🔁 Alternative Solutions

### 🔹 Approach: Using `string::find_first_not_of()`
- **Idea:** Instead of manually looping through the string to find the first non-'9' or non-'0' digit, we can use the standard library function `string::find_first_not_of()` to simplify the logic and make the code cleaner.

    - `find_first_not_of('9')` helps us find the digit to replace for the **maximum** value.
    - `find_first_not_of('0')` helps us find the digit to replace for the **minimum** value.
    - We use `std::replace()` to replace all occurrences of the digit in one line.
- **Time Complexity:** O(log num)
- **Space Complexity:** O(log num)
