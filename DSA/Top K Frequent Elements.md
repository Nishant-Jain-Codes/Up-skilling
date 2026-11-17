# Top K Frequent Elements

- Link: https://neetcode.io/problems/top-k-elements-in-list
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: frequency count + min-heap of size k (or bucket sort by frequency)
- Language: Python
- Solved on: 2026-11-17 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
# hash freq for each number
# min priority queue of size k
# space of a priority queue of size k: O(k)
# space : O(N) (hash)
# time : N log k
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        from collections import Counter
        import heapq

        hsh = Counter(nums)
        pq = []

        for num, freq in hsh.items():
            heapq.heappush(pq, (freq, num))

            if len(pq) > k:
                heapq.heappop(pq)

        ans = []

        while pq:
            item = heapq.heappop(pq)
            ans.append(item[1])

        return ans
```

## Approach (own words)
Count how often each number appears. Keep a min-heap of the `k` most frequent (frequency, number) pairs. After every push, if the heap has more than `k` items, pop the smallest frequency. What's left at the end are the `k` most frequent numbers.

## Complexity
Let N = length of `nums` and D = number of distinct values (D ≤ N).
- Time: counting O(N), then D pushes/pops of O(log k) each. Total **O(N + D log k)**, which is bounded by O(N log k). Your claim is right.
- Space: Counter O(D) + heap O(k), so O(N) overall. Your note "heap of size k is O(k)" is right.

## Critique
- Correct, and you reasoned about the heap's own space, which is good.
- Tuples `(freq, num)` compare by frequency first, then by number. Ties are fine here because numbers are ints.
- The result comes out in ascending-frequency order. The problem allows any order, so that's fine.
- Put the `import` lines at the top of the file, not inside the function.
- Small simplification: `heapq.heappushpop(pq, (freq, num))` once the heap has `k` items does the push and pop in one call.
- `heapq.nlargest(k, hsh.keys(), key=hsh.get)` is the one-liner (also about O(N log k)).

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Sort the (freq, num) pairs | O(D log D) | O(D) |
| Min-heap of size k (mine) | O(N log k) | O(N) |
| **Bucket sort by frequency** | **O(N)** | O(N) |

The bucket-sort version is what an interviewer wants if they ask "can you do better than N log k?". A number's frequency is at most N, so use the frequency as an index:
```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = Counter(nums)
        buckets: list[list[int]] = [[] for _ in range(len(nums) + 1)]  # index = frequency
        for num, freq in count.items():
            buckets[freq].append(num)
        res: list[int] = []
        for freq in range(len(buckets) - 1, 0, -1):   # highest frequency first
            for num in buckets[freq]:
                res.append(num)
                if len(res) == k:
                    return res
        return res
```

## Mistake / what to remember
- "Top k of something" means a size-k heap, N log k. If asked for better than that, think bucket sort, since the key (frequency) has a bounded range.
- A min-heap of size k keeps the k largest, because the smallest of the current best gets evicted.
- Imports go at the top.
- Revisit: write the bucket-sort version from memory at the +14 day review.

## Review
- [ ] +3 days (2026-11-20)
- [ ] +14 days (2026-12-01)
