# Encode and Decode Strings

- Link: https://neetcode.io/problems/string-encode-and-decode (★ LeetCode premium, free on NeetCode)
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: length-prefix encoding (`length#string`), so the content never needs escaping
- Language: Python
- Solved on: 2026-11-17 · minutes: 
- Accepted on submission #19. My first approach used a plain delimiter and failed on the delimiter-collision problem (a string containing the delimiter), so I switched to length-prefix encoding.
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:

    def encode(self, strs: List[str]) -> str:
        s = ""
        for wrd in strs:
            l = len(wrd)
            s += str(l) + "#" + wrd
        return s

    def decode(self, s: str) -> List[str]:
        ans = []
        i = 0

        while i < len(s):
            l = ""

            while s[i] != "#":
                l += s[i]
                i += 1

            i += 1
            slen = int(l)

            si = i
            ei = i + slen

            wrd = s[si:ei]
            ans.append(wrd)

            i = ei

        return ans
```

## Approach (own words)
Write each word as its length, a `#`, then the word itself. When decoding, read digits until the first `#` to get the length, then take exactly that many characters as the word and jump past them. Because the length says exactly where each word ends, the words can safely contain `#`, digits or anything else.

## Complexity
Let N = the total length of all strings.
- Time: O(N) for decode. Encode is O(N) too if you build it with a list and `join` (see critique).
- Space: O(N) for the encoded string and for the decoded output.

## Critique
- Correct, and the approach is the standard one.
- **`s +=` in a loop** can be O(N²) in the worst case, because strings are immutable and each `+=` may copy. CPython often optimises it, but you shouldn't rely on that. Collect the pieces in a list and `"".join(...)` once.
- Reading the length digit by digit works. `s.find("#", i)` and one slice is simpler.
- Empty strings work (`"0#"`), and so does a string containing `#` (the first `#` always ends the length).
- `l` as a variable name is easy to confuse with `1`. Use `length`.

Tidier version:
```python
class Solution:
    def encode(self, strs: list[str]) -> str:
        return "".join(f"{len(w)}#{w}" for w in strs)

    def decode(self, s: str) -> list[str]:
        res: list[str] = []
        i = 0
        while i < len(s):
            j = s.find("#", i)          # end of the length prefix
            length = int(s[i:j])
            res.append(s[j + 1 : j + 1 + length])
            i = j + 1 + length
        return res
```

## Other approaches
| Approach | Idea | Problem |
|---|---|---|
| Length prefix (mine) | `5#hello` | none, this is the robust one |
| Plain delimiter, e.g. `,` or `#` | join with a separator | breaks when a string contains the separator |
| Delimiter + escaping | escape the separator inside strings | works, but fiddlier than the length prefix |

## Mistake / what to remember
- Plain-delimiter approach failed on collisions, which is the lesson of the problem. Next time, before coding, ask "what input would break my separator?" and test that input first.
- A separator alone is not safe. A length prefix removes the ambiguity.
- Use `list` + `join` to build strings in a loop.
- Skipped #5 Top K Frequent Elements. Remember to come back to it.

## Review
- [ ] +3 days (2026-11-20)
- [ ] +14 days (2026-12-01)
