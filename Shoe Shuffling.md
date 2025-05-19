# Shoe Shuffling

## Problem Statement

A class of students is tired of wearing the same shoes every day, so they decide to **shuffle their shoes**. Each student owns a **pair of shoes** with a specific size. A pair of shoes is **inseparable** and is treated as a single item.

There are `n` students, and you're given an array `s` (of length `n`) sorted in non-decreasing order, where `s[i]` is the shoe size of the i-th student.

You must output a **valid shuffling** such that:

- No student receives **their original shoes** (i.e., `p[i] != i`).
- Each student receives a pair of shoes with **size ≥ their original size**.

Return a permutation `p` of `{1, 2, ..., n}`, where student `i` receives the shoes of student `p[i]`. If no valid shuffling exists, return `-1`.

A **permutation** is a rearrangement of distinct elements. For example, `[2,3,1,5,4]` is a valid permutation for `n = 5`, but `[1,2,2]` or `[1,3,4]` is not.

---

## Approach

We must assign each student a pair of shoes **not belonging to them**, and the new shoes must have a **size ≥ their original pair**.

### Key Insight

- If any shoe size occurs **only once**, it's **impossible** to assign that shoe to someone else **and** satisfy the non-decreasing size condition.
- So, we group students by their shoe sizes. If any size occurs only once, return `-1`.

### Strategy

1. Group students by shoe size.
2. For each group:
   - Perform a **cyclic shift** within the group. For example, for students `[i, j, k]`, assign:
     - `i → j`, `j → k`, `k → i`
   - This ensures each student gets a pair of the same size (≥ their own) and **not their own**.
3. Concatenate all groups and print the result.

This works because:
- Students with same-sized shoes can safely swap among themselves.
- We're guaranteed sorted input, so the grouping is straightforward.

---

## Time Complexity

- **Time:** O(n)
  - Reading input, grouping, and printing each take linear time.
  
## Space Complexity

- **Space:** O(n)
  - For storing the output permutation.

---

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        vector<int> nums(n);
        for (int i = 0; i < n; i++) cin >> nums[i];

        map<int, int> freq;
        for (int num : nums) freq[num]++;

        bool valid = true;
        int sum = 0;
        vector<int> result;

        for (auto it : freq) {
            if (it.second == 1) {
                valid = false;
                break;
            } else {
                sum += 2;
                for (int i = 0; i < it.second; i++) {
                    if (i == it.second - 1) result.push_back(sum - 1);
                    else result.push_back(sum + i);
                }
                sum -= 2;
                sum += it.second;
            }
        }

        if (valid) {
            for (int i = 0; i < n; i++) cout << result[i] << " ";
            cout << endl;
        } else {
            cout << -1 << endl;
        }
    }
}
