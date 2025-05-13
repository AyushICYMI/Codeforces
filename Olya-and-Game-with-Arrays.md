# Olya and Game with Arrays

## Problem Statement

Artem suggested a game to the girl Olya. There is a list of **n** arrays, where the **i-th** array contains **mi ≥ 2** positive integers:  
`a[i,1], a[i,2], ..., a[i,mi]`.

Olya can move **at most one (possibly 0)** integer from each array to another array.  
Note that integers can be moved from one array only once, but integers can be added to one array **multiple times**, and all the movements are done **at the same time**.

The **beauty** of the list of arrays is defined as: 
- `For each array, we find the minimum value in it and then sum up these values.`

The goal of the game is to **maximize the beauty** of the list of arrays after performing the allowed movements.

---

## Approach

### Key Observations

- Each array has at least two elements, so we can always find the two smallest values: `min1` and `min2`.
- If we assume we **move the smallest element** `min1` from each array elsewhere, then the new minimum for each array becomes `min2`.
- The sum of all such second-smallest values is our baseline beauty.
- To improve this, we can **keep one** of the `min1` values (preferably the global minimum among them) instead of its corresponding `min2`.

### Strategy

1. Sort each array individually.
2. Extract:
   - `min1[i] = v[i][0]` (smallest in i-th array)
   - `min2[i] = v[i][1]` (second smallest in i-th array)
3. Let:
   - `sumMin2 = sum of all second smallest values`
   - `minOfMin1 = smallest among all min1`
   - `minOfMin2 = smallest among all min2`
4. Final beauty:
   - `Beauty = sumMin2 - minOfMin2 + minOfMin1`

This ensures we maximize the sum of minimums by reinserting one best `min1`.

---

## Time Complexity

- Sorting each array: `O(N * M log M)` where M is average array size.
- Remaining operations: `O(N)`

**Total:** `O(N * M log M)`

---

## Space Complexity

- Input storage: `O(N * M)`
- Additional space: negligible

**Total:** `O(N * M)`

---

## C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while(t--) {
        int n;
        cin >> n;
        vector<vector<int>> v;
        
        for (int i = 0; i < n; i++) {
            int m;
            cin >> m;
            vector<int> w(m);
            for (int i = 0; i < m; i++) cin >> w[i];
            sort(w.begin(), w.end());
            v.push_back(w);
        }

        ll b = 0;
        int minF = INT_MAX;
        for (int i = 0; i < n; i++) {
            b += v[i][1];
            minF = min(minF, v[i][0]);
        }

        if (n == 1) {
            cout << minF << endl;
            continue;
        }

        int minS = INT_MAX;
        int minSIndex = -1;
        for (int i = 0; i < n; i++) {
            if (minS > v[i][1]) {
                minS = v[i][1];
                minSIndex = i;
            }
        }

        b = b - v[minSIndex][1] + minF;
        cout << b << endl;
    }
}
