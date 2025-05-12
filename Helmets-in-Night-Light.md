# Problem: Helmets in Night Light

Pak Chanek, the chief of the village Khuntien, needs to notify all `n` residents of an important announcement.

### Rules:
1. Pak Chanek can directly notify one or more residents, at a cost of `p` per person.
2. Once a resident knows the announcement, they can share it with others using a helmet-shaped device. The cost for each resident `i` to share with others is `b[i]` per person, and they can share the announcement with at most `a[i]` residents.

### Goal:
Find the **minimum cost** for Pak Chanek to notify all `n` residents about the announcement.

---

### Input:
- Two integers `n` and `p`, where:
  - `n` is the number of residents.
  - `p` is the cost to notify a resident directly.
- Two arrays:
  - `a[1...n]`: Maximum number of residents each person can share with.
  - `b[1...n]`: Cost per share for each resident.

### Output:
- A single integer representing the **minimum total cost** to notify all `n` residents.

---

### Approach

The problem can be solved using a **greedy algorithm** to minimize the cost:

1. **Initial Step**: Pak Chanek directly notifies the first set of residents, at a cost of `p` per person.
2. **Sharing the Announcement**: After Pak Chanek directly notifies some residents, other residents can share the announcement to others using their helmet-shaped devices.
   - Each resident can share the announcement to a limited number of other residents (`a[i]`), and there is a cost for each share (`b[i]`).
   - We aim to minimize the sharing cost by always picking the least expensive sharing option first (using a **priority queue**).
3. **Greedy Choice**: Keep sharing the announcement using the least costly option until all residents are notified.

### Steps:
1. Push all `(b[i], a[i])` pairs into a **min-heap** (priority queue) to access the cheapest share cost.
2. Start with one resident notified and add the base cost `p`.
3. Keep notifying more residents using the cheapest available share cost.
4. If at any point, the cost of sharing exceeds `p`, directly notify the remaining residents.

---

### Time Complexity (TC)
- **O(n log n)** per test case, where `n` is the number of residents.
  - The priority queue operations (insertions and deletions) each take `O(log n)` time, and there are `n` such operations.

### Space Complexity (SC)
- **O(n)** for storing the arrays `a[]` and `b[]`, and the priority queue.

---

### Solution Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int t;
    cin >> t;
    while(t--) {
        int n, p;
        cin >> n >> p;
        
        vector<int> a(n);
        for (int i = 0; i < n; i++) cin >> a[i];
        
        vector<int> b(n);
        for (int i = 0; i < n; i++) cin >> b[i];
        
        if (n == 1) {
            cout << p << '\n';
            continue;
        }
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minH;
        for (int i = 0; i < n; i++) {
            minH.push({b[i], a[i]});
        }
        
        long long minCost = p;  
        int sharedTo = 1;        
        
        while(!minH.empty() && sharedTo < n) {
            int bi = minH.top().first;
            int ai = minH.top().second;
            minH.pop();
            
            if (bi >= p) {
                break;
            }
            
            int canShare = min(ai, n - sharedTo);
            minCost += (long long)canShare * bi;
            sharedTo += canShare;
        }
        
        if (sharedTo < n) {
            minCost += (long long)(n - sharedTo) * p;
        }
        
        cout << minCost << '\n';
    }
    return 0;
}
