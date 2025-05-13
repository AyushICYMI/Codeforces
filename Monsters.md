# Monsters

## Problem Statement
Monocarp is playing a computer game where his character has to kill `n` monsters, each with an initial health value. The character has an ability that deals `k` damage to the monster with the highest current health. If there are multiple monsters with the same highest health, the one with the smallest index is chosen. This process repeats until all monsters are dead. The task is to determine the order in which the monsters die.

## Approach
1. **Calculate Remainders**: For each monster, compute the remainder when its health is divided by `k`. If the remainder is zero, treat it as `k`.
2. **Group by Remainders**: Use a map to group monsters by their remainders. Each key in the map is a remainder, and the value is a list of monster indices with that remainder.
3. **Sort Indices**: Within each remainder group, sort the monster indices in descending order.
4. **Reconstruct Order**: Starting from the end of the result array, place the sorted indices from each remainder group in ascending order of remainders (due to `std::map`'s default ordering).

This approach efficiently determines the attack order by leveraging the properties of remainders, avoiding the need for explicit simulation of each attack.

## Time and Space Complexity
- **Time Complexity**: O(n log n) per test case, due to sorting the indices within each remainder group.
- **Space Complexity**: O(n) per test case, for storing the map and the result array.

## C++ Solution
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
        map<int, vector<int>> m;
        for (int i = 0; i < n; i++) {
            if (v[i] % k == 0) m[k].push_back(i + 1);
            else m[v[i] % k].push_back(i + 1);
        }
        
        
        int index = n - 1;
        for (auto i : m) {
            sort(i.second.begin(), i.second.end(), greater<>());
            for (auto j : i.second) {
                v[index] = j;
                index -= 1;
            }
        }
        
        for (int i = 0; i < n; i++) cout << v[i] << " ";
        cout << endl;
    }
}
