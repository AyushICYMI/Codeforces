# Raspberries

## Problem Statement

You are given an array of integers `a1, a2, ..., an` and a number `k (2 ≤ k ≤ 5)`.

In one operation, you can increase any element of the array by 1 (i.e., set `ai = ai + 1`).

Find the minimum number of such operations needed to make the product of all array elements divisible by `k`.

---

## Approach

Since the only allowed operation is incrementing elements by 1, we want to make the product of all elements divisible by `k` with as few increments as possible.

### For `k = 2`, `3`, `5` (prime numbers):
- If any element is divisible by `k`, then the product is divisible.
- Otherwise, compute the minimal `k - (v[i] % k)` to find the least number of increments needed.

### For `k = 4` (not a prime):
- 4 = 2 × 2 → requires two factors of 2.
- Count:
  - Elements divisible by 4 → res = 0.
  - Pairs of elements divisible by 2.
  - Otherwise, try the minimum additions to make two values even.

---

## Time Complexity

- O(n) per test case — each array element is processed once.

## Space Complexity

- O(n) for storing input array.
- O(1) for extra variables.

---

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
        int n, k;
        cin >> n >> k;
        
        vector<int> v(n);
        for (int i = 0; i < n; i++) cin >> v[i];
        
        int res = INT_MAX;
        if (k == 2 || k == 3 || k == 5) {
            for (int i = 0; i < n; i++) {
                if (v[i] % k == 0) {
                    res = 0;
                    break;
                } else {
                    res = min(res, k - v[i] % k);
                }
            }
            cout << res << endl;
        } else if (k == 4) {
            int evenCount = 0;
            res = 4;
            for (int i = 0; i < n; i++) {
                if (v[i] % 4 == 0) {
                    res = 0;
                    break;
                }
                int remainder = v[i] % 4;
                res = min(res, 4 - remainder);
                
                if (v[i] % 2 == 0) {
                    evenCount++;
                }
            }
            if (evenCount >= 2) {
                res = min(res, 0);
            } else if (evenCount == 1 && n >= 2) {
                res = min(res, 1); 
            } else if (n >= 2) {
                res = min(res, 2); 
            }
            cout << res << endl;
        }
    }
    return 0;
}
