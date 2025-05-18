# Traffic Light Crossing Problem

## Problem Statement
You encounter a traffic light with an unusual repeating pattern:
- Colors: red (r), yellow (y), green (g)
- Pattern repeats every `n` seconds (given by string `s`)
- Can only cross when light is green
- Know current color but not current time
- Find **minimum guaranteed time** to wait until green appears

## Approach
### Key Insight
The solution requires finding the **maximum distance** between any occurrence of the current color and the next green light in the cyclic pattern.

### Solution Steps
1. **Double the string** to handle circular nature (`s += s`)
2. For each occurrence of current color `c`:
   - Scan forward until finding next 'g'
   - Track maximum distance found
3. The maximum distance found is the guaranteed wait time

## Complexity Analysis
| Complexity | Description |
|------------|-------------|
| **Time**   | O(n)        | 
| **Space**  | O(n)        |

## Solution Code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int t;
    cin >> t;
    while(t--) {
        int n; 
        cin >> n;
        
        char c; 
        cin >> c;
        
        string s; 
        cin >> s;
        
        s += s; // Handle circular pattern
        int i = 0, ans = 0;
        int j;
        while(i < n) {
            if (s[i] != c) i++;
            else {
                j = i;
                while(j < 2 * n) {
                    if (s[j] == 'g') {
                        ans = max(ans, j - i);
                        i = j + 1;
                        break;
                    }
                    else j++;
                }
            }
        }
        cout << ans << endl;
    }
    return 0;
}
