# Group Anagrams

- Link: https://neetcode.io/problems/anagram-groups
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: canonical key (a signature that is identical for all anagrams) + hash map of lists
- Language: Python
- Solved on: 2026-11-16 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        hsh = {}

        for wrd in strs:
            key = "".join(sorted(wrd))

            if key not in hsh:
                hsh[key] = []

            hsh[key].append(wrd)

        return list(hsh.values())
```

## Approach (own words)
Words that are anagrams of each other become identical when their letters are sorted. So I use the sorted word as a key and collect all the original words that share the same key into one list. The dict's values are the groups.

## Complexity
Let n = number of words and k = length of the longest word.
- Time: **O(n · k log k)**, since each word is sorted.
- Space: O(n · k) for the keys and the groups.

## Critique
- Correct, and a good reuse of the idea from [[Valid Anagram]].
- Cleaner with `defaultdict(list)`, which removes the `if key not in hsh` block.
- You didn't state the complexity this time. Build the habit of writing it in the comment.
- Empty strings work (key is `""`), so no special case is needed.

Tidier version:
```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
        groups: defaultdict[str, list[str]] = defaultdict(list)
        for word in strs:
            groups["".join(sorted(word))].append(word)
        return list(groups.values())
```

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Sorted word as key (mine) | O(n · k log k) | O(n · k) |
| Count of each letter as key | O(n · k) | O(n · k) |

Count-based version (the one the interviewer may want if you say "can we avoid sorting?"). It assumes lowercase a–z:
```python
groups = defaultdict(list)
for word in strs:
    count = [0] * 26
    for c in word:
        count[ord(c) - ord("a")] += 1
    groups[tuple(count)].append(word)   # a list is unhashable, so convert to a tuple
```

## Mistake / what to remember
- Dict keys must be hashable: strings and tuples are, lists are not. That's why the count version needs `tuple(count)`.
- Pattern: "group things that are equivalent" means compute a canonical key and group by it. It comes back in many problems.
- Habit: write the complexity comment before you submit.

## Review
- [ ] +3 days (2026-11-19)
- [ ] +14 days (2026-11-30)
