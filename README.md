# 🏆 The FAANG DSA Pattern Mastery Roadmap

> **Read this first:** This guide will feel long. That's intentional. Skim nothing on your first read — the "common mistakes" and "key insights" are where 80% of your interview edge lives. Bookmark this, paste it into Notion, and treat it like your bible for the next 3–4 months.

---

## 📅 Master Timeline (Before We Dive In)

| Phase                 | Weeks  | Focus                 | Goal                            |
| --------------------- | ------ | --------------------- | ------------------------------- |
| **Foundation**  | 1–4   | Patterns 1–8         | Solve any Easy, attempt Mediums |
| **Core**        | 5–10  | Patterns 9–17        | Solve Mediums confidently       |
| **Advanced**    | 11–16 | Patterns 18–28       | Crack Hards, own DP             |
| **Mock Sprint** | 17–20 | Full revision + mocks | Interview-ready                 |

**Daily budget:** 1–2 hrs. **Non-negotiable split:** 70% solving, 30% reviewing what you got wrong.

---

---

# PATTERN #1 — Arrays & Hashing

---

### 🧭 When to Use It

* Problem involves finding **duplicates, counts, frequencies, or lookups**
* You need O(1) access to previously seen elements
* Keywords:  *"find if exists"* ,  *"count occurrences"* ,  *"group by"* , *"two elements that sum to"*
* Constraints hint at O(n) expected solution (large input, no sorting allowed)

### 🧠 Core Intuition

A HashMap trades space for speed. Instead of scanning the array again and again to find something, you store what you've seen in a map and look it up in O(1). The core move: **store what you need later, as you go.**

### 🎯 Mental Model

> *You're checking coats at a party. Instead of searching through every coat when someone asks, you give them a ticket (key → index/value). Instant retrieval.*

### ⏱ Complexity Template

* **Time:** O(n) — one pass, O(1) lookup per element
* **Space:** O(n) — the hash map

---

### 📚 Problem List

| #  | Problem                                | Link                                                                | Difficulty | 💡 Key Insight                                                                                  | ⚠️ Common Mistake                                                               |
| -- | -------------------------------------- | ------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| 1  | Contains Duplicate                     | [LC #217](https://leetcode.com/problems/contains-duplicate/)           | 🟢 Easy    | Add to set as you go; if element already in set → duplicate found.                             | Sorting works too but is O(n log n) — set is O(n).                               |
| 2  | Valid Anagram                          | [LC #242](https://leetcode.com/problems/valid-anagram/)                | 🟢 Easy    | Count char frequencies in s; decrement for t. Any non-zero at end = not anagram.                | Don't use two separate maps — use one and +/- on it.                             |
| 3  | Two Sum                                | [LC #1](https://leetcode.com/problems/two-sum/)                        | 🟢 Easy    | Store `target - num`as key → index. One pass.                                                | Returning values instead of indices; checking `num == target - num`edge case.   |
| 4  | Group Anagrams                         | [LC #49](https://leetcode.com/problems/group-anagrams/)                | 🟡 Medium  | Key = sorted string (or char freq tuple). Group by key in a map.                                | Using sorted string as key is fine; freq array as tuple is faster.                |
| 5  | Top K Frequent Elements                | [LC #347](https://leetcode.com/problems/top-k-frequent-elements/)      | 🟡 Medium  | Freq map + bucket sort by frequency (index = freq, value = list of nums).                       | Using a heap is O(n log k) — bucket sort gets you O(n). Know both.               |
| 6  | Encode & Decode Strings                | [LC #271](https://leetcode.com/problems/encode-and-decode-strings/)    | 🟡 Medium  | Prefix each string with its length + a delimiter:`"4#word"`.                                  | Using a simple delimiter like `#`alone breaks on strings containing `#`.      |
| 7  | Product of Array Except Self           | [LC #238](https://leetcode.com/problems/product-of-array-except-self/) | 🟡 Medium  | Two passes: left products, then right products. No division needed.                             | Using division (breaks on zeros). The two-pass prefix/suffix trick is the unlock. |
| 8  | Valid Sudoku                           | [LC #36](https://leetcode.com/problems/valid-sudoku/)                  | 🟡 Medium  | Three sets: rows, cols, 3×3 boxes. Box index =`(r//3, c//3)`.                                | Forgetting to validate boxes, only rows and cols.                                 |
| 9  | Longest Consecutive Sequence           | [LC #128](https://leetcode.com/problems/longest-consecutive-sequence/) | 🟡 Medium  | Use a set. Only start counting from sequence starts (`n-1`not in set).                        | Sorting — that's O(n log n). The O(n) trick is skipping non-sequence-starters.   |
| 10 | Subarray Sum Equals K                  | [LC #560](https://leetcode.com/problems/subarray-sum-equals-k/)        | 🟡 Medium  | Prefix sum + hashmap.`count += map[prefixSum - k]`.                                           | Forgetting to initialize `map[0] = 1`— misses subarrays starting from index 0. |
| 11 | 4Sum II                                | [LC #454](https://leetcode.com/problems/4sum-ii/)                      | 🟡 Medium  | Split into two pairs. Store sums of first two arrays in a map, count complements in second two. | Trying to do 4 nested loops — the split-in-half trick halves the complexity.     |
| 12 | Majority Element II                    | [LC #229](https://leetcode.com/problems/majority-element-ii/)          | 🟡 Medium  | Boyer-Moore Voting with 2 candidates. At most 2 elements can appear > n/3 times.                | Using a plain frequency map (correct but O(n) space — Boyer-Moore is O(1)).      |
| 13 | Minimum Window Substring (map variant) | [LC #76](https://leetcode.com/problems/minimum-window-substring/)      | 🔴 Hard    | Sliding window + two frequency maps. Shrink when window is valid.                               | This bridges into sliding window — revisit after Pattern 3.                      |

---

---

# PATTERN #2 — Two Pointers

---

### 🧭 When to Use It

* Array is **sorted** (or can be sorted without losing info)
* You need to find **pairs, triplets, or subarrays** satisfying a condition
* You're comparing elements from **both ends** moving inward
* Keywords:  *"pair with sum"* ,  *"remove duplicates"* ,  *"reverse"* , *"palindrome check"*
* Brute force is O(n²) — two pointers gets you O(n)

### 🧠 Core Intuition

Place one pointer at the start, one at the end. Move them toward each other based on whether your current result is too big or too small. You eliminate entire swaths of the search space with each move.

### 🎯 Mental Model

> *Two people walking toward each other on a hallway. They adjust direction based on what they find — they don't need to check every possible pair, just follow the signal.*

### ⏱ Complexity Template

* **Time:** O(n) or O(n log n) if sorting is needed first
* **Space:** O(1) extra (in-place)

---

### 📚 Problem List

| #  | Problem                             | Link                                                                      | Difficulty | 💡 Key Insight                                                                                                        | ⚠️ Common Mistake                                                                       |
| -- | ----------------------------------- | ------------------------------------------------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| 1  | Valid Palindrome                    | [LC #125](https://leetcode.com/problems/valid-palindrome/)                   | 🟢 Easy    | Left and right pointers, skip non-alphanumeric, compare.                                                              | Using extra space to clean string first — do it in-place.                                |
| 2  | Two Sum II (sorted array)           | [LC #167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)   | 🟢 Easy    | Sum too big → move right left. Sum too small → move left right.                                                     | Using a hashmap — that's Pattern 1. The sorted constraint demands two pointers.          |
| 3  | Remove Duplicates from Sorted Array | [LC #26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | 🟢 Easy    | Slow pointer tracks unique position; fast pointer scans ahead.                                                        | Shifting array elements — O(n²). Use the slow/fast pointer write-in-place trick.        |
| 4  | Move Zeroes                         | [LC #283](https://leetcode.com/problems/move-zeroes/)                        | 🟢 Easy    | Slow pointer = position to write next non-zero. Fast pointer scans.                                                   | Swapping vs. overwriting — swapping maintains relative order of non-zeros.               |
| 5  | 3Sum                                | [LC #15](https://leetcode.com/problems/3sum/)                                | 🟡 Medium  | Sort. Fix one element, two-pointer on the rest. Skip duplicates carefully.                                            | Not skipping duplicates after finding a valid triplet → duplicate results.               |
| 6  | Container With Most Water           | [LC #11](https://leetcode.com/problems/container-with-most-water/)           | 🟡 Medium  | Always move the shorter wall inward — moving the taller one can only decrease area.                                  | Moving the taller pointer — you can prove mathematically this is wrong.                  |
| 7  | Trapping Rain Water                 | [LC #42](https://leetcode.com/problems/trapping-rain-water/)                 | 🔴 Hard    | Water at index i =`min(maxLeft, maxRight) - height[i]`. Track running max from each side.                           | Using a stack approach (valid but harder). Two-pointer with running maxes is cleaner.     |
| 8  | 3Sum Closest                        | [LC #16](https://leetcode.com/problems/3sum-closest/)                        | 🟡 Medium  | Same as 3Sum — track minimum difference from target as you go.                                                       | Forgetting to update the closest when difference equals zero (early exit possible).       |
| 9  | Sort Colors (Dutch Flag)            | [LC #75](https://leetcode.com/problems/sort-colors/)                         | 🟡 Medium  | Three pointers: low, mid, high. Swap based on value at mid.                                                           | Incrementing mid when swapping with high — don't, because swapped element is unexamined. |
| 10 | Remove Nth Node From End of List    | [LC #19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)    | 🟡 Medium  | Fast pointer goes N steps ahead first; then both move until fast hits end.                                            | Off-by-one. Use a dummy head node to cleanly handle edge case of removing head.           |
| 11 | Palindrome Linked List              | [LC #234](https://leetcode.com/problems/palindrome-linked-list/)             | 🟢 Easy    | Find middle (slow/fast), reverse second half, compare.                                                                | Forgetting to restore the list (some interviewers ask you to).                            |
| 12 | 4Sum                                | [LC #18](https://leetcode.com/problems/4sum/)                                | 🟡 Medium  | Fix two outer loops, two-pointer the rest. Skip duplicates at all 4 levels.                                           | Not deduplicating all four pointers — easy to miss the inner duplicates.                 |
| 13 | Minimum Size Subarray Sum           | [LC #209](https://leetcode.com/problems/minimum-size-subarray-sum/)          | 🟡 Medium  | This is actually a sliding window — but two-pointer framing works too. Expand right, shrink left when sum ≥ target. | Resetting the sum fully when shrinking — just subtract the left element.                 |

---

---

# PATTERN #3 — Sliding Window

---

### 🧭 When to Use It

* You need the **best/longest/shortest contiguous subarray or substring**
* The window condition is monotonic: adding elements can only make it better (or worse), not randomly flip
* Keywords:  *"subarray/substring"* ,  *"contiguous"* ,  *"at most k"* , *"longest with condition"*
* If brute force is O(n²) over contiguous ranges → sliding window

### 🧠 Core Intuition

Instead of checking every possible subarray from scratch (O(n²)), maintain a window with two pointers. Expand the right pointer to grow, shrink the left pointer when the window violates the constraint. Each element enters and exits the window at most once → O(n).

### 🎯 Mental Model

> *A rubber band stretched over an array. You stretch it right to include more, and snap the left end forward when it gets too big or invalid.*

### ⏱ Complexity Template

* **Time:** O(n) — each element processed at most twice (once in, once out)
* **Space:** O(k) where k = size of charset or constraint (for the frequency map)

---

### 📚 Problem List

| #  | Problem                                           | Link                                                                                      | Difficulty | 💡 Key Insight                                                                                                | ⚠️ Common Mistake                                                                       |
| -- | ------------------------------------------------- | ----------------------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| 1  | Best Time to Buy & Sell Stock                     | [LC #121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)                    | 🟢 Easy    | Track running minimum price; profit = current price − running min.                                           | Using two nested loops — you only need one pass tracking the min seen so far.            |
| 2  | Maximum Average Subarray I                        | [LC #643](https://leetcode.com/problems/maximum-average-subarray-i/)                         | 🟢 Easy    | Fixed-size window. Slide: add right, remove left. Track max sum.                                              | Recalculating sum from scratch for each window — just slide it.                          |
| 3  | Longest Substring Without Repeating Characters    | [LC #3](https://leetcode.com/problems/longest-substring-without-repeating-characters/)       | 🟡 Medium  | Set/map tracks chars in window. On repeat, shrink left until no repeat.                                       | Moving left by just 1 on repeat — jump left to `charLastIndex + 1`directly.            |
| 4  | Longest Repeating Character Replacement           | [LC #424](https://leetcode.com/problems/longest-repeating-character-replacement/)            | 🟡 Medium  | `windowSize - maxFreqChar > k`→ shrink. You don't need to recalc maxFreq on shrink.                        | Updating maxFreq down when shrinking — keep it as a high-water mark; it can only grow.   |
| 5  | Permutation in String                             | [LC #567](https://leetcode.com/problems/permutation-in-string/)                              | 🟡 Medium  | Fixed window of size `len(s1)`. Compare freq maps (or track a `matches`counter).                          | Rebuilding freq map from scratch each slide — maintain it incrementally.                 |
| 6  | Find All Anagrams in a String                     | [LC #438](https://leetcode.com/problems/find-all-anagrams-in-a-string/)                      | 🟡 Medium  | Exactly like #5 but collect all valid window start indices.                                                   | Same as #5 — incremental freq map update is the key.                                     |
| 7  | Minimum Window Substring                          | [LC #76](https://leetcode.com/problems/minimum-window-substring/)                            | 🔴 Hard    | Variable window. Track how many unique chars are "satisfied". Shrink when all satisfied, tracking min length. | Shrinking too eagerly or not enough — the `formed == required`counter is the unlock.   |
| 8  | Sliding Window Maximum                            | [LC #239](https://leetcode.com/problems/sliding-window-maximum/)                             | 🔴 Hard    | Monotonic deque: keep indices of decreasing elements. Front of deque = max of current window.                 | Using a heap — O(n log n). The deque gives O(n).                                         |
| 9  | Fruit Into Baskets                                | [LC #904](https://leetcode.com/problems/fruit-into-baskets/)                                 | 🟡 Medium  | "At most 2 distinct values in window" → shrink when map size > 2.                                            | This is a disguised "longest subarray with at most K distinct" — recognize the pattern.  |
| 10 | Longest Subarray of 1s After Deleting One Element | [LC #1493](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/) | 🟡 Medium  | Allow at most 1 zero in window. Shrink when zeros > 1. Answer is `windowSize - 1`.                          | Subtracting 1 at the end confuses people — you delete the element, so answer is len - 1. |
| 11 | Subarrays with K Different Integers               | [LC #992](https://leetcode.com/problems/subarrays-with-k-different-integers/)                | 🔴 Hard    | `exactly K = atMost(K) - atMost(K-1)`. Build a helper for "at most K distinct".                             | Trying to solve "exactly K" directly — always decompose into atMost.                     |
| 12 | Max Consecutive Ones III                          | [LC #1004](https://leetcode.com/problems/max-consecutive-ones-iii/)                          | 🟡 Medium  | Allow at most k zeros in window. Track zero count, shrink when zeros > k.                                     | Resetting zero count when shrinking — just decrement if left element was a zero.         |
| 13 | Minimum Number of K Consecutive Bit Flips         | [LC #995](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/)          | 🔴 Hard    | Greedy + sliding window with a flip effect queue — track flips within the window range.                      | This is hard for a reason — if you get here, you're ready for top-tier interviews.       |

---

---

# PATTERN #4 — Prefix Sum

---

### 🧭 When to Use It

* Multiple queries of the type **"sum of elements from index i to j"**
* Questions about **subarrays summing to a target** (combine with hashmap)
* 2D grid problems asking for **rectangle sums**
* Keywords:  *"range sum"* ,  *"subarray sum"* ,  *"cumulative"* , *"running total"*

### 🧠 Core Intuition

Pre-compute cumulative sums so any range sum is a single subtraction: `sum(i,j) = prefix[j+1] - prefix[i]`. Pair with a hashmap to find subarrays with a target sum in O(n) instead of O(n²).

### 🎯 Mental Model

> *Odometer in a car. To find distance between two cities, subtract the odometer reading at city A from city B — you don't re-drive the route.*

### ⏱ Complexity Template

* **Build:** O(n)
* **Query:** O(1) per range query
* **Space:** O(n) for the prefix array

---

### 📚 Problem List

| #  | Problem                            | Link                                                                      | Difficulty | 💡 Key Insight                                                                                          | ⚠️ Common Mistake                                                                    |
| -- | ---------------------------------- | ------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 1  | Range Sum Query - Immutable        | [LC #303](https://leetcode.com/problems/range-sum-query-immutable/)          | 🟢 Easy    | Build prefix array once; answer query as `prefix[r+1] - prefix[l]`.                                   | Off-by-one:`prefix[i] = prefix[i-1] + nums[i-1]`with 1-indexed prefix array.         |
| 2  | Find Pivot Index                   | [LC #724](https://leetcode.com/problems/find-pivot-index/)                   | 🟢 Easy    | `leftSum == totalSum - leftSum - nums[i]`at each index. O(n) with total sum precomputed.              | Computing full left/right sums for every index — O(n²).                              |
| 3  | Subarray Sum Equals K              | [LC #560](https://leetcode.com/problems/subarray-sum-equals-k/)              | 🟡 Medium  | `prefixSum - k`seen before → valid subarray exists. Map stores count of each prefix sum seen.        | Forgetting `map[0] = 1`init — misses subarrays starting at index 0.                 |
| 4  | Continuous Subarray Sum            | [LC #523](https://leetcode.com/problems/continuous-subarray-sum/)            | 🟡 Medium  | `(prefixSum % k)`seen before at index ≤ i-2 → valid. Map stores first seen index of each remainder. | Not checking that subarray length ≥ 2; not initializing `map[0] = -1`.              |
| 5  | Subarray Sums Divisible by K       | [LC #974](https://leetcode.com/problems/subarray-sums-divisible-by-k/)       | 🟡 Medium  | Same prefix mod trick but count all pairs, not just any. Normalize negative mods:`(mod + k) % k`.     | Negative modulo values — Python handles it, Java/C++ don't. Always normalize.         |
| 6  | Find the Highest Altitude          | [LC #1732](https://leetcode.com/problems/find-the-highest-altitude/)         | 🟢 Easy    | Running prefix sum, track max. Direct application.                                                      | Overcomplicating — it's literally one pass.                                           |
| 7  | Count Number of Nice Subarrays     | [LC #1248](https://leetcode.com/problems/count-number-of-nice-subarrays/)    | 🟡 Medium  | Replace odd numbers with 1, even with 0. Now it's "subarray sum = k" — use prefix map.                 | Trying to use sliding window directly — the prefix sum transformation is cleaner.     |
| 8  | Product of Array Except Self       | [LC #238](https://leetcode.com/problems/product-of-array-except-self/)       | 🟡 Medium  | Left prefix products, then right suffix products in one array.                                          | This is a "prefix product" variant — recognize it as the same family.                 |
| 9  | Range Sum Query 2D - Immutable     | [LC #304](https://leetcode.com/problems/range-sum-query-2d-immutable/)       | 🟡 Medium  | 2D prefix:`prefix[r][c] = grid + prefix[r-1][c] + prefix[r][c-1] - prefix[r-1][c-1]`.                 | Inclusion-exclusion formula direction — draw it out once, memorize it.                |
| 10 | Maximum Size Subarray Sum Equals k | [LC #325](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/) | 🟡 Medium  | Prefix map stores first seen index of each sum. Answer = current index - stored index.                  | Storing the latest index (should store earliest to maximize length).                   |
| 11 | Binary Subarrays With Sum          | [LC #930](https://leetcode.com/problems/binary-subarrays-with-sum/)          | 🟡 Medium  | Prefix sum + map — or atMost(goal) - atMost(goal-1) trick.                                             | Both approaches work — knowing two approaches shows depth in interviews.              |
| 12 | Make Sum Divisible by P            | [LC #1590](https://leetcode.com/problems/make-sum-divisible-by-p/)           | 🔴 Hard    | Find shortest subarray whose sum ≡ totalSum mod p. Prefix mod + hashmap (store latest index).          | Not using the latest index here (opposite of #10!) — reason: minimize removal length. |

---

---

# PATTERN #5 — Binary Search

---

### 🧭 When to Use It

**Vanilla:** Array is sorted; find target or insertion position
**On Answer:** You're searching for an optimal value in a range, and you can write an `isValid(mid)` predicate

* Keywords:  *"sorted array"* ,  *"find minimum/maximum"* ,  *"minimize the maximum"* ,  *"k-th element"* , *"search in rotated"*
* If the answer space is monotonic (larger mid → valid, smaller mid → invalid, or vice versa) → binary search on answer

### 🧠 Core Intuition

Every step, eliminate half the search space. The key is identifying **what you're searching over** and  **what makes a midpoint valid** . For "on answer" problems: you're not searching the array, you're searching the  *answer space* .

### 🎯 Mental Model

> *Hot/cold game. You always pick the middle number. "Too hot" → go lower half. "Too cold" → go upper half. You never need to check everything.*

### ⏱ Complexity Template

* **Time:** O(log n) — vanilla. O(n log(range)) — binary search on answer (where n = cost of checking one mid)
* **Space:** O(1)

---

### 📚 Problem List

| #  | Problem                                 | Link                                                                            | Difficulty | 💡 Key Insight                                                                                        | ⚠️ Common Mistake                                                                           |
| -- | --------------------------------------- | ------------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| 1  | Binary Search                           | [LC #704](https://leetcode.com/problems/binary-search/)                            | 🟢 Easy    | `mid = lo + (hi - lo) // 2`. Return -1 if not found.                                                | Integer overflow with `(lo + hi) / 2`in Java/C++. Always use `lo + (hi-lo)/2`.            |
| 2  | Search Insert Position                  | [LC #35](https://leetcode.com/problems/search-insert-position/)                    | 🟢 Easy    | At loop exit,`lo`= insertion position. No extra logic needed.                                       | Writing separate post-loop logic — when loop ends, lo == hi and is the answer.               |
| 3  | First Bad Version                       | [LC #278](https://leetcode.com/problems/first-bad-version/)                        | 🟢 Easy    | Binary search for first `true`in boolean array. When bad,`hi = mid`; else `lo = mid + 1`.       | Using `hi = mid - 1`— you'd miss the bad version itself.                                   |
| 4  | Find Minimum in Rotated Sorted Array    | [LC #153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)     | 🟡 Medium  | If `mid > hi`, minimum is in right half. Else it's in left half (including mid).                    | Comparing mid to lo (not hi) — hi comparison is the correct pivot detection.                 |
| 5  | Search in Rotated Sorted Array          | [LC #33](https://leetcode.com/problems/search-in-rotated-sorted-array/)            | 🟡 Medium  | Determine which half is sorted, then check if target falls in it.                                     | Trying to find the pivot first — you can do it in one binary search pass.                    |
| 6  | Koko Eating Bananas                     | [LC #875](https://leetcode.com/problems/koko-eating-bananas/)                      | 🟡 Medium  | Binary search on speed (1 to max(piles)).`canFinish(mid)`= can she eat all in h hours?              | Not binary searching on the answer space — this is the "on answer" paradigm unlock.          |
| 7  | Capacity To Ship Packages Within D Days | [LC #1011](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | 🟡 Medium  | Search on capacity (max(weights) to sum(weights)).`canShip(mid)`= greedy simulation.                | Setting wrong bounds: lo must be `max(weights)`not 0 (must fit biggest package).            |
| 8  | Find Peak Element                       | [LC #162](https://leetcode.com/problems/find-peak-element/)                        | 🟡 Medium  | If `nums[mid] < nums[mid+1]`, peak is to the right. Else it's at mid or left.                       | Checking boundary conditions — treat out-of-bounds as `-infinity`.                         |
| 9  | Time Based Key-Value Store              | [LC #981](https://leetcode.com/problems/time-based-key-value-store/)               | 🟡 Medium  | Binary search on timestamps array for largest timestamp ≤ given time.                                | Linear scan — timestamps are always increasing (insertion order), so binary search is valid. |
| 10 | Median of Two Sorted Arrays             | [LC #4](https://leetcode.com/problems/median-of-two-sorted-arrays/)                | 🔴 Hard    | Binary search on partition index of smaller array. Partition is valid when `maxLeft1 ≤ minRight2`. | Doing it in O(n) by merging — that's O(n). The constraint wants O(log(m+n)).                 |
| 11 | Aggressive Cows / Minimum Distance      | [GFG](https://www.geeksforgeeks.org/aggressive-cows/)                              | 🟡 Medium  | Search on minimum distance between cows.`canPlace(mid)`= greedy placement check.                    | Classic "binary search on answer" template problem — must-do before interviews.              |
| 12 | Split Array Largest Sum                 | [LC #410](https://leetcode.com/problems/split-array-largest-sum/)                  | 🔴 Hard    | Search on the maximum subarray sum (answer).`canSplit(mid)`= can you split into ≤ m parts?         | Treating it as DP first — DP is O(n²), binary search on answer is O(n log sum).             |
| 13 | Find K-th Smallest in Sorted Matrix     | [LC #378](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)  | 🟡 Medium  | Binary search on value range. Count elements ≤ mid — if count ≥ k, answer is in lower half.        | Using a heap — valid but O(k log n). Binary search on value is O(n log(max-min)).            |

---

---

# PATTERN #6 — Linked List (Fast/Slow Pointers + Reversal)

---

### 🧭 When to Use It

* Detecting **cycles** in a linked list
* Finding the **middle** of a list
* Problems requiring **in-place reversal** of all or part of a list
* **Merging** multiple sorted lists
* Keywords:  *"detect cycle"* ,  *"find middle"* ,  *"reverse"* ,  *"k-th from end"* , *"palindrome linked list"*

### 🧠 Core Intuition

**Fast/Slow (Floyd's):** Fast moves 2x speed of slow. If there's a cycle, fast laps slow and they meet. If no cycle, fast hits null. Slow stops at the middle when fast reaches end.
**Reversal:** Change `next` pointers one by one with a prev, curr, next dance.

### 🎯 Mental Model

> *Fast/Slow: Two runners on a circular track — the fast one always catches the slow one.*
> *Reversal: Reversing a chain of paper clips one link at a time, never losing any.*

### ⏱ Complexity Template

* **Time:** O(n) — one or two passes
* **Space:** O(1) — always in-place

---

### 📚 Problem List

| #  | Problem                                 | Link                                                                   | Difficulty | 💡 Key Insight                                                                                         | ⚠️ Common Mistake                                                                                    |
| -- | --------------------------------------- | ---------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| 1  | Reverse Linked List                     | [LC #206](https://leetcode.com/problems/reverse-linked-list/)             | 🟢 Easy    | Three-pointer dance:`prev, curr, next`. Update in right order or you lose the list.                  | Losing `next`before updating `curr.next`— always save `next = curr.next`first.                  |
| 2  | Merge Two Sorted Lists                  | [LC #21](https://leetcode.com/problems/merge-two-sorted-lists/)           | 🟢 Easy    | Dummy head + compare and stitch. Clean pointer manipulation.                                           | Forgetting to attach the remaining non-null list at the end.                                           |
| 3  | Linked List Cycle                       | [LC #141](https://leetcode.com/problems/linked-list-cycle/)               | 🟢 Easy    | Fast/slow — if they meet, cycle exists. If fast hits null, no cycle.                                  | Moving fast by 1 instead of 2 — defeats the whole point.                                              |
| 4  | Middle of Linked List                   | [LC #876](https://leetcode.com/problems/middle-of-the-linked-list/)       | 🟢 Easy    | Fast moves 2, slow moves 1. When fast hits end, slow is at middle.                                     | Off-by-one for even-length lists — check if you want first or second middle.                          |
| 5  | Linked List Cycle II (find entry point) | [LC #142](https://leetcode.com/problems/linked-list-cycle-ii/)            | 🟡 Medium  | After meeting point, reset one pointer to head. Move both at speed 1 — they meet at cycle start.      | This requires math to understand why it works — derive it once, then just memorize the 2-phase trick. |
| 6  | Palindrome Linked List                  | [LC #234](https://leetcode.com/problems/palindrome-linked-list/)          | 🟢 Easy    | Find middle, reverse second half, compare both halves from their heads.                                | Not handling odd-length lists — middle element doesn't need comparison.                               |
| 7  | Reorder List                            | [LC #143](https://leetcode.com/problems/reorder-list/)                    | 🟡 Medium  | Three steps: find middle, reverse second half, merge two halves alternately.                           | Trying to do it in one pass — breaking it into 3 clean steps is the unlock.                           |
| 8  | Remove Nth Node From End                | [LC #19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | 🟡 Medium  | Fast starts N nodes ahead. When fast hits null, slow.next is the target node.                          | Not using a dummy head — removing the actual head node becomes an edge case.                          |
| 9  | Swap Nodes in Pairs                     | [LC #24](https://leetcode.com/problems/swap-nodes-in-pairs/)              | 🟡 Medium  | Recursion: swap first two, recur on rest. Or iterative with careful pointer juggling.                  | Losing track of the "previous" node before the pair — use a dummy head.                               |
| 10 | Reverse Nodes in k-Group                | [LC #25](https://leetcode.com/problems/reverse-nodes-in-k-group/)         | 🔴 Hard    | Reverse k nodes at a time; connect tail of reversed group to result of recursive call.                 | Not checking if k nodes remain before reversing (don't reverse the last partial group).                |
| 11 | Copy List with Random Pointer           | [LC #138](https://leetcode.com/problems/copy-list-with-random-pointer/)   | 🟡 Medium  | Interleave cloned nodes into original list, set random pointers, then separate. Or use a hashmap.      | Hashmap approach is O(n) space but simpler — the interleaving trick is O(1) space.                    |
| 12 | Sort List                               | [LC #148](https://leetcode.com/problems/sort-list/)                       | 🟡 Medium  | Merge sort on linked list. Find middle (fast/slow), split, sort both, merge.                           | Using array sort (defeats the purpose) — the interviewer wants O(n log n) in-place.                   |
| 13 | Add Two Numbers                         | [LC #2](https://leetcode.com/problems/add-two-numbers/)                   | 🟡 Medium  | Simulate digit-by-digit addition with a carry variable. Handle different length lists and final carry. | Forgetting to add a new node if carry remains after both lists are exhausted.                          |
| 14 | Merge K Sorted Lists                    | [LC #23](https://leetcode.com/problems/merge-k-sorted-lists/)             | 🔴 Hard    | Min-heap of (value, list_index, node) — pop min, add its next to heap. Or divide-and-conquer merge.   | Naive merge one by one → O(kn). Heap or D&C → O(n log k).                                            |

---

---

# PATTERN #7 — Stack & Monotonic Stack

---

### 🧭 When to Use It

**Regular Stack:** Balanced parentheses, undo operations, DFS simulation, expression evaluation
**Monotonic Stack:** Next greater element, previous smaller element, histogram problems, "span" problems

* Keywords:  *"next greater"* ,  *"previous smaller"* ,  *"largest rectangle"* ,  *"valid parentheses"* , *"evaluate expression"*
* When you need to efficiently find the nearest element that is greater/smaller

### 🧠 Core Intuition

**Monotonic Stack:** Maintain a stack that is always increasing (or always decreasing). When a new element breaks the monotonicity, pop and process — the new element is the "answer" for everything it popped. You process each element at most twice (push + pop) → O(n).

### 🎯 Mental Model

> *Stack of plates at a buffet — you always take from the top (most recent). Monotonic stack: you throw away shorter plates as soon as a taller one arrives, because the tall one will always block them from being "the next greatest".*

### ⏱ Complexity Template

* **Time:** O(n) — each element pushed and popped at most once
* **Space:** O(n) — the stack

---

### 📚 Problem List

| #  | Problem                              | Link                                                                    | Difficulty | 💡 Key Insight                                                                                              | ⚠️ Common Mistake                                                                            |
| -- | ------------------------------------ | ----------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| 1  | Valid Parentheses                    | [LC #20](https://leetcode.com/problems/valid-parentheses/)                 | 🟢 Easy    | Push open brackets; on close bracket, pop and check if it matches.                                          | Not checking if stack is empty before popping — throws exception on `"]"`.                  |
| 2  | Min Stack                            | [LC #155](https://leetcode.com/problems/min-stack/)                        | 🟡 Medium  | Maintain a parallel "min stack" that tracks running minimum at each state.                                  | Storing global min — it breaks when you pop above the min element.                            |
| 3  | Evaluate Reverse Polish Notation     | [LC #150](https://leetcode.com/problems/evaluate-reverse-polish-notation/) | 🟡 Medium  | Push numbers; on operator, pop two, apply, push result.                                                     | Order of operands for `/`and `-`:`second op first`(not `first op second`).             |
| 4  | Daily Temperatures                   | [LC #739](https://leetcode.com/problems/daily-temperatures/)               | 🟡 Medium  | Monotonic decreasing stack of indices. When warmer day found, pop and record days difference.               | Storing values instead of indices — you need the index to compute the gap.                    |
| 5  | Next Greater Element I               | [LC #496](https://leetcode.com/problems/next-greater-element-i/)           | 🟢 Easy    | Monotonic stack on nums2, build a map. Then answer queries for nums1 from the map.                          | Brute-forcing each query — the stack processes nums2 once, queries are O(1) after.            |
| 6  | Next Greater Element II (circular)   | [LC #503](https://leetcode.com/problems/next-greater-element-ii/)          | 🟡 Medium  | Iterate array twice (simulate circular). Use `i % n`for index. Stack stores indices.                      | Not doubling the iteration — need to look ahead past the end of the array.                    |
| 7  | Largest Rectangle in Histogram       | [LC #84](https://leetcode.com/problems/largest-rectangle-in-histogram/)    | 🔴 Hard    | Monotonic increasing stack. Pop when current bar is shorter — popped bar's width extends to current index. | Not appending a `0`sentinel at the end — misses rectangles not terminated naturally.        |
| 8  | Trapping Rain Water (stack solution) | [LC #42](https://leetcode.com/problems/trapping-rain-water/)               | 🔴 Hard    | Stack-based: pop bottom of puddle, calculate horizontal water chunk between left wall and current bar.      | Two-pointer solution is easier — use stack only if you want to show depth.                    |
| 9  | Car Fleet                            | [LC #853](https://leetcode.com/problems/car-fleet/)                        | 🟡 Medium  | Sort by position (descending). If car's arrival time ≤ fleet ahead, it joins. Stack counts fleets.         | Not sorting by position first — the spatial order is essential.                               |
| 10 | Generate Parentheses                 | [LC #22](https://leetcode.com/problems/generate-parentheses/)              | 🟡 Medium  | Backtracking (see Pattern 14) but stack intuition helps: add `(`if open < n,`)`if close < open.         | Generating all combos then filtering — recursive generation with constraint is cleaner.       |
| 11 | Decode String                        | [LC #394](https://leetcode.com/problems/decode-string/)                    | 🟡 Medium  | Stack stores (current_string, current_multiplier). Push on `[`, pop and expand on `]`.                  | Handling multi-digit numbers (e.g.,`10[a]`) — must accumulate digits, not just take one.    |
| 12 | Remove K Digits                      | [LC #402](https://leetcode.com/problems/remove-k-digits/)                  | 🟡 Medium  | Monotonic increasing stack — pop whenever current digit < top AND k > 0. Result is the stack.              | Not removing remaining from the end when k > 0 after loop, or not stripping leading zeros.     |
| 13 | Largest Rectangle in Matrix          | [LC #85](https://leetcode.com/problems/maximal-rectangle/)                 | 🔴 Hard    | Build histogram row by row, run LC #84 on each row. Combine both solutions.                                 | Trying a 2D DP directly — it's possible but more complex. Histogram stacking is the standard. |
| 14 | Sum of Subarray Minimums             | [LC #907](https://leetcode.com/problems/sum-of-subarray-minimums/)         | 🔴 Hard    | For each element, find how many subarrays it is the minimum of using prev/next smaller element arrays.      | Not handling duplicates (use strict < on one side, ≤ on other to avoid double counting).      |

---

---

# PATTERN #8 — Queue & Monotonic Queue

---

### 🧭 When to Use It

* **Regular Queue / BFS:** Level-order traversal, shortest path in unweighted graph
* **Deque:** Sliding window maximum/minimum — O(n) instead of O(n log n) with heap
* Keywords:  *"sliding window max/min"* ,  *"level order"* ,  *"first in first out"* , *"shortest path unweighted"*

### 🧠 Core Intuition

**Monotonic Deque:** Like a monotonic stack but you also remove from the front when elements slide out of the window. Maintain a deque of indices in decreasing order (for max). Front = max of current window. Remove front if it's outside the window. Remove back if new element ≥ back (it'll never be the max while the new element exists).

### 🎯 Mental Model

> *VIP queue: When a bigger celebrity arrives, all the lesser celebrities behind them leave (they'll never be the spotlight). And when someone's been waiting too long (outside the window), they're shown the door.*

### ⏱ Complexity Template

* **Monotonic Deque:** O(n) — each element added and removed at most once
* **Space:** O(k) — at most k elements in the deque at once

---

### 📚 Problem List

| # | Problem                               | Link                                                                                            | Difficulty | 💡 Key Insight                                                                                                 | ⚠️ Common Mistake                                                                             |
| - | ------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| 1 | Number of Recent Calls                | [LC #933](https://leetcode.com/problems/number-of-recent-calls/)                                   | 🟢 Easy    | Enqueue each call; dequeue calls older than `t - 3000`. Size = answer.                                       | Using a list and counting manually — deque with popleft is O(1).                               |
| 2 | Implement Queue using Stacks          | [LC #232](https://leetcode.com/problems/implement-queue-using-stacks/)                             | 🟢 Easy    | Two stacks: push to stack1; pour to stack2 only when stack2 empty.                                             | Pouring stack1 to stack2 on every operation — amortized O(1) means only pour when needed.      |
| 3 | Sliding Window Maximum                | [LC #239](https://leetcode.com/problems/sliding-window-maximum/)                                   | 🔴 Hard    | Monotonic decreasing deque of indices. Front = max. Remove front if out of window. Pop back if ≤ new element. | Using a heap — valid but O(n log k). Deque is O(n).                                            |
| 4 | Jump Game VI                          | [LC #1696](https://leetcode.com/problems/jump-game-vi/)                                            | 🟡 Medium  | DP + monotonic deque for sliding window maximum of DP values.`dp[i] = nums[i] + max(dp[i-k..i-1])`.          | Computing max of window with brute force — defeats the O(n log n) target (deque is O(n)).      |
| 5 | Shortest Subarray with Sum at Least K | [LC #862](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)                    | 🔴 Hard    | Prefix sum + monotonic increasing deque. For each i, pop front while `prefix[i] - prefix[front] ≥ k`.       | Using standard sliding window — doesn't work with negative numbers. Deque on prefix sums does. |
| 6 | Constrained Subsequence Sum           | [LC #1425](https://leetcode.com/problems/constrained-subsequence-sum/)                             | 🔴 Hard    | DP + sliding window max over last k elements. Monotonic deque optimizes the max lookup.                        | Brute force DP is O(nk) — deque brings it to O(n).                                             |
| 7 | First Unique Character in a Stream    | [GFG variant](https://www.geeksforgeeks.org/find-first-non-repeating-character-stream-characters/) | 🟡 Medium  | Queue of candidates + frequency map. Front of queue = first non-repeating; pop if it becomes repeating.        | Scanning entire stream on each query — queue gives O(1) amortized answer.                      |
| 8 | Task Scheduler                        | [LC #621](https://leetcode.com/problems/task-scheduler/)                                           | 🟡 Medium  | Math formula:`max(n * (maxFreq - 1) + countOfMaxFreq, totalTasks)`. Queue simulation also works.             | Simulating tick-by-tick without the formula — both work but the formula is O(1).               |

---

---

> **Continue from Pattern #9** to get Trees BFS, Trees DFS, BST, Tries, Heap, Backtracking, Graphs, Union-Find, and the full DP section.

> Just say: *"Continue from Pattern 9"* and the next batch will follow in the same format.

> **Picking up right where we left off.** Same format, same density. No filler. Patterns 9–28 are where FAANG interviews actually live — Trees, Graphs, and DP separate the good candidates from the great ones.

---

# PATTERN #9 — Trees: BFS (Level Order Traversal)

---

### 🧭 When to Use It

* Problem mentions **level-by-level** processing
* You need the **shortest path** in an unweighted tree/graph
* Keywords:  *"level order"* ,  *"minimum depth"* ,  *"right side view"* ,  *"zigzag"* , *"nearest"*
* Any time you need to process nodes **layer by layer** — BFS, not DFS

### 🧠 Core Intuition

Use a queue. Start with the root. At each level, process all nodes currently in the queue, and enqueue their children. The queue naturally enforces level-by-level ordering. When you need to know which level you're on — check `len(queue)` at the start of each level iteration.

### 🎯 Mental Model

> *Ripples in a pond. Drop a stone (root) — the ripple expands outward one ring at a time. BFS processes one complete ring before moving to the next.*

### ⏱ Complexity Template

* **Time:** O(n) — every node visited exactly once
* **Space:** O(w) — w = maximum width of the tree (worst case O(n) for a complete tree)

---

### 📚 Problem List

| #  | Problem                               | Link                                                                               | Difficulty | 💡 Key Insight                                                                                                  | ⚠️ Common Mistake                                                                                           |
| -- | ------------------------------------- | ---------------------------------------------------------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| 1  | Binary Tree Level Order Traversal     | [LC #102](https://leetcode.com/problems/binary-tree-level-order-traversal/)           | 🟢 Easy    | Snapshot `level_size = len(queue)`at start of each level. Process exactly that many nodes.                    | Using a single while loop without tracking level size — you lose level boundaries.                           |
| 2  | Binary Tree Right Side View           | [LC #199](https://leetcode.com/problems/binary-tree-right-side-view/)                 | 🟡 Medium  | BFS level by level. Last node processed at each level = visible from right.                                     | Using DFS (works, but BFS is more intuitive and direct for this problem).                                     |
| 3  | Average of Levels in Binary Tree      | [LC #637](https://leetcode.com/problems/average-of-levels-in-binary-tree/)            | 🟢 Easy    | Sum all nodes in a level, divide by `level_size`. Straightforward BFS.                                        | Integer overflow when summing large values — use long/float accumulator.                                     |
| 4  | Minimum Depth of Binary Tree          | [LC #111](https://leetcode.com/problems/minimum-depth-of-binary-tree/)                | 🟢 Easy    | BFS: first leaf node encountered = minimum depth. Return immediately — no need to explore all levels.          | Using DFS and taking `min(left, right)`— mishandles nodes with one null child (not a leaf).                |
| 5  | Binary Tree Zigzag Level Order        | [LC #103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)    | 🟡 Medium  | BFS + direction flag. Odd levels → left-to-right; even → right-to-left (reverse the level list).              | Changing queue insertion order — just reverse the result list, don't complicate the BFS.                     |
| 6  | Populating Next Right Pointers        | [LC #116](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) | 🟡 Medium  | BFS level by level. Connect each node to the next in the queue. Last node at each level points to null.         | O(n) space BFS is easy — the follow-up wants O(1) space using the `next`pointers themselves.               |
| 7  | Binary Tree Level Order Traversal II  | [LC #107](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)        | 🟢 Easy    | Standard BFS, then reverse the result list at the end.                                                          | Reversing the insertion logic — just reverse the output, keep BFS clean.                                     |
| 8  | Find Largest Value in Each Tree Row   | [LC #515](https://leetcode.com/problems/find-largest-value-in-each-tree-row/)         | 🟡 Medium  | BFS, track max per level. Direct application of the template.                                                   | Overcomplicating — this is the template with a `max()`call inside.                                         |
| 9  | Maximum Width of Binary Tree          | [LC #662](https://leetcode.com/problems/maximum-width-of-binary-tree/)                | 🟡 Medium  | Assign indices: root=1, left child = 2*i, right child = 2*i+1. Width = rightmost index − leftmost index + 1. | Not normalizing indices per level — they can overflow for deep trees. Subtract leftmost index at each level. |
| 10 | Word Ladder                           | [LC #127](https://leetcode.com/problems/word-ladder/)                                 | 🔴 Hard    | BFS on word graph. Neighbors = words differing by 1 letter. BFS guarantees shortest path.                       | DFS — finds a path but not guaranteed shortest. BFS is mandatory for shortest path.                          |
| 11 | Rotting Oranges                       | [LC #994](https://leetcode.com/problems/rotting-oranges/)                             | 🟡 Medium  | Multi-source BFS — start with all rotten oranges simultaneously in the queue.                                  | Single-source BFS from one orange — multi-source is the key because all rot simultaneously.                  |
| 12 | Walls and Gates                       | [LC #286](https://leetcode.com/problems/walls-and-gates/)                             | 🟡 Medium  | Multi-source BFS starting from all gates at once. Fill distances outward.                                       | BFS from each gate separately — O(m*n*gates). Multi-source is O(m*n).                                      |
| 13 | Serialize and Deserialize Binary Tree | [LC #297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)       | 🔴 Hard    | BFS serialization: encode null as "N". On deserialize, use a queue to match parents with children.              | Using DFS (works but slightly trickier to handle null children during deserialization).                       |

---

---

# PATTERN #10 — Trees: DFS (Pre/In/Post Order)

---

### 🧭 When to Use It

* You need to process **parent before children** (preorder), **sorted order for BST** (inorder), or **children before parent** (postorder)
* Computing values that **depend on subtree results** (heights, diameters, path sums)
* Keywords:  *"path sum"* ,  *"diameter"* ,  *"lowest common ancestor"* ,  *"max depth"* , *"subtree"*
* When the answer to a node's problem = combining answers from its children

### 🧠 Core Intuition

DFS on trees is almost always recursion. The key discipline:  **decide what your function returns** , and trust that the recursive call returns the correct answer for each subtree. You only need to handle the current node + combine children's results.

**The golden rule:** If you find yourself writing complex loop logic in a tree problem — you're probably missing a cleaner recursive formulation.

### 🎯 Mental Model

> *Exploring a cave system. You go as deep as possible down one tunnel, then backtrack and try the next. You can't see ahead — but you trust your map (recursion) to chart unexplored tunnels.*

### ⏱ Complexity Template

* **Time:** O(n) — visit every node once
* **Space:** O(h) — call stack depth = tree height. O(log n) balanced, O(n) skewed

---

### 📚 Problem List

| #  | Problem                            | Link                                                                                             | Difficulty | 💡 Key Insight                                                                                                                | ⚠️ Common Mistake                                                                                    |
| -- | ---------------------------------- | ------------------------------------------------------------------------------------------------ | ---------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 1  | Maximum Depth of Binary Tree       | [LC #104](https://leetcode.com/problems/maximum-depth-of-binary-tree/)                              | 🟢 Easy    | `return 1 + max(dfs(left), dfs(right))`. Base case: null node returns 0.                                                    | Off-by-one on depth counting. Base case must return 0, not 1.                                          |
| 2  | Invert Binary Tree                 | [LC #226](https://leetcode.com/problems/invert-binary-tree/)                                        | 🟢 Easy    | Swap left and right children, then recurse on both. Three lines of code.                                                      | Swapping values instead of nodes — swap the child pointers.                                           |
| 3  | Same Tree                          | [LC #100](https://leetcode.com/problems/same-tree/)                                                 | 🟢 Easy    | Recurse: nodes are same if values equal AND left subtrees same AND right subtrees same.                                       | Not handling both-null and one-null cases at the top of the function.                                  |
| 4  | Subtree of Another Tree            | [LC #572](https://leetcode.com/problems/subtree-of-another-tree/)                                   | 🟢 Easy    | At each node in `root`, check if `isSameTree(root, subRoot)`. Recurse if not.                                             | Checking only structural match without value match — both structure and values must match.            |
| 5  | Diameter of Binary Tree            | [LC #543](https://leetcode.com/problems/diameter-of-binary-tree/)                                   | 🟢 Easy    | At each node: diameter = leftHeight + rightHeight. Track global max. Return height to parent.                                 | Returning diameter instead of height from helper — parent needs height to compute its diameter.       |
| 6  | Path Sum II                        | [LC #113](https://leetcode.com/problems/path-sum-ii/)                                               | 🟡 Medium  | DFS with running sum. On reaching leaf with correct sum, snapshot current path. Backtrack on return.                          | Not backtracking (removing the last element) before returning — path gets polluted.                   |
| 7  | Binary Tree Maximum Path Sum       | [LC #124](https://leetcode.com/problems/binary-tree-maximum-path-sum/)                              | 🔴 Hard    | At each node: local max = node + max(0, leftGain) + max(0, rightGain). But return only one branch to parent.                  | Returning both branches to the parent — a path can only pass through one side when continuing upward. |
| 8  | Lowest Common Ancestor             | [LC #236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)                   | 🟡 Medium  | If current node is p or q, return it. LCA is the node where one target comes from each subtree.                               | Writing iterative solutions with parent pointers — recursive is far cleaner and more readable.        |
| 9  | Count Good Nodes                   | [LC #1448](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)                          | 🟡 Medium  | DFS passing the running max from root. Node is "good" if its value ≥ max seen on path so far.                                | Not passing the running max down — each call needs to know the max above it.                          |
| 10 | Balanced Binary Tree               | [LC #110](https://leetcode.com/problems/balanced-binary-tree/)                                      | 🟢 Easy    | Return -1 from any subtree that's already unbalanced. Parent propagates -1 upward.                                            | Calling `height()`separately — that's O(n²). The -1 sentinel trick makes it O(n).                  |
| 11 | Flatten Binary Tree to Linked List | [LC #114](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)                        | 🟡 Medium  | Postorder: flatten left, flatten right, attach left chain before right chain. Use rightmost of left chain as bridge.          | Top-down approaches lose the right subtree — postorder (bottom-up) is safe.                           |
| 12 | Construct Tree from Pre/Inorder    | [LC #105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | 🟡 Medium  | Preorder[0] = root. Find it in inorder → splits into left and right subtrees. Recurse.                                       | Linear search in inorder for each node → O(n²). Use a hashmap for O(1) lookup → O(n).               |
| 13 | Path Sum III                       | [LC #437](https://leetcode.com/problems/path-sum-iii/)                                              | 🟡 Medium  | DFS + prefix sum map (Pattern 4 in a tree!). Count paths ending at current node using `prefixSum - target`.                 | Brute-forcing every path from every node — O(n²). Prefix sum on tree paths is O(n).                  |
| 14 | Binary Tree Cameras                | [LC #968](https://leetcode.com/problems/binary-tree-cameras/)                                       | 🔴 Hard    | Greedy postorder DFS. Each node has 3 states: covered, has camera, not covered. Place cameras at parents of uncovered leaves. | Placing cameras at leaves — placing at their parents covers more nodes per camera.                    |

---

---

# PATTERN #11 — Binary Search Tree (BST)

---

### 🧭 When to Use It

* Tree is explicitly a **BST** (left < root < right)
* You need **in-order traversal** (gives sorted order)
* **Searching, inserting, or deleting** in a sorted tree structure
* Keywords:  *"kth smallest"* ,  *"validate BST"* ,  *"BST iterator"* , *"inorder successor"*

### 🧠 Core Intuition

A BST is a sorted array folded into a tree. **Inorder traversal = sorted order** — this is the single most important BST fact. For search/insert/delete, you eliminate half the tree at each node (like binary search). The BST property (`left < node < right`) must hold  **globally** , not just locally.

### 🎯 Mental Model

> *A perfectly organized filing cabinet. Every folder to the left is alphabetically before the label, every folder to the right comes after. You never need to search the whole cabinet — just follow the labels.*

### ⏱ Complexity Template

* **Time:** O(h) for search/insert/delete — O(log n) balanced, O(n) skewed
* **In-order traversal:** O(n)
* **Space:** O(h) for recursion stack

---

### 📚 Problem List

| #  | Problem                         | Link                                                                                  | Difficulty | 💡 Key Insight                                                                                                                                                  | ⚠️ Common Mistake                                                                                              |
| -- | ------------------------------- | ------------------------------------------------------------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| 1  | Search in BST                   | [LC #700](https://leetcode.com/problems/search-in-a-binary-search-tree/)                 | 🟢 Easy    | If val < node → go left; if val > node → go right; if equal → return node.                                                                                   | Traversing both subtrees — BST eliminates one side at every step.                                               |
| 2  | Insert into a BST               | [LC #701](https://leetcode.com/problems/insert-into-a-binary-search-tree/)               | 🟡 Medium  | Navigate to the null position where val belongs; insert there. Return node to rebuild the path.                                                                 | Forgetting to return the node — recursive insert must return `root`to reattach modified subtree.              |
| 3  | Validate Binary Search Tree     | [LC #98](https://leetcode.com/problems/validate-binary-search-tree/)                     | 🟡 Medium  | Pass min/max bounds down: left subtree max = current node, right subtree min = current node.                                                                    | Checking only `left.val < root.val`locally — the BST property is global, not just parent-child.               |
| 4  | Kth Smallest in BST             | [LC #230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)                  | 🟡 Medium  | Inorder traversal = sorted order. Decrement a counter; return value when counter hits 0.                                                                        | Collecting all inorder values then indexing — wastes memory. Stop early when k reaches 0.                       |
| 5  | Lowest Common Ancestor of BST   | [LC #235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | 🟢 Easy    | If both p,q < node → LCA is in left; if both > node → right; else current node is LCA.                                                                        | Using the general tree LCA algorithm (Pattern 10 #8) — BST property makes this O(log n), not O(n).              |
| 6  | BST Iterator                    | [LC #173](https://leetcode.com/problems/binary-search-tree-iterator/)                    | 🟡 Medium  | Use an explicit stack simulating iterative inorder. Push all left children on init; on `next()`, pop and push right subtree's lefts.                          | Storing all inorder values upfront — O(n) space. Stack approach is O(h) space.                                  |
| 7  | Delete Node in BST              | [LC #450](https://leetcode.com/problems/delete-node-in-a-bst/)                           | 🟡 Medium  | 3 cases: no children (delete), one child (replace), two children (replace with inorder successor, then delete successor).                                       | Handling the two-children case — find the leftmost node in the right subtree (inorder successor).               |
| 8  | Convert Sorted Array to BST     | [LC #108](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)     | 🟢 Easy    | Mid of array = root. Recurse on left half for left subtree, right half for right subtree.                                                                       | Off-by-one on mid —`mid = (lo + hi) // 2`is standard.                                                         |
| 9  | Recover BST (two nodes swapped) | [LC #99](https://leetcode.com/problems/recover-binary-search-tree/)                      | 🔴 Hard    | Inorder traversal — two nodes are swapped when inorder sequence has anomalies. First anomaly's first node + second anomaly's second node are the swapped pair. | Finding only one anomaly — you must track both the first AND second violation to correctly identify both nodes. |
| 10 | Two Sum IV (BST)                | [LC #653](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)                      | 🟢 Easy    | Inorder to get sorted array, then two pointers. Or: DFS + HashSet (faster to implement).                                                                        | Trying BST-specific traversal — inorder + two pointers or DFS + set are both fine.                              |
| 11 | Trim a BST                      | [LC #669](https://leetcode.com/problems/trim-a-binary-search-tree/)                      | 🟡 Medium  | If node < lo → return `trim(right)`; if node > hi → return `trim(left)`; else trim both children.                                                         | Not using the BST property to skip entire subtrees — whole left subtree can be discarded if root < lo.          |
| 12 | All Elements in Two BSTs        | [LC #1305](https://leetcode.com/problems/all-elements-in-two-binary-search-trees/)       | 🟡 Medium  | Inorder each BST (gets sorted lists), then merge two sorted arrays. O(m+n) merge.                                                                               | Building one big list and sorting — O((m+n) log(m+n)). Merge of two sorted lists is O(m+n).                     |

---

---

# PATTERN #12 — Tries (Prefix Trees)

---

### 🧭 When to Use It

* Dealing with a large set of **strings and prefix queries**
* Auto-complete, spell-check, or **IP routing** type problems
* You need to find all strings with a given **prefix** efficiently
* Keywords:  *"prefix"* ,  *"dictionary"* ,  *"word search"* ,  *"autocomplete"* , *"starts with"*

### 🧠 Core Intuition

A Trie stores strings by their characters, sharing common prefixes. Each node represents one character. Children are the next possible characters. A Trie of n words with average length L uses O(n*L) space but gives O(L) lookup — far better than O(n*L) for set-based prefix search.

### 🎯 Mental Model

> *A phone book tree. Everyone with the surname starting "Sm" shares the first branch, "Smi" shares the next, "Smith" terminates one path — it's a common-prefix highway system.*

### ⏱ Complexity Template

* **Insert/Search:** O(L) where L = length of the word
* **Space:** O(n * L * 26) worst case — n words, L length, 26 children per node

---

### 📚 Problem List

| #  | Problem                             | Link                                                                              | Difficulty | 💡 Key Insight                                                                                                                  | ⚠️ Common Mistake                                                                                                               |
| -- | ----------------------------------- | --------------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| 1  | Implement Trie                      | [LC #208](https://leetcode.com/problems/implement-trie-prefix-tree/)                 | 🟡 Medium  | Each node:`children[26]`array (or dict) +`isEndOfWord`flag. Insert char-by-char, creating nodes as needed.                  | Using a dict of full strings — defeats the purpose. The shared-prefix structure is the whole point.                              |
| 2  | Design Add and Search Words         | [LC #211](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | 🟡 Medium  | Trie + recursive search with `.`wildcard: at `.`, try all 26 children.                                                      | Iterative search with wildcards — recursion handles branching naturally.                                                         |
| 3  | Word Search II                      | [LC #212](https://leetcode.com/problems/word-search-ii/)                             | 🔴 Hard    | Build Trie from words. DFS on board, walk Trie simultaneously. Cut DFS branch when no Trie match.                               | Running DFS for each word separately — O(words * board). Trie prunes early, making it O(board * Trie depth).                     |
| 4  | Replace Words                       | [LC #648](https://leetcode.com/problems/replace-words/)                              | 🟡 Medium  | Build Trie from roots. For each word in sentence, walk Trie until `isEnd`is found — replace with that prefix.                | Using `startsWith`with a list — O(roots * sentence_length). Trie is O(sentence_length).                                        |
| 5  | Map Sum Pairs                       | [LC #677](https://leetcode.com/problems/map-sum-pairs/)                              | 🟡 Medium  | Store value in Trie node at end of word. For `sum(prefix)`, walk to prefix node, then DFS-sum all terminal values below.      | Scanning all keys with `startsWith`— O(n). Trie gives O(prefix_len + output).                                                  |
| 6  | Longest Common Prefix               | [LC #14](https://leetcode.com/problems/longest-common-prefix/)                       | 🟢 Easy    | Trie approach: insert all strings, traverse the single-child chain from root. Stops at branch or `isEnd`.                     | Sorting and comparing first/last strings — valid O(n log n) but Trie is more instructive here.                                   |
| 7  | Word Break                          | [LC #139](https://leetcode.com/problems/word-break/)                                 | 🟡 Medium  | DP with Trie:`dp[i]`= can string[0..i] be segmented. At each i, walk Trie forward to find valid dictionary words ending here. | Pure DP with set lookup — also O(n²) but Trie makes prefix pruning explicit.                                                    |
| 8  | Maximum XOR of Two Numbers in Array | [LC #421](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)     | 🟡 Medium  | Binary Trie of all numbers. For each number, greedily pick the opposite bit at each level to maximize XOR.                      | Brute forcing all pairs — O(n²). Binary Trie makes this O(n * 32).                                                              |
| 9  | Palindrome Pairs                    | [LC #336](https://leetcode.com/problems/palindrome-pairs/)                           | 🔴 Hard    | Reversed Trie: for each word, find its reverse prefix and suffix palindrome cases. Three sub-cases to handle.                   | This is genuinely hard. If you solve it clean, you're in the top 10% of candidates.                                               |
| 10 | Stream of Characters                | [LC #1032](https://leetcode.com/problems/stream-of-characters/)                      | 🔴 Hard    | Build Trie from reversed words. Maintain a list of active Trie nodes. On each character, advance all active nodes.              | Forward Trie on streaming input — you don't know where the word starts, so reverse-Trie from current char backward is the trick. |

---

---

# PATTERN #13 — Heap / Priority Queue

---

### 🧭 When to Use It

* You need the **k-th largest/smallest** element repeatedly
* **Streaming data** where you need running median or extremes
* Greedy problems where you always process the **minimum or maximum** cost next
* Keywords:  *"k largest"* ,  *"k smallest"* ,  *"merge k sorted"* ,  *"median stream"* , *"task scheduling"*

### 🧠 Core Intuition

A heap gives you O(1) access to the min (or max) and O(log n) insert/delete. When you need to  **repeatedly extract the best element** , heap beats sorting (which is O(n log n) upfront for all elements). A min-heap of size k maintains the k largest elements seen so far — the top is the k-th largest.

### 🎯 Mental Model

> *A tournament bracket that always keeps the strongest player at the top. When someone new enters, the bracket re-organizes in O(log n) — not O(n).*

### ⏱ Complexity Template

* **Push/Pop:** O(log n)
* **Peek min/max:** O(1)
* **Build heap from array:** O(n) — not O(n log n), this surprises people
* **K-th element problems:** O(n log k)

---

### 📚 Problem List

| #  | Problem                         | Link                                                                                 | Difficulty | 💡 Key Insight                                                                                                              | ⚠️ Common Mistake                                                                                                    |
| -- | ------------------------------- | ------------------------------------------------------------------------------------ | ---------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 1  | Kth Largest Element in Array    | [LC #215](https://leetcode.com/problems/kth-largest-element-in-an-array/)               | 🟡 Medium  | Min-heap of size k. Push each element; if size > k, pop. Top of heap = k-th largest.                                        | Sorting everything — O(n log n). Min-heap of size k is O(n log k). Know QuickSelect too (O(n) avg).                   |
| 2  | Last Stone Weight               | [LC #1046](https://leetcode.com/problems/last-stone-weight/)                            | 🟢 Easy    | Max-heap. Repeatedly smash two heaviest stones. Push remainder if nonzero.                                                  | Using a sorted list and re-sorting each time — O(n² log n). Heap is O(n log n).                                      |
| 3  | K Closest Points to Origin      | [LC #973](https://leetcode.com/problems/k-closest-points-to-origin/)                    | 🟡 Medium  | Max-heap of size k by distance. Pop when size > k. Remaining = k closest. No need to sort.                                  | Computing actual distance (uses sqrt) — compare `x² + y²`directly to avoid floating point.                        |
| 4  | Task Scheduler                  | [LC #621](https://leetcode.com/problems/task-scheduler/)                                | 🟡 Medium  | Max-heap by frequency + a cooldown queue. Greedily schedule the most frequent available task.                               | Trying to simulate tick-by-tick without a queue — the cooldown queue tracks when each task becomes available again.   |
| 5  | Design Twitter                  | [LC #355](https://leetcode.com/problems/design-twitter/)                                | 🟡 Medium  | Per-user tweet list. On `getNewsFeed`: merge k sorted lists (followees' tweets) using a heap.                             | Merging all tweets into one list — O(total tweets). Heap merge is O(k log k) where k = followees.                     |
| 6  | Find Median from Data Stream    | [LC #295](https://leetcode.com/problems/find-median-from-data-stream/)                  | 🔴 Hard    | Two heaps: max-heap for lower half, min-heap for upper half. Balance sizes ±1. Median = top(s) or average of tops.         | One heap — you can't efficiently access both ends. Two heaps is the unlock.                                           |
| 7  | Merge K Sorted Lists            | [LC #23](https://leetcode.com/problems/merge-k-sorted-lists/)                           | 🔴 Hard    | Min-heap of (value, list_id, node). Pop min, push its next. Repeat until heap empty.                                        | Merging lists one-by-one — O(nk). Heap merge is O(n log k).                                                           |
| 8  | Kth Smallest in Sorted Matrix   | [LC #378](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)       | 🟡 Medium  | Min-heap starting with first column. Pop min, push the element below it. After k pops, answer is found.                     | Pushing all elements into heap — O(n²). Heap on just the frontier is O(k log n).                                     |
| 9  | IPO (Maximize Capital)          | [LC #502](https://leetcode.com/problems/ipo/)                                           | 🔴 Hard    | Min-heap by capital (unlock projects we can afford). Max-heap by profit (pick best available). Alternate between them.      | Trying to pick the best overall project each round without checking affordability — two heaps enforce the constraint. |
| 10 | Reorganize String               | [LC #767](https://leetcode.com/problems/reorganize-string/)                             | 🟡 Medium  | Max-heap by frequency. Always place most frequent char, then second most. Alternate greedily.                               | Placing chars without checking the previous char — adjacent duplicates are the constraint to enforce.                 |
| 11 | Smallest Range Covering K Lists | [LC #632](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) | 🔴 Hard    | Min-heap of current element from each list. Range = [min (heap top), running max]. Advance the min list.                    | Not tracking the running max — max can only be updated when you push a new element.                                   |
| 12 | Meeting Rooms II                | [LC #253](https://leetcode.com/problems/meeting-rooms-ii/)                              | 🟡 Medium  | Min-heap of end times. Sort meetings by start. If earliest end ≤ current start, reuse room (pop). Always push current end. | Counting overlaps naively — heap size at any point = rooms in use. Final size = answer.                               |
| 13 | Sliding Window Median           | [LC #480](https://leetcode.com/problems/sliding-window-median/)                         | 🔴 Hard    | Two heaps (like #6) + a lazy deletion strategy for elements leaving the window.                                             | Rebuilding heaps on each window slide — O(n²). Lazy deletion with a counter map gives O(n log k).                    |

---

---

# PATTERN #14 — Backtracking

---

### 🧭 When to Use It

* You need to **generate all possible** combinations, permutations, or subsets
* Problem asks for  **all solutions** , not just one optimal
* You explore a decision tree and **prune invalid branches early**
* Keywords:  *"all combinations"* ,  *"all permutations"* ,  *"generate"* ,  *"all subsets"* ,  *"word search"* , *"N-Queens"*

### 🧠 Core Intuition

Backtracking = DFS on a decision tree + pruning. At each step,  **make a choice** , recurse to explore that branch, then **undo the choice** (backtrack) and try the next option. The "undo" step is what makes it backtracking, not just DFS. Prune branches where the constraint is already violated — don't go deeper if you know it's wrong.

**The 3-part template every backtracking solution follows:**

1. **Choose** — pick an option
2. **Explore** — recurse
3. **Un-choose** — undo the pick

### 🎯 Mental Model

> *A lock with 4 dials. You try each digit on dial 1, then for each, try all digits on dial 2, and so on. If a partial combo is already invalid (locked out), you skip that entire branch without trying the remaining dials.*

### ⏱ Complexity Template

* **Subsets:** O(2ⁿ) — each element in or out
* **Permutations:** O(n!) — n choices for first, n-1 for second, etc.
* **Combinations:** O(C(n,k) * k)
* **Space:** O(n) recursion depth + O(result size)

---

### 📚 Problem List

| #  | Problem                             | Link                                                                        | Difficulty | 💡 Key Insight                                                                                                            | ⚠️ Common Mistake                                                                                                           |
| -- | ----------------------------------- | --------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 1  | Subsets                             | [LC #78](https://leetcode.com/problems/subsets/)                               | 🟡 Medium  | At each index, two choices: include or exclude. Or: iterate and at each step add current element to all existing subsets. | Forgetting to add the empty subset (it's always included).                                                                    |
| 2  | Combination Sum                     | [LC #39](https://leetcode.com/problems/combination-sum/)                       | 🟡 Medium  | Recurse from current index (allow reuse). Subtract from target; if 0 → found. If negative → prune.                      | Starting from index 0 each time (creates duplicates). Pass current index to allow reuse without revisiting previous elements. |
| 3  | Combination Sum II                  | [LC #40](https://leetcode.com/problems/combination-sum-ii/)                    | 🟡 Medium  | Sort first. Skip duplicates at the same recursion level:`if i > start and nums[i] == nums[i-1]: continue`.              | Not sorting first — deduplication only works on sorted input.                                                                |
| 4  | Permutations                        | [LC #46](https://leetcode.com/problems/permutations/)                          | 🟡 Medium  | Swap elements with current position, recurse, then swap back. Or use a `used[]`boolean array.                           | Not undoing the swap on backtrack — the array gets corrupted for subsequent branches.                                        |
| 5  | Permutations II (with duplicates)   | [LC #47](https://leetcode.com/problems/permutations-ii/)                       | 🟡 Medium  | Sort. Skip `nums[i]`if same as `nums[i-1]`AND `nums[i-1]`was not used (same recursion level).                       | Skipping when `nums[i-1]`was used (opposite of what you want) — the condition is subtle, derive it once carefully.         |
| 6  | Subsets II                          | [LC #90](https://leetcode.com/problems/subsets-ii/)                            | 🟡 Medium  | Sort. Skip element if it's the same as previous at the same recursion depth.                                              | Same dedup pattern as Combination Sum II — if `i > start and nums[i] == nums[i-1]`, skip.                                  |
| 7  | Word Search                         | [LC #79](https://leetcode.com/problems/word-search/)                           | 🟡 Medium  | DFS on grid. Mark cell as visited (in-place with `#`), recurse in 4 directions, unmark on return.                       | Not unmarking the cell on backtrack — subsequent paths see the cell as unavailable.                                          |
| 8  | Palindrome Partitioning             | [LC #131](https://leetcode.com/problems/palindrome-partitioning/)              | 🟡 Medium  | At each index, try all substrings starting here. If palindrome → add to path, recurse on remainder.                      | Checking palindrome with a helper inside backtracking — precompute palindrome table with DP for efficiency.                  |
| 9  | Letter Combinations of Phone Number | [LC #17](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) | 🟡 Medium  | At each digit, try all its mapped letters. Recurse on the next digit. Add to result when path length = input length.      | Building all combinations with nested loops — clean backtracking scales to any number of digits.                             |
| 10 | N-Queens                            | [LC #51](https://leetcode.com/problems/n-queens/)                              | 🔴 Hard    | One queen per row. Track attacked columns and both diagonals (sets). Place queen if column/diagonals clear.               | Checking the whole board for conflicts each time — O(n) per check. Using sets for cols/diags is O(1) per check.              |
| 11 | Sudoku Solver                       | [LC #37](https://leetcode.com/problems/sudoku-solver/)                         | 🔴 Hard    | Try digits 1–9 in each empty cell. Prune if digit already in same row, col, or 3×3 box. Backtrack if stuck.             | Not preallocating sets for rows/cols/boxes — recomputing valid digits each time is O(n) instead of O(1).                     |
| 12 | Expression Add Operators            | [LC #282](https://leetcode.com/problems/expression-add-operators/)             | 🔴 Hard    | Backtrack on every possible split. Track running value AND last operand (for multiplication precedence).                  | Not tracking the last operand —`a + b * c`requires knowing `b`to undo when multiplying by `c`.                         |
| 13 | Generate Parentheses                | [LC #22](https://leetcode.com/problems/generate-parentheses/)                  | 🟡 Medium  | Add `(`if `open < n`. Add `)`if `close < open`. Base:`open == close == n`.                                      | Generating all strings of 2n characters and filtering — exponential wasted work. Prune during generation.                    |

---

---

# PATTERN #15 — Graphs: BFS & DFS

---

### 🧭 When to Use It

* **BFS:** Shortest path in **unweighted** graph, connected components level-by-level, multi-source spreading
* **DFS:** Connected components, cycle detection, topological sort, flood fill, island counting
* Keywords:  *"number of islands"* ,  *"connected components"* ,  *"shortest path"* ,  *"clone graph"* , *"course schedule"*
* If grid problem with 4-directional movement → almost always BFS or DFS

### 🧠 Core Intuition

Build the graph (adjacency list). Mark nodes visited. BFS uses a queue (shortest path guarantee). DFS uses recursion or a stack (explores fully before backtracking). The visited set is non-negotiable — without it, you loop forever.

**The real skill:** Recognizing that a grid IS a graph (each cell is a node, edges go to 4 neighbors).

### 🎯 Mental Model

> *BFS: Ink dropped in water — spreads to all neighbors simultaneously, ring by ring.*
> *DFS: A mouse in a maze — picks one tunnel, goes all the way to the end before trying another.*

### ⏱ Complexity Template

* **Time:** O(V + E) — vertices + edges
* **Space:** O(V) — visited set + queue/stack
* **Grid graph:** O(m * n) time and space

---

### 📚 Problem List

| #  | Problem                        | Link                                                                                         | Difficulty | 💡 Key Insight                                                                                                                                              | ⚠️ Common Mistake                                                                                                   |
| -- | ------------------------------ | -------------------------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 1  | Number of Islands              | [LC #200](https://leetcode.com/problems/number-of-islands/)                                     | 🟡 Medium  | DFS/BFS from each unvisited `'1'`. Sink visited cells (mark `'0'`). Count DFS initiations.                                                              | Not sinking/marking cells — you revisit them and double-count islands.                                               |
| 2  | Clone Graph                    | [LC #133](https://leetcode.com/problems/clone-graph/)                                           | 🟡 Medium  | BFS/DFS + hashmap `{original → clone}`. On visiting a neighbor: if not in map, create clone and add to map.                                              | Not using a map — you create multiple clones of the same node, breaking the graph structure.                         |
| 3  | Max Area of Island             | [LC #695](https://leetcode.com/problems/max-area-of-island/)                                    | 🟡 Medium  | DFS returns size of island. Track running max. Classic DFS on grid.                                                                                         | Counting cells without sinking — re-counts cells already visited in the DFS.                                         |
| 4  | Pacific Atlantic Water Flow    | [LC #417](https://leetcode.com/problems/pacific-atlantic-water-flow/)                           | 🟡 Medium  | Reverse flow: BFS/DFS from ocean borders inward (uphill). Cells reachable from both oceans = answer.                                                        | Forward simulation from every cell — O(m²n²). Reverse BFS from borders is O(mn).                                   |
| 5  | Surrounded Regions             | [LC #130](https://leetcode.com/problems/surrounded-regions/)                                    | 🟡 Medium  | DFS from all border `'O'`s, mark them safe. Flip remaining `'O'`s to `'X'`, revert safe marks.                                                        | Trying to determine if each `'O'`is surrounded from scratch — border-DFS inverts the problem cleanly.              |
| 6  | Walls and Gates                | [LC #286](https://leetcode.com/problems/walls-and-gates/)                                       | 🟡 Medium  | Multi-source BFS from all gates simultaneously. Distance fills outward.                                                                                     | BFS from each gate separately — redundant work. Multi-source is one BFS pass.                                        |
| 7  | Shortest Path in Binary Matrix | [LC #1091](https://leetcode.com/problems/shortest-path-in-binary-matrix/)                       | 🟡 Medium  | BFS with 8-directional movement. First time you reach destination = shortest path.                                                                          | Using DFS — doesn't guarantee shortest path. BFS is mandatory here.                                                  |
| 8  | Redundant Connection           | [LC #684](https://leetcode.com/problems/redundant-connection/)                                  | 🟡 Medium  | Process edges; add each to graph. If both endpoints already connected (BFS/DFS check) → this edge is redundant. (Union-Find is cleaner — see Pattern 17.) | Checking connectivity with DFS on the full graph — O(n) per edge = O(n²). Union-Find is O(α(n)) per edge.          |
| 9  | Course Schedule                | [LC #207](https://leetcode.com/problems/course-schedule/)                                       | 🟡 Medium  | Detect cycle in directed graph using DFS with 3-color marking: unvisited, in-progress, done. Cycle = prerequisite loop.                                     | Using visited as a boolean — you need 3 states to detect back edges (in-progress is the key).                        |
| 10 | Number of Connected Components | [LC #323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) | 🟡 Medium  | DFS/BFS from each unvisited node, mark all reachable. Count DFS initiations. (Union-Find is also perfect here.)                                             | Not tracking visited globally — re-visiting nodes and overcounting components.                                       |
| 11 | Graph Valid Tree               | [LC #261](https://leetcode.com/problems/graph-valid-tree/)                                      | 🟡 Medium  | Valid tree = connected + no cycles. DFS: cycle detected if visited node is not the parent. Also check all nodes visited.                                    | Forgetting the "connected" check — no cycles alone is not sufficient for a tree.                                     |
| 12 | Open the Lock                  | [LC #752](https://leetcode.com/problems/open-the-lock/)                                         | 🟡 Medium  | BFS on lock states (strings). Each state has 8 neighbors (±1 on each of 4 wheels). Shortest path to target.                                                | DFS — doesn't give shortest number of turns. BFS guarantees minimum turns.                                           |
| 13 | Word Ladder                    | [LC #127](https://leetcode.com/problems/word-ladder/)                                           | 🔴 Hard    | BFS on word states. Generate neighbors by changing one letter at a time. Bidirectional BFS cuts search space significantly.                                 | Generating neighbors by scanning the whole word list — O(n * L). Instead, generate all 26 variants of each position. |
| 14 | Alien Dictionary               | [LC #269](https://leetcode.com/problems/alien-dictionary/)                                      | 🔴 Hard    | Build character ordering graph from adjacent word comparisons. Topological sort = alien alphabet.                                                           | Not handling the case where a longer word comes before its prefix — that's invalid input.                            |

---

---

# PATTERN #16 — Graphs: Topological Sort

---

### 🧭 When to Use It

* Problems involving **ordering with dependencies** (do A before B)
* Detecting **cycles in directed graphs**
* Keywords:  *"prerequisites"* ,  *"build order"* ,  *"course schedule"* ,  *"dependency"* , *"ordering tasks"*
* Any problem where you have directed edges meaning "X must come before Y"

### 🧠 Core Intuition

Topological sort orders nodes so every directed edge goes from earlier to later in the sequence. Two approaches: **Kahn's Algorithm** (BFS-based, uses in-degree counts — nodes with in-degree 0 are ready to process) or **DFS-based** (post-order reversal). If a cycle exists, topological sort is impossible — use this to detect cycles.

### 🎯 Mental Model

> *Getting dressed in the morning. You can't put on shoes before socks. Build a dependency graph, then process items in the order that respects all "must come before" rules.*

### ⏱ Complexity Template

* **Time:** O(V + E)
* **Space:** O(V + E) for the graph + O(V) for in-degree array

---

### 📚 Problem List

| # | Problem                   | Link                                                                                  | Difficulty | 💡 Key Insight                                                                                                   | ⚠️ Common Mistake                                                                                                        |
| - | ------------------------- | ------------------------------------------------------------------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 1 | Course Schedule           | [LC #207](https://leetcode.com/problems/course-schedule/)                                | 🟡 Medium  | Kahn's: if topo sort visits all V nodes, no cycle exists. If any node unvisited, cycle detected.                 | DFS with 2-color visited — you need 3 states (or Kahn's) to cleanly detect cycles.                                        |
| 2 | Course Schedule II        | [LC #210](https://leetcode.com/problems/course-schedule-ii/)                             | 🟡 Medium  | Same as #1 but collect the topo order. Kahn's naturally produces it as nodes are processed.                      | Not returning the order when a cycle is detected (return empty array for invalid input).                                   |
| 3 | Alien Dictionary          | [LC #269](https://leetcode.com/problems/alien-dictionary/)                               | 🔴 Hard    | Compare adjacent words pairwise to build character-dependency edges. Topo sort the character graph.              | Using more than the first differing character between two adjacent words — only the FIRST difference gives ordering info. |
| 4 | Minimum Height Trees      | [LC #310](https://leetcode.com/problems/minimum-height-trees/)                           | 🟡 Medium  | Reverse Kahn's: repeatedly prune leaf nodes (degree 1). Last 1–2 remaining nodes = roots of MHTs.               | Running full topo from every node — O(n²). Leaf-pruning is O(n).                                                         |
| 5 | Sequence Reconstruction   | [LC #444](https://leetcode.com/problems/sequence-reconstruction/)                        | 🟡 Medium  | Build order graph from sequences. Topo sort must be unique (queue never has >1 element at a time).               | Not verifying uniqueness of topo order — if at any point the queue has 2+ elements, multiple orderings exist.             |
| 6 | Course Schedule III       | [LC #630](https://leetcode.com/problems/course-schedule-iii/)                            | 🔴 Hard    | Greedy + heap, not topological sort. Sort by deadline; use max-heap to swap out longest course if over budget.   | Applying topological sort here — this is a greedy interval scheduling problem, not a dependency ordering problem.         |
| 7 | Sort Items by Groups      | [LC #1203](https://leetcode.com/problems/sort-items-respecting-group-dependencies/)      | 🔴 Hard    | Two-level topo sort: sort within groups AND sort groups relative to each other. Build two graphs simultaneously. | Flattening the problem into one graph — the group-level dependencies require a separate topo sort.                        |
| 8 | Parallel Courses          | [LC #1136](https://leetcode.com/problems/parallel-courses/)                              | 🟡 Medium  | Kahn's topo sort — number of BFS levels = minimum semesters needed.                                             | Counting edges instead of levels — the semester count = depth of the critical path (longest chain).                       |
| 9 | Find All Possible Recipes | [LC #2115](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/) | 🟡 Medium  | Build dependency graph. Supplies have in-degree 0 (starting points). Topo sort; collect nodes that are recipes.  | Not initializing supply nodes with in-degree 0 — they're the source nodes, not discovered through edges.                  |

---

---

# PATTERN #17 — Union-Find (Disjoint Set Union)

---

### 🧭 When to Use It

* **Dynamic connectivity:** Are two nodes in the same component? Merge two components.
* Detecting **cycles in undirected graphs**
* Problems involving **grouping or clustering** that changes over time
* Keywords:  *"connected components"* ,  *"unions"* ,  *"same group"* ,  *"dynamic connectivity"* , *"number of components"*

### 🧠 Core Intuition

Each node starts as its own parent (its own group). `find(x)` returns the root of x's group. `union(x, y)` merges two groups by making one's root point to the other's root. With **path compression** (make every node point directly to root on find) and **union by rank** (attach smaller tree under larger), both operations approach O(1) amortized — technically O(α(n)), the inverse Ackermann function.

### 🎯 Mental Model

> *A club roster. Everyone starts as their own club president. When two clubs merge, one president answers to the other. When you ask "who's your president?" you climb the chain — and as you go, you note shortcuts so future lookups are instant.*

### ⏱ Complexity Template

* **Find + Union:** O(α(n)) ≈ O(1) amortized with both optimizations
* **Space:** O(n) for parent and rank arrays

---

### 📚 Problem List

| #  | Problem                                        | Link                                                                                   | Difficulty | 💡 Key Insight                                                                                                                       | ⚠️ Common Mistake                                                                                                                              |
| -- | ---------------------------------------------- | -------------------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1  | Number of Provinces                            | [LC #547](https://leetcode.com/problems/number-of-provinces/)                             | 🟡 Medium  | Union all directly connected cities. Count distinct roots at the end.                                                                | BFS/DFS also works but Union-Find is cleaner and faster for connectivity counting.                                                               |
| 2  | Redundant Connection                           | [LC #684](https://leetcode.com/problems/redundant-connection/)                            | 🟡 Medium  | Process edges one by one. If both endpoints already share a root → this edge creates a cycle. Return it.                            | Not using path compression — without it, find() can be O(n) per call.                                                                           |
| 3  | Number of Islands                              | [LC #200](https://leetcode.com/problems/number-of-islands/)                               | 🟡 Medium  | Union each land cell with its neighbors. Count unique roots among land cells.                                                        | This is usually solved with DFS but Union-Find is worth knowing as an alternative approach here.                                                 |
| 4  | Graph Valid Tree                               | [LC #261](https://leetcode.com/problems/graph-valid-tree/)                                | 🟡 Medium  | Valid tree: n-1 edges, no cycle. Union edges — if any edge unites already-connected nodes → not a tree.                            | Forgetting to verify n-1 edges AND connectivity (both conditions required).                                                                      |
| 5  | Accounts Merge                                 | [LC #721](https://leetcode.com/problems/accounts-merge/)                                  | 🟡 Medium  | Union all emails within an account. Group by root email. Map root → account name.                                                   | Using the first email as the root (it might not be — path compression can change roots). Use consistent root tracking with a separate name map. |
| 6  | Smallest String With Swaps                     | [LC #1202](https://leetcode.com/problems/smallest-string-with-swaps/)                     | 🟡 Medium  | Union indices that can be swapped. Within each component, sort chars and place smallest available in order.                          | Not realizing that transitive swaps make all indices in the same component freely swappable.                                                     |
| 7  | Satisfiability of Equality Equations           | [LC #990](https://leetcode.com/problems/satisfiability-of-equality-equations/)            | 🟡 Medium  | Two passes: first union all `==`pairs; then check if any `!=`pair shares a root. If yes → impossible.                           | Mixing `==`and `!=`in one pass — process all equalities before checking inequalities.                                                       |
| 8  | Optimize Water Distribution                    | [LC #1168](https://leetcode.com/problems/optimize-water-distribution-in-a-village/)       | 🔴 Hard    | Add a virtual node 0 with edge to each house (cost = well cost). MST on the new graph = minimum cost.                                | Not adding the virtual node — makes it a standard Kruskal's MST problem once you do.                                                            |
| 9  | Number of Operations to Make Network Connected | [LC #1319](https://leetcode.com/problems/number-of-operations-to-make-network-connected/) | 🟡 Medium  | Count components and redundant edges. Need `components - 1`extra edges. If redundant edges < that → impossible.                   | Not counting redundant cables — you need to know how many spare connections exist to redistribute.                                              |
| 10 | Longest Consecutive Sequence                   | [LC #128](https://leetcode.com/problems/longest-consecutive-sequence/)                    | 🟡 Medium  | Union each num with num+1 if it exists. Track component sizes. Max size = answer. (Hashset approach in Pattern 1 is simpler though.) | Using Union-Find here is over-engineering — the hashset approach in Pattern 1 is cleaner. Know both.                                            |
| 11 | Making a Large Island                          | [LC #827](https://leetcode.com/problems/making-a-large-island/)                           | 🔴 Hard    | Label islands with Union-Find, track sizes. For each `0`cell, sum sizes of distinct neighboring islands + 1.                       | Summing island sizes without deduplicating — if two neighbors belong to the same island, count it only once.                                    |

---

---

# PATTERN #18 — Dynamic Programming: 1D

---

### 🧭 When to Use It

* Problem has **overlapping subproblems** (you compute the same sub-answer multiple times in brute force)
* Problem has **optimal substructure** (optimal solution built from optimal sub-solutions)
* Keywords:  *"minimum/maximum number of ways"* ,  *"count paths"* ,  *"can you reach"* , *"minimum cost"*
* When backtracking gives correct answers but TLEs → the overlapping sub-problems signal DP

### 🧠 Core Intuition

**Define your state first, in plain English.** `dp[i]` = what? Once you define the state, write the recurrence. Once you have the recurrence, the code writes itself. The most common mistake is jumping to code without defining what `dp[i]` means.

**Top-down (memoization):** Recursive + cache. Natural. Easy to write.
**Bottom-up (tabulation):** Iterative + array. More efficient. Preferred for interviews.

### 🎯 Mental Model

> *Staircase problem. To reach step 10, you came from step 9 or step 8. So the answer for step 10 is the answer for step 9 + the answer for step 8. You've never re-computed anything — just remembered it.*

### ⏱ Complexity Template

* **Time:** O(n) — fill each state once
* **Space:** O(n) for dp array. Often reducible to O(1) if only previous few states are needed

---

### 📚 Problem List

| #  | Problem                        | Link                                                                  | Difficulty | 💡 Key Insight                                                                                                                  | ⚠️ Common Mistake                                                                                      |
| -- | ------------------------------ | --------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 1  | Climbing Stairs                | [LC #70](https://leetcode.com/problems/climbing-stairs/)                 | 🟢 Easy    | `dp[i] = dp[i-1] + dp[i-2]`. It's Fibonacci. Space optimize to two variables.                                                 | Recursion without memoization — exponential time.                                                       |
| 2  | House Robber                   | [LC #198](https://leetcode.com/problems/house-robber/)                   | 🟡 Medium  | `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`. Rob this house or skip it.                                                         | Using dp[i-1] and dp[i-2] without careful base case initialization.                                      |
| 3  | House Robber II                | [LC #213](https://leetcode.com/problems/house-robber-ii/)                | 🟡 Medium  | Circular = run robber twice: once on `[0..n-2]`, once on `[1..n-1]`. Take the max.                                          | Trying to handle circularity inside one DP — splitting into two linear problems is the clean unlock.    |
| 4  | Min Cost Climbing Stairs       | [LC #746](https://leetcode.com/problems/min-cost-climbing-stairs/)       | 🟢 Easy    | `dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`. Start from either step 0 or 1.                                       | Off-by-one on cost indexing — draw the first 3 steps manually to verify.                                |
| 5  | Coin Change                    | [LC #322](https://leetcode.com/problems/coin-change/)                    | 🟡 Medium  | `dp[amount] = min(dp[amount - coin] + 1)`for each coin. Init dp with infinity, dp[0] = 0.                                     | Greedy (always pick largest coin) — fails for many coin systems. DP is required.                        |
| 6  | Coin Change II (count ways)    | [LC #518](https://leetcode.com/problems/coin-change-2/)                  | 🟡 Medium  | Unbounded knapsack variant. For each coin, update `dp[amount] += dp[amount - coin]`. Iterate coins in outer loop.             | Swapping inner/outer loop order — causes counting permutations instead of combinations.                 |
| 7  | Word Break                     | [LC #139](https://leetcode.com/problems/word-break/)                     | 🟡 Medium  | `dp[i]`= can `s[0..i-1]`be segmented. For each i, try all j < i where `dp[j]`is true and `s[j..i]`is in the dictionary. | Using recursion without memoization — exponential. The DP table is the memoization.                     |
| 8  | Decode Ways                    | [LC #91](https://leetcode.com/problems/decode-ways/)                     | 🟡 Medium  | `dp[i]`= ways to decode `s[0..i-1]`. Check 1-digit decode (s[i-1] ≠ '0') and 2-digit decode (s[i-2..i-1] in 10–26).       | Not handling `'0'`carefully — '0' alone is invalid; only '10' and '20' are valid two-digit decodings. |
| 9  | Longest Increasing Subsequence | [LC #300](https://leetcode.com/problems/longest-increasing-subsequence/) | 🟡 Medium  | O(n²) DP:`dp[i] = max(dp[j] + 1)`for all j < i where `nums[j] < nums[i]`. O(n log n) with patience sorting.                | Thinking about it as a subarray (contiguous) problem — LIS allows gaps.                                 |
| 10 | Partition Equal Subset Sum     | [LC #416](https://leetcode.com/problems/partition-equal-subset-sum/)     | 🟡 Medium  | 0/1 Knapsack: can any subset sum to `total/2`?`dp[j] = dp[j] OR dp[j - num]`iterating j backwards.                          | Iterating j forwards in the knapsack — causes a number to be "used" multiple times in one pass.         |
| 11 | Jump Game                      | [LC #55](https://leetcode.com/problems/jump-game/)                       | 🟡 Medium  | Greedy (not DP): track `maxReach`. If current index > maxReach → can't proceed.                                              | Using DP — it works (O(n²)) but greedy is O(n). Both are acceptable but know the greedy.               |
| 12 | Jump Game II                   | [LC #45](https://leetcode.com/problems/jump-game-ii/)                    | 🟡 Medium  | Greedy BFS: at each step, expand the current range's maximum reach. When you exhaust the current range, increment jumps.        | DP — O(n²). BFS/greedy gives O(n).                                                                     |
| 13 | Perfect Squares                | [LC #279](https://leetcode.com/problems/perfect-squares/)                | 🟡 Medium  | Coin change with coins = perfect squares ≤ n.`dp[i] = min(dp[i - sq] + 1)`for each square.                                   | Using math (Lagrange's four-square theorem) in an interview — DP is the expected approach.              |

---

---

# PATTERN #19 — DP: 2D / Grid

---

### 🧭 When to Use It

* Problems on a **2D grid** requiring min/max paths, counts, or reachability
* String problems where `dp[i][j]` represents a relationship between `s1[0..i]` and `s2[0..j]`
* Keywords:  *"unique paths"* ,  *"minimum path sum"* ,  *"edit distance"* ,  *"grid"* , *"dungeon"*

### 🧠 Core Intuition

Extend 1D DP to two dimensions. `dp[i][j]` typically represents the answer for the first i rows and first j columns (or first i characters of string 1 and j characters of string 2). Fill the table row by row. Base cases are the first row and first column.

**Space optimization:** Often you only need the previous row → reduce from O(m*n) to O(n) space.

### 🎯 Mental Model

> *Filling out a crossword grid. Each cell's answer depends on the cells above and to the left. You fill row by row, and each cell "knows" the answers of everything that came before it.*

### ⏱ Complexity Template

* **Time:** O(m * n)
* **Space:** O(m * n) — reducible to O(n) with rolling array optimization

---

### 📚 Problem List

| #  | Problem                    | Link                                                               | Difficulty | 💡 Key Insight                                                                                                                            | ⚠️ Common Mistake                                                                                             |
| -- | -------------------------- | ------------------------------------------------------------------ | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| 1  | Unique Paths               | [LC #62](https://leetcode.com/problems/unique-paths/)                 | 🟡 Medium  | `dp[i][j] = dp[i-1][j] + dp[i][j-1]`. Base case: entire first row and column = 1.                                                       | Recursion without memoization — exponential. Pure math (combinations) also works: C(m+n-2, m-1).               |
| 2  | Minimum Path Sum           | [LC #64](https://leetcode.com/problems/minimum-path-sum/)             | 🟡 Medium  | `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`. Fill in-place on grid itself to save space.                                      | Forgetting that you can only move right or down (not up or left).                                               |
| 3  | Edit Distance              | [LC #72](https://leetcode.com/problems/edit-distance/)                | 🔴 Hard    | `dp[i][j]`= min ops to convert `s1[0..i]`→`s2[0..j]`. If chars match:`dp[i-1][j-1]`. Else:`1 + min(insert, delete, replace)`.  | Not setting up base cases:`dp[i][0] = i`and `dp[0][j] = j`(delete or insert i/j chars).                     |
| 4  | Longest Common Subsequence | [LC #1143](https://leetcode.com/problems/longest-common-subsequence/) | 🟡 Medium  | `dp[i][j]`= LCS of `s1[0..i-1]`and `s2[0..j-1]`. If chars match:`1 + dp[i-1][j-1]`. Else:`max(dp[i-1][j], dp[i][j-1])`.         | Confusing subsequence (gaps allowed) with substring (contiguous).                                               |
| 5  | Maximal Square             | [LC #221](https://leetcode.com/problems/maximal-square/)              | 🟡 Medium  | `dp[i][j]`= side length of largest square with bottom-right at (i,j). If cell is '1':`1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`. | Computing area instead of side length — store side length, square it at the end.                               |
| 6  | Dungeon Game               | [LC #174](https://leetcode.com/problems/dungeon-game/)                | 🔴 Hard    | Fill dp from bottom-right to top-left.`dp[i][j]`= min health needed when entering cell (i,j).                                           | Filling top-left to bottom-right — you can't determine health needed without knowing future cells.             |
| 7  | Triangle                   | [LC #120](https://leetcode.com/problems/triangle/)                    | 🟡 Medium  | Bottom-up: start from second-to-last row.`dp[j] = triangle[i][j] + min(dp[j], dp[j+1])`. Answer at dp[0].                               | Top-down — bottom-up on the last row naturally accumulates the minimum path.                                   |
| 8  | Interleaving String        | [LC #97](https://leetcode.com/problems/interleaving-string/)          | 🟡 Medium  | `dp[i][j]`= can `s1[0..i-1]`and `s2[0..j-1]`interleave to form `s3[0..i+j-1]`. Check char match from either string.               | Greedy (always take whichever char matches) — fails when both match. Need DP to explore both options.          |
| 9  | Distinct Subsequences      | [LC #115](https://leetcode.com/problems/distinct-subsequences/)       | 🔴 Hard    | `dp[i][j]`= ways to form `t[0..j-1]`from `s[0..i-1]`. If chars match:`dp[i-1][j-1] + dp[i-1][j]`. Else:`dp[i-1][j]`.            | Mixing up which string is the source — s is longer and being searched, t is the target pattern.                |
| 10 | Cherry Pickup              | [LC #741](https://leetcode.com/problems/cherry-pickup/)               | 🔴 Hard    | Two simultaneous paths from (0,0) to (n-1,n-1).`dp[t][r1][r2]`where t = step count, r1/r2 = rows of each path.                          | Simulating one path then another greedily — optimal combined paths aren't simply two consecutive greedy paths. |

---

---

# PATTERN #20 — DP: Kadane's Algorithm & Variants

---

### 🧭 When to Use It

* **Maximum/minimum subarray** problems (contiguous)
* The answer can be extended or restarted at each position
* Keywords:  *"maximum subarray"* ,  *"maximum product subarray"* ,  *"circular subarray"* , *"buy and sell stock"*

### 🧠 Core Intuition

At each element, decide: **extend the current subarray, or start fresh?** `currentMax = max(num, currentMax + num)`. If the running sum goes negative, it's a liability — drop it and restart from the current element. Track the global max throughout.

### 🎯 Mental Model

> *Running a tally at a carnival. If your running score goes below zero, you're better off throwing away your ticket and starting fresh from the current game. But always remember your highest score.*

### ⏱ Complexity Template

* **Time:** O(n) — one pass
* **Space:** O(1)

---

### 📚 Problem List

| # | Problem                          | Link                                                                         | Difficulty | 💡 Key Insight                                                                                                                | ⚠️ Common Mistake                                                                                                                         |
| - | -------------------------------- | ---------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | Maximum Subarray                 | [LC #53](https://leetcode.com/problems/maximum-subarray/)                       | 🟡 Medium  | `curr = max(num, curr + num)`. Update global max. Classic Kadane's.                                                         | Resetting curr to 0 instead of num — fails when all numbers are negative.                                                                  |
| 2 | Maximum Product Subarray         | [LC #152](https://leetcode.com/problems/maximum-product-subarray/)              | 🟡 Medium  | Track both max AND min at each step (negative × negative = positive).`maxProd = max(num, maxProd*num, minProd*num)`.       | Tracking only max — a large negative min can flip to a large positive max with the next negative number.                                   |
| 3 | Maximum Sum Circular Subarray    | [LC #918](https://leetcode.com/problems/maximum-sum-circular-subarray/)         | 🟡 Medium  | Answer = max of (max subarray, total sum − min subarray). The circular case = wrap-around = exclude the minimum middle part. | Not handling the all-negatives edge case: if max subarray < 0, the circular formula gives 0 which is wrong — return max subarray directly. |
| 4 | Longest Turbulent Subarray       | [LC #978](https://leetcode.com/problems/longest-turbulent-subarray/)            | 🟡 Medium  | Track `inc`and `dec`lengths. At each element, swap based on whether current comparison is > or <.                         | Tracking only one direction (ascending or descending) — turbulent means alternating, so track both.                                        |
| 5 | Maximum Absolute Sum of Subarray | [LC #1749](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/) | 🟡 Medium  | Run Kadane's for max subarray AND min subarray. Answer = max(abs(maxSum), abs(minSum)).                                       | Only running one pass — you need both the most positive and most negative subarrays.                                                       |
| 6 | K-Concatenation Maximum Sum      | [LC #1191](https://leetcode.com/problems/k-concatenation-maximum-sum/)          | 🟡 Medium  | 3 cases: k=1 (Kadane's), k=2 (Kadane's on doubled), k>2 (add (k-2) × max(0, total sum) to k=2 answer).                       | Not handling negative total sum when k > 2 — repeating a negative-sum array shrinks the answer.                                            |

---

---

# PATTERN #21 — DP: 0/1 Knapsack & Unbounded Knapsack

---

### 🧭 When to Use It

* Choosing items with **weights and values** to maximize value within a capacity
* **Each item used at most once** (0/1) or **unlimited times** (unbounded)
* Keywords:  *"subset sum"* ,  *"partition"* ,  *"target sum"* ,  *"coin change"* , *"item selection with constraint"*

### 🧠 Core Intuition

**0/1 Knapsack:** For each item, either include it or don't. `dp[j] = max(dp[j], dp[j - weight] + value)`. Iterate j **backwards** so each item is only considered once.
**Unbounded:** Same recurrence but iterate j **forwards** — allows the same item to be picked multiple times.

The direction of the inner loop (forward vs backward) is the only difference between the two variants. Memorize this.

### 🎯 Mental Model

> *Packing a suitcase with weight limit. 0/1: each item is unique — take it or leave it. Unbounded: you have infinite copies of each item. The backwards/forwards iteration enforces the difference.*

### ⏱ Complexity Template

* **Time:** O(n * capacity) — n items, capacity states
* **Space:** O(capacity) — with 1D rolling array optimization

---

### 📚 Problem List

| # | Problem                    | Link                                                              | Difficulty | 💡 Key Insight                                                                                                                  | ⚠️ Common Mistake                                                                                                  |
| - | -------------------------- | ----------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| 1 | Partition Equal Subset Sum | [LC #416](https://leetcode.com/problems/partition-equal-subset-sum/) | 🟡 Medium  | 0/1 Knapsack: can any subset sum to `total/2`? Iterate j backwards.                                                           | Iterating j forwards — items get "reused," turning it into unbounded knapsack.                                      |
| 2 | Target Sum                 | [LC #494](https://leetcode.com/problems/target-sum/)                 | 🟡 Medium  | Assign `+`or `−`to each number. Equivalent to: find subset with sum =`(total + target) / 2`. 0/1 knapsack count variant. | Brute force DFS — 2ⁿ time. DP converts it to O(n * sum).                                                           |
| 3 | Coin Change                | [LC #322](https://leetcode.com/problems/coin-change/)                | 🟡 Medium  | Unbounded knapsack (infinite coins). Iterate j forwards.`dp[j] = min(dp[j], dp[j-coin] + 1)`.                                 | Using 0/1 knapsack (backwards) — each coin denomination is infinite, not limited to one use.                        |
| 4 | Coin Change II             | [LC #518](https://leetcode.com/problems/coin-change-2/)              | 🟡 Medium  | Unbounded: count ways. Outer loop = coins, inner loop = amount forwards. Combination ordering.                                  | Swapping inner/outer loops — you'd count permutations (ordered) instead of combinations (unordered).                |
| 5 | Last Stone Weight II       | [LC #1049](https://leetcode.com/problems/last-stone-weight-ii/)      | 🟡 Medium  | Minimize `                                                                                                                      | S1 - S2                                                                                                              |
| 6 | Ones and Zeroes            | [LC #474](https://leetcode.com/problems/ones-and-zeroes/)            | 🟡 Medium  | 2D 0/1 knapsack with two constraints (m zeros, n ones).`dp[i][j]`= max strings fitting within i zeros and j ones.             | 1D knapsack — two constraints require 2D DP table.                                                                  |
| 7 | Combination Sum IV         | [LC #377](https://leetcode.com/problems/combination-sum-iv/)         | 🟡 Medium  | Count ordered sequences (permutations). Outer loop = amount, inner loop = nums. Different from Coin Change II!                  | Using coin-change-II structure (outer = nums) — that gives combinations. Here order matters → outer = amount.      |
| 8 | Profitable Schemes         | [LC #879](https://leetcode.com/problems/profitable-schemes/)         | 🔴 Hard    | 2D knapsack:`dp[members][profit]`. Add members and profit of each crime if within member limit; profit caps at minProfit.     | Not capping profit at minProfit — you need "at least minProfit," so all profits ≥ minProfit collapse to one state. |

---

---

# PATTERN #22 — DP: LCS / LIS Family

---

### 🧭 When to Use It

* **Longest Common Subsequence:** Two sequences, find longest matching subsequence (non-contiguous)
* **Longest Increasing Subsequence:** One sequence, find longest strictly increasing subsequence
* Keywords:  *"common subsequence"* ,  *"increasing subsequence"* ,  *"delete to match"* , *"minimum insertions/deletions"*

### 🧠 Core Intuition

**LCS:** `dp[i][j]` = LCS of first i chars of s1 and first j chars of s2. If they match: `dp[i-1][j-1] + 1`. If not: `max(dp[i-1][j], dp[i][j-1])`.
**LIS:** `dp[i]` = LIS ending at index i. For each j < i where `nums[j] < nums[i]`: `dp[i] = max(dp[i], dp[j] + 1)`. O(n log n) variant uses binary search (patience sorting).

### 🎯 Mental Model

> *LCS: Two rivers running in parallel — find the longest set of points where they're at the same "height" (same character) at the same relative position.*
> *LIS: Building the tallest possible tower of cards where each new card must be taller than the previous one you placed.*

### ⏱ Complexity Template

* **LCS:** O(m * n) time, O(m * n) space (reducible to O(n))
* **LIS:** O(n²) DP or O(n log n) with patience sorting

---

### 📚 Problem List

| # | Problem                          | Link                                                                            | Difficulty | 💡 Key Insight                                                                                                                | ⚠️ Common Mistake                                                                                                                  |
| - | -------------------------------- | ------------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| 1 | Longest Common Subsequence       | [LC #1143](https://leetcode.com/problems/longest-common-subsequence/)              | 🟡 Medium  | Core LCS recurrence. Memorize this — dozens of problems reduce to it.                                                        | Confusing with "longest common substring" (contiguous). LCS allows gaps.                                                             |
| 2 | Longest Increasing Subsequence   | [LC #300](https://leetcode.com/problems/longest-increasing-subsequence/)           | 🟡 Medium  | O(n²) DP for understanding. O(n log n) patience sort for interviews (binary search on a "tails" array).                      | Thinking the answer is always at `dp[n-1]`— it's `max(dp[0..n-1])`.                                                             |
| 3 | Delete Operation for Two Strings | [LC #583](https://leetcode.com/problems/delete-operation-for-two-strings/)         | 🟡 Medium  | Min deletions =`m + n - 2 * LCS(s1, s2)`. Reduce to LCS immediately.                                                        | Solving it as a separate DP instead of recognizing the LCS reduction.                                                                |
| 4 | Shortest Common Supersequence    | [LC #1092](https://leetcode.com/problems/shortest-common-supersequence/)           | 🔴 Hard    | Length =`m + n - LCS`. To reconstruct: trace LCS table; shared chars go in once, unshared chars from both strings.          | Computing length only — the problem asks for the actual string. Reconstruction requires tracing the DP table.                       |
| 5 | Minimum ASCII Delete Sum         | [LC #712](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/) | 🟡 Medium  | LCS variant: maximize sum of ASCII values of common chars (not count). Then answer = totalASCII - 2 * maxCommonASCII.         | Using character count instead of ASCII value in the DP — the weight of each match matters here.                                     |
| 6 | Longest Bitonic Subsequence      | [GFG](https://www.geeksforgeeks.org/longest-bitonic-subsequence-dp-15/)            | 🟡 Medium  | LIS from left for each index + LIS from right for each index. Answer = max of `LIS[i] + LDS[i] - 1`.                        | Computing only one direction — bitonic requires increasing then decreasing, so both directions needed.                              |
| 7 | Russian Doll Envelopes           | [LC #354](https://leetcode.com/problems/russian-doll-envelopes/)                   | 🔴 Hard    | Sort by width ascending, then by height**descending**for equal widths. LIS on heights only.                             | Sorting height ascending for equal widths — same-width envelopes can't nest, so descending prevents them from being counted in LIS. |
| 8 | Longest String Chain             | [LC #1048](https://leetcode.com/problems/longest-string-chain/)                    | 🟡 Medium  | Sort by word length. For each word, try removing each character; if shorter word in map, update chain length. LIS on strings. | Not sorting by length first — predecessors must be processed before successors.                                                     |
| 9 | Uncrossed Lines                  | [LC #1035](https://leetcode.com/problems/uncrossed-lines/)                         | 🟡 Medium  | Exactly LCS — non-crossing lines = common subsequence. The visual "crossing" constraint maps 1:1 to LCS.                     | Solving it as a geometry/graph problem — recognize the LCS equivalence immediately.                                                 |

---

---

# PATTERN #23 — DP: Interval DP

---

### 🧭 When to Use It

* Problems on **contiguous ranges or intervals** of a sequence
* Optimal solution on interval `[i, j]` depends on splitting it at some point k
* Keywords:  *"burst balloons"* ,  *"matrix chain multiplication"* ,  *"palindrome partitioning"* ,  *"stone merge"* , *"optimal game strategy"*

### 🧠 Core Intuition

`dp[i][j]` = optimal answer for the subproblem on interval `[i, j]`. Fill by **increasing interval length** — solve all length-1 intervals first, then length-2, etc. For each `[i, j]`, try every split point k and combine `dp[i][k]` with `dp[k+1][j]`.

### 🎯 Mental Model

> *Dividing a piece of rope into segments. To find the optimal cut of rope [i..j], try every possible first cut point k — the cost is the cost of cutting [i..k] + cost of cutting [k+1..j] + some joining cost.*

### ⏱ Complexity Template

* **Time:** O(n³) — n² intervals × n split points
* **Space:** O(n²) for the dp table

---

### 📚 Problem List

| # | Problem                            | Link                                                                       | Difficulty | 💡 Key Insight                                                                                                                                             | ⚠️ Common Mistake                                                                                  |
| - | ---------------------------------- | -------------------------------------------------------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| 1 | Minimum Cost Tree from Leaf Values | [LC #1130](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/) | 🟡 Medium  | Interval DP:`dp[i][j]`= min non-leaf sum for subarray `[i,j]`. Split at k:`dp[i][k] + dp[k+1][j] + max(arr[i..k]) * max(arr[k+1..j])`.               | Greedy (always merge smallest pair) — this works! Actually the greedy here is valid. Know both.     |
| 2 | Burst Balloons                     | [LC #312](https://leetcode.com/problems/burst-balloons/)                      | 🔴 Hard    | Think about the**last**balloon burst in range `[i, j]`.`dp[i][j] = dp[i][k-1] + nums[i-1]*nums[k]*nums[j+1] + dp[k+1][j]`.                       | Thinking about the first balloon burst — the last burst formulation avoids changing borders.        |
| 3 | Strange Printer                    | [LC #664](https://leetcode.com/problems/strange-printer/)                     | 🔴 Hard    | `dp[i][j]`= min turns to print `s[i..j]`. If `s[k] == s[j]`for some k in [i,j-1], we can merge their prints:`dp[i][j] = dp[i][k] + dp[k+1][j-1]`.  | Brute-force simulation — the key is recognizing when two same characters can share a print turn.    |
| 4 | Palindrome Partitioning II         | [LC #132](https://leetcode.com/problems/palindrome-partitioning-ii/)          | 🔴 Hard    | Precompute `isPalin[i][j]`. Then 1D DP:`dp[i]`= min cuts for `s[0..i]`. For each j ≤ i where `isPalin[j][i]`is true:`dp[i] = min(dp[j-1] + 1)`. | Not precomputing palindromes — checking on the fly inside DP is O(n³). Precompute in O(n²).       |
| 5 | Optimal Strategy for a Game        | [GFG](https://www.geeksforgeeks.org/optimal-strategy-for-a-game-dp-31/)       | 🟡 Medium  | `dp[i][j]`= max coins first player can guarantee from `[i,j]`. Choose left or right coin; opponent plays optimally.                                    | Assuming opponent plays badly —`dp[i][j]`must account for opponent's optimal response.            |
| 6 | Zuma Game                          | [LC #488](https://leetcode.com/problems/zuma-game/)                           | 🔴 Hard    | Interval DP + grouping consecutive same-color balls. Genuinely hard — only attempt if you've mastered the simpler interval DPs.                           | Treating each ball independently — grouping consecutive same-colored balls reduces the state space. |

---

---

# PATTERN #24 — DP: Bitmask (Advanced)

---

### 🧭 When to Use It

* **Small n (≤ 20)** with subset-based states
* You need to track **which elements have been used** in a set
* Keywords:  *"assign tasks"* ,  *"traveling salesman"* ,  *"cover all nodes"* , *"minimum XOR over subsets"*
* The "try all subsets" brute force is too slow → bitmask DP compresses subset states

### 🧠 Core Intuition

Represent a subset of n elements as an integer bitmask (bit i = 1 means element i is included). `dp[mask]` = optimal answer using exactly the elements in `mask`. Transition: try adding one element not yet in `mask`. With n ≤ 20, there are 2²⁰ ≈ 1M states — manageable.

### 🎯 Mental Model

> *A checklist of n items represented as a binary number. Each combination of checked/unchecked items is a unique number. You DP over all 2ⁿ possible checklist states.*

### ⏱ Complexity Template

* **Time:** O(2ⁿ * n) — 2ⁿ states, n transitions each
* **Space:** O(2ⁿ) for the DP array

---

### 📚 Problem List

| # | Problem                          | Link                                                                    | Difficulty | 💡 Key Insight                                                                                                   | ⚠️ Common Mistake                                                                                       |
| - | -------------------------------- | ----------------------------------------------------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| 1 | Counting Bits                    | [LC #338](https://leetcode.com/problems/counting-bits/)                    | 🟢 Easy    | `dp[i] = dp[i >> 1] + (i & 1)`. Right shift + check last bit.                                                  | Counting bits naively for each number — O(n log n). DP gives O(n).                                       |
| 2 | Partition to K Equal Sum Subsets | [LC #698](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/) | 🟡 Medium  | Bitmask DP:`dp[mask]`= can elements in mask be partitioned into complete groups. Track running sum mod target. | Pure backtracking — TLEs for large inputs. Memoization over bitmask states avoids recomputation.         |
| 3 | Minimum XOR Sum of Two Arrays    | [LC #1879](https://leetcode.com/problems/minimum-xor-sum-of-two-arrays/)   | 🔴 Hard    | `dp[mask]`= min XOR sum using elements of nums2 indicated by mask. Assign nums1[i] to the i-th set bit.        | Greedy pairing — XOR is not globally minimized by local greedy choices. DP over assignments is required. |
| 4 | Shortest Path Visiting All Nodes | [LC #847](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) | 🔴 Hard    | BFS on `(node, visited_mask)`states. Start BFS from all nodes simultaneously. Full mask = all nodes visited.   | DFS — doesn't guarantee shortest path. BFS on the combined state space is required.                      |
| 5 | Maximum AND Sum of Array         | [LC #2172](https://leetcode.com/problems/maximum-and-sum-of-array/)        | 🔴 Hard    | `dp[mask]`= max AND sum assigning nums to slots indicated by mask. Each slot holds ≤ 2 numbers.               | Not accounting for the ≤ 2 per slot constraint — track slot occupancy in the bitmask carefully.         |

---

---

# PATTERN #25 — Greedy Algorithms

---

### 🧭 When to Use It

* Making the **locally optimal choice** at each step leads to a globally optimal solution
* Problems involving **scheduling, intervals, minimum spanning trees, or optimization**
* Keywords:  *"minimum number of"* ,  *"maximum possible"* ,  *"earliest"* , *"least"* with a clear local ordering
* **Proof test:** Can you prove that never choosing the greedy option leads to a worse or equal outcome?

### 🧠 Core Intuition

Greedy works when the problem has the **exchange argument property** — swapping the greedy choice with any other choice doesn't improve the solution. You don't need to try all possibilities: just always take the best-looking option right now. The key skill is **recognizing when greedy is safe** (and when it isn't — that's when you use DP).

### 🎯 Mental Model

> *Paying with cash. To minimize coins used, always take the largest denomination that fits. This works for standard coin systems but fails for exotic ones — that's when you need DP (Coin Change).*

### ⏱ Complexity Template

* Typically O(n log n) — due to sorting before the greedy pass
* O(n) if no sorting needed

---

### 📚 Problem List

| #  | Problem                                    | Link                                                                              | Difficulty | 💡 Key Insight                                                                                                                    | ⚠️ Common Mistake                                                                                                                 |
| -- | ------------------------------------------ | --------------------------------------------------------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1  | Jump Game                                  | [LC #55](https://leetcode.com/problems/jump-game/)                                   | 🟡 Medium  | Track max reachable index. If current index > max reachable → stuck. Update max reach greedily.                                  | DP — O(n²) and unnecessary. Greedy one-pass is O(n).                                                                              |
| 2  | Jump Game II                               | [LC #45](https://leetcode.com/problems/jump-game-ii/)                                | 🟡 Medium  | Greedy BFS: at each step, find the farthest reachable index in the current range. Increment jumps when range exhausted.           | Simulating each jump individually — O(n²). Greedy range expansion is O(n).                                                        |
| 3  | Gas Station                                | [LC #134](https://leetcode.com/problems/gas-station/)                                | 🟡 Medium  | If total gas ≥ total cost, a solution exists. Starting point = first index after a cumulative deficit.                           | Brute-forcing every starting position — O(n²). The key insight: if a prefix fails, skip it entirely.                              |
| 4  | Hand of Straights                          | [LC #846](https://leetcode.com/problems/hand-of-straights/)                          | 🟡 Medium  | Sort cards, greedily form groups starting from the smallest card. Use a sorted map to track counts.                               | Starting groups from random cards — always start from the smallest available to minimize waste.                                    |
| 5  | Merge Triplets to Form Target              | [LC #1899](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/)     | 🟡 Medium  | Filter triplets where any element exceeds target. Among remaining, check if max of each position equals target.                   | Including triplets with values exceeding target — merging takes max, so they'd corrupt the result.                                 |
| 6  | Partition Labels                           | [LC #763](https://leetcode.com/problems/partition-labels/)                           | 🟡 Medium  | Last occurrence of each char = how far that partition must extend. Greedily extend current partition's end.                       | Not tracking the furthest last occurrence — you must extend the partition whenever a char's last occurrence is beyond current end. |
| 7  | Valid Parenthesis String                   | [LC #678](https://leetcode.com/problems/valid-parenthesis-string/)                   | 🟡 Medium  | Track range `[minOpen, maxOpen]`.`*`→ try all three options by expanding/shrinking range. Valid if minOpen reaches 0 at end. | Greedy with a single counter —`*`has three interpretations; track the full possible range.                                       |
| 8  | Task Scheduler                             | [LC #621](https://leetcode.com/problems/task-scheduler/)                             | 🟡 Medium  | Math:`(maxFreq - 1) * (n + 1) + countOfMaxFreq`. Or greedy simulation with max-heap.                                            | Simulating without the formula — both valid but formula is O(1).                                                                   |
| 9  | Candy                                      | [LC #135](https://leetcode.com/problems/candy/)                                      | 🔴 Hard    | Two passes: left-to-right (handle right-neighbor rules), right-to-left (handle left-neighbor rules). Take max at each position.   | Single pass — you need both directions. A child with higher rating than both neighbors needs both constraints satisfied.           |
| 10 | Minimum Number of Arrows to Burst Balloons | [LC #452](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) | 🟡 Medium  | Sort by end. Shoot at end of first balloon. Skip all balloons it hits. Shoot again at next unpopped balloon's end.                | Sorting by start — sorting by end is the greedy key for interval shooting/covering problems.                                       |

---

---

# PATTERN #26 — Intervals (Merge & Sweep)

---

### 🧭 When to Use It

* Working with **ranges/intervals** that may overlap
* Finding **overlap, union, gaps, or minimum coverage**
* Keywords:  *"merge intervals"* ,  *"insert interval"* ,  *"meeting rooms"* ,  *"minimum platforms"* , *"covered intervals"*

### 🧠 Core Intuition

**Merge:** Sort by start. For each interval, if it overlaps with the previous (current start ≤ prev end), merge by extending the end. Otherwise, start a new interval.
**Sweep Line:** Create events for interval starts (+1) and ends (-1). Sort all events and process chronologically — count of active intervals at any point = sum so far.

### 🎯 Mental Model

> *Traffic on a highway. Sort cars by when they enter. If a car enters before the previous exits — they're on the road simultaneously. Track the peak count. That's your answer.*

### ⏱ Complexity Template

* **Sorting:** O(n log n) — dominates
* **Processing:** O(n) after sorting
* **Space:** O(n) for output or events

---

### 📚 Problem List

| # | Problem                                | Link                                                                           | Difficulty | 💡 Key Insight                                                                                                    | ⚠️ Common Mistake                                                                                                    |
| - | -------------------------------------- | ------------------------------------------------------------------------------ | ---------- | ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 1 | Merge Intervals                        | [LC #56](https://leetcode.com/problems/merge-intervals/)                          | 🟡 Medium  | Sort by start. Merge if current start ≤ last merged end. Update end to max of both.                              | Not updating the end to `max(prev_end, curr_end)`— a smaller interval inside a larger one gets skipped incorrectly. |
| 2 | Insert Interval                        | [LC #57](https://leetcode.com/problems/insert-interval/)                          | 🟡 Medium  | Three phases: add all intervals ending before new start, merge all overlapping, add remainder.                    | Trying to handle all cases with one loop — three clean phases is the unlock.                                          |
| 3 | Non-Overlapping Intervals              | [LC #435](https://leetcode.com/problems/non-overlapping-intervals/)               | 🟡 Medium  | Sort by end. Greedily keep interval with earliest end. Count removals when overlap detected.                      | Sorting by start — sorting by end is the greedy key for keeping maximum non-overlapping intervals.                    |
| 4 | Meeting Rooms                          | [LC #252](https://leetcode.com/problems/meeting-rooms/)                           | 🟢 Easy    | Sort by start. If any interval starts before the previous ends → conflict.                                       | Not sorting first — overlap check only works after sorting by start time.                                             |
| 5 | Meeting Rooms II                       | [LC #253](https://leetcode.com/problems/meeting-rooms-ii/)                        | 🟡 Medium  | Min-heap of end times. Sort meetings by start. If earliest end ≤ current start, reuse the room. Count heap size. | Sweep line (also works): create start/end events, sort, scan — +1 for start, -1 for end. Peak = answer.               |
| 6 | Minimum Interval to Include Each Query | [LC #2158](https://leetcode.com/problems/minimum-interval-to-include-each-query/) | 🔴 Hard    | Sort queries AND intervals. Sweep: add valid intervals to min-heap (by size), pop expired ones per query.         | Not sorting queries offline — online processing forces re-scanning all intervals per query.                           |
| 7 | Employee Free Time                     | [LC #759](https://leetcode.com/problems/employee-free-time/)                      | 🔴 Hard    | Flatten all schedules, sort, merge (like LC #56). Gaps between merged intervals = free time.                      | Trying to compute free time per employee separately — merge everyone's schedule first, then find global gaps.         |
| 8 | Points That Intersect With Cars        | [LC #2848](https://leetcode.com/problems/points-that-intersect-with-cars/)        | 🟢 Easy    | Merge intervals (LC #56), then count total points covered. Direct application.                                    | Counting points without merging — overcounts points in overlapping regions.                                           |
| 9 | Count Ways to Group Overlapping Ranges | [LC #2580](https://leetcode.com/problems/count-ways-to-group-overlapping-ranges/) | 🟡 Medium  | Merge overlapping ranges, count merged groups. Answer =`2^groups mod 10^9+7`(each group goes to group 1 or 2).  | Counting groups wrong by not merging — overlapping ranges must form one group.                                        |

---

---

# PATTERN #27 — Math & Bit Manipulation

---

### 🧭 When to Use It

* Problem involves **single number detection, power-of-2 checks, counting bits, or XOR tricks**
* Constraints suggest an **O(1) or O(log n)** mathematical solution exists
* Keywords:  *"without extra space"* ,  *"single number"* ,  *"power of two"* ,  *"number of 1 bits"* , *"reverse bits"*

### 🧠 Core Intuition — The 10 Bit Tricks You Must Know

| Trick | Operation          | What it Does                    |
| ----- | ------------------ | ------------------------------- |
| 1     | `n & (n-1)`      | Clears the lowest set bit       |
| 2     | `n & (-n)`       | Isolates the lowest set bit     |
| 3     | `n & 1`          | Check if n is odd               |
| 4     | `n >> 1`         | Divide by 2 (floor)             |
| 5     | `a ^ a == 0`     | XOR of a number with itself = 0 |
| 6     | `a ^ 0 == a`     | XOR with 0 = identity           |
| 7     | `n & (n-1) == 0` | Check if n is a power of 2      |
| 8     | `~n + 1 == -n`   | Two's complement negation       |
| 9     | `a ^ b ^ a == b` | XOR is its own inverse          |
| 10    | `(a + b) >> 1`   | Average without overflow        |

### 🎯 Mental Model

> *Bits are light switches. XOR = flip the switch. AND = keep only what's on in both. OR = turn on if either is on. You can do incredible things with just flipping switches.*

### ⏱ Complexity Template

* Most bit tricks: O(1) or O(log n)
* Bit counting: O(number of set bits) with `n & (n-1)` loop

---

### 📚 Problem List

| #  | Problem                             | Link                                                                | Difficulty | 💡 Key Insight                                                                                                         | ⚠️ Common Mistake                                                                                                                             |
| -- | ----------------------------------- | ------------------------------------------------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 1  | Single Number                       | [LC #136](https://leetcode.com/problems/single-number/)                | 🟢 Easy    | XOR all numbers. Pairs cancel (a^a=0). Single number remains.                                                          | Using a hash set — O(n) space. XOR is O(1) space.                                                                                              |
| 2  | Number of 1 Bits                    | [LC #191](https://leetcode.com/problems/number-of-1-bits/)             | 🟢 Easy    | `while n: count += 1; n &= n-1`. Each iteration removes one set bit.                                                 | Bit-shifting 32 times — works but `n & (n-1)`is faster for sparse bits.                                                                      |
| 3  | Counting Bits                       | [LC #338](https://leetcode.com/problems/counting-bits/)                | 🟢 Easy    | `dp[i] = dp[i >> 1] + (i & 1)`. Count bits for all n in O(n).                                                        | Computing popcount for each number separately — O(n log n). DP gives O(n).                                                                     |
| 4  | Reverse Bits                        | [LC #190](https://leetcode.com/problems/reverse-bits/)                 | 🟢 Easy    | For each of 32 bits: extract LSB, add to result with left shift, right-shift n.                                        | Treating n as a signed integer — use unsigned right shift (`>>>`in Java).                                                                    |
| 5  | Missing Number                      | [LC #268](https://leetcode.com/problems/missing-number/)               | 🟢 Easy    | XOR all indices (0..n) with all values. Missing number = result. Or: expected sum − actual sum.                       | Sorting — O(n log n) and unnecessary. XOR and sum tricks are both O(n) time, O(1) space.                                                       |
| 6  | Power of Two                        | [LC #231](https://leetcode.com/problems/power-of-two/)                 | 🟢 Easy    | `n > 0 and (n & n-1) == 0`. Power of 2 has exactly one set bit.                                                      | Dividing by 2 in a loop — O(log n). Bit trick is O(1).                                                                                         |
| 7  | Single Number II                    | [LC #137](https://leetcode.com/problems/single-number-ii/)             | 🔴 Hard    | For each bit position, count total 1s. If `count % 3 != 0`, the unique number has that bit set.                      | Using XOR (works for "appears twice" but not "appears three times" — the modulo-3 counting is required).                                       |
| 8  | Sum of Two Integers (no + operator) | [LC #371](https://leetcode.com/problems/sum-of-two-integers/)          | 🟡 Medium  | `a ^ b`= sum without carry.`(a & b) << 1`= carry. Repeat until carry is 0.                                         | Recursing without masking in Python — Python integers have unlimited precision, causing infinite loops on negatives. Mask with `0xFFFFFFFF`. |
| 9  | Reverse Integer                     | [LC #7](https://leetcode.com/problems/reverse-integer/)                | 🟡 Medium  | Pop digits with `% 10`and push with `* 10`. Check overflow before each multiplication.                             | Reversing the string — valid but misses the overflow check requirement.                                                                        |
| 10 | Excel Sheet Column Number           | [LC #171](https://leetcode.com/problems/excel-sheet-column-number/)    | 🟢 Easy    | Base-26 to decimal.`result = result * 26 + (char - 'A' + 1)`.                                                        | Treating 'A' as 0 — in this system, 'A'=1, 'Z'=26. No zero digit.                                                                              |
| 11 | Bitwise AND of Numbers Range        | [LC #201](https://leetcode.com/problems/bitwise-and-of-numbers-range/) | 🟡 Medium  | Right-shift both m and n until equal. The common prefix is the answer.                                                 | AND-ing every number in range — O(n). Common prefix shift is O(log n).                                                                         |
| 12 | Find the Duplicate Number           | [LC #287](https://leetcode.com/problems/find-the-duplicate-number/)    | 🟡 Medium  | Floyd's cycle detection (Pattern 6) on the array as a linked list (`next = nums[i]`). Or: bit counting per position. | Sorting or using a hash set — both violate the O(1) space constraint.                                                                          |

---

---

# PATTERN #28 — String Algorithms (KMP, Rabin-Karp)

---

### 🧭 When to Use It

* **Pattern matching** in a text: find all occurrences of a pattern in a string
* Checking if a string is a **rotation or repetition** of another
* Constraints suggest the O(n*m) brute force will TLE: very long text AND pattern
* Keywords:  *"find pattern in text"* ,  *"repeated substring"* , *"string rotation"*

> ⚠️ **Honest advice:** KMP comes up rarely in standard FAANG interviews. Know the concept and be able to explain it. Only code it from scratch if you're targeting companies like Google or going for senior roles. For most interviews, built-in string methods + Rolling Hash intuition is sufficient.

### 🧠 Core Intuition

**KMP:** Precompute a `lps` (Longest Proper Prefix which is also Suffix) array for the pattern. On mismatch, instead of restarting from scratch, use lps to jump back to the farthest position that still makes progress.
**Rabin-Karp:** Use a rolling hash. Hash the pattern and a sliding window of the text. When hashes match, verify with string comparison. Efficient for multiple pattern search.

### 🎯 Mental Model

> *KMP: Reading a book looking for a chapter title. If you mismatch halfway through "Chapter", instead of going back to the start of the page, your bookmark (lps array) tells you exactly where you can safely resume.*

### ⏱ Complexity Template

* **KMP:** O(n + m) — n = text, m = pattern
* **Rabin-Karp:** O(n + m) average, O(nm) worst case (hash collisions)

---

### 📚 Problem List

| # | Problem                            | Link                                                                                                 | Difficulty | 💡 Key Insight                                                                                                      | ⚠️ Common Mistake                                                                                   |
| - | ---------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| 1 | Find the Index of First Occurrence | [LC #28](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)             | 🟢 Easy    | This IS the KMP problem. Built-in `find()`works but know the O(n+m) KMP solution.                                 | Using built-in and calling it a day — in an interview, explain KMP even if you use the built-in.     |
| 2 | Repeated Substring Pattern         | [LC #459](https://leetcode.com/problems/repeated-substring-pattern/)                                    | 🟢 Easy    | If `s`is a repeat:`(s + s)[1:-1]`contains `s`. Or: KMP lps[n-1] > 0 and `n % (n - lps[n-1]) == 0`.          | Brute-forcing all divisors — O(n²). The doubled-string trick or KMP lps is O(n).                    |
| 3 | Rotate String                      | [LC #796](https://leetcode.com/problems/rotate-string/)                                                 | 🟢 Easy    | `s`is a rotation of `goal`iff `goal`appears in `s + s`. O(n) with KMP or built-in.                          | Trying all rotation shifts manually — O(n²). The concatenation trick reduces to a substring search. |
| 4 | Shortest Palindrome                | [LC #214](https://leetcode.com/problems/shortest-palindrome/)                                           | 🔴 Hard    | Concatenate `s + '#' + reverse(s)`. The lps of this combined string gives the longest palindrome prefix of s.     | Trying all prefix/suffix palindromes — O(n²). KMP on the combined string is O(n).                   |
| 5 | String Matching in Array           | [LC #1408](https://leetcode.com/problems/string-matching-in-an-array/)                                  | 🟢 Easy    | For each word, check if it's a substring of any other word. Join all words; use KMP or built-in contains.           | Comparing each pair naively — O(n² * L). Joining and searching is O(total_chars).                   |
| 6 | Minimum Characters for Words       | [GFG](https://www.geeksforgeeks.org/minimum-characters-to-be-added-at-front-to-make-string-palindrome/) | 🟡 Medium  | KMP lps on `s + '#' + reverse(s)`.`n - lps[-1]`= chars to prepend.                                              | Manual palindrome checking from the end — O(n²). KMP lps gives O(n).                                |
| 7 | Rabin-Karp Implementation          | [GFG](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/)                        | 🟡 Medium  | Rolling hash: subtract leaving char, add new char, multiply for position. Check string equality on hash match only. | Not handling hash collisions — always verify with string comparison on hash match.                   |

---

---

# 🏁 You're Now Fully Equipped

Here's the complete picture of everything you now have:

| Phase              | Patterns                                  | Status    |
| ------------------ | ----------------------------------------- | --------- |
| Foundation         | 1–8 (Arrays through Queues)              | ✅ Part 1 |
| Trees              | 9–12 (BFS, DFS, BST, Tries)              | ✅ Part 2 |
| Core DS            | 13–14 (Heap, Backtracking)               | ✅ Part 2 |
| Graphs             | 15–17 (BFS/DFS, Topo, Union-Find)        | ✅ Part 2 |
| DP — All Variants | 18–24 (1D through Bitmask)               | ✅ Part 2 |
| Advanced           | 25–28 (Greedy, Intervals, Bits, Strings) | ✅ Part 2 |

---

## 🎯 What Separates Good from Great Candidates

**Good candidates** can solve problems they've seen before.
**Great candidates** see a new problem, identify the pattern in 60 seconds, and build the solution from first principles.

The difference is built through **three habits:**

**1. Always ask "what's the brute force?" first** — then ask "what's wasteful about it?" The answer to the second question IS the pattern.

**2. Never just solve — always classify.** After every problem you do, write one sentence: *"This was a [pattern] problem because [trigger]. I recognized it when I saw [clue]."* Do this for 100 problems and pattern recognition becomes automatic.

**3. Revisit your failures.** The problems that took you >30 minutes or that you had to peek at — those are your gold. Re-solve them cold in 48 hours, 7 days, and 30 days. Mastery is built in the reps after the first solve, not during it.

---



# 🗺️ Week-by-Week Learning Schedule

## Phase 1: Foundation (Weeks 1–4)

| Week             | Patterns                        | Daily Goal        | Focus                                                                            |
| ---------------- | ------------------------------- | ----------------- | -------------------------------------------------------------------------------- |
| **Week 1** | [Arrays & Hashing](#pattern-#1-—-arrays-&-hashing) + Two Pointers | 3 problems/day    | Get comfortable with the basics. Every Easy must be solved, then attack Mediums. |
| **Week 2** | Sliding Window + Prefix Sum     | 3 problems/day    | Master the "window mindset." Draw every window change on paper first.            |
| **Week 3** | Binary Search (both variants)   | 2–3 problems/day | Nail the template. Write `isValid()`for answer-space problems before coding.   |
| **Week 4** | Linked List + Stack             | 2–3 problems/day | Pointer diagrams are mandatory. Draw every reversal.                             |

**Week 4 Milestone:** You should be able to solve any Easy under 10 mins, and most Mediums from Patterns 1–6 under 25 mins. If not — spend another week in review before moving on.

**Revision days:** Every Saturday = re-solve 5 problems from the current week from scratch (no peeking). Every Sunday = redo 3 problems from previous weeks.

---

## Phase 2: Core (Weeks 5–10)

| Week              | Patterns                      | Daily Goal        | Focus                                                                                                       |
| ----------------- | ----------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------------- |
| **Week 5**  | Trees — BFS + DFS            | 3 problems/day    | Master recursive DFS. Code BFS with a queue, not recursion.                                                 |
| **Week 6**  | BST + Tries                   | 2 problems/day    | BST properties, Trie insert/search. Not glamorous but frequently tested.                                    |
| **Week 7**  | Heap / Priority Queue         | 2–3 problems/day | Know the `heapq`API cold. Top-K problems are everywhere.                                                  |
| **Week 8**  | Backtracking                  | 2 problems/day    | Slow down. Draw the decision tree before coding. Every backtracking has 3 parts: choose, explore, unchoose. |
| **Week 9**  | Graphs — BFS & DFS           | 2–3 problems/day | Build adjacency list. Know both BFS (queue) and DFS (stack/recursion).                                      |
| **Week 10** | Topological Sort + Union-Find | 2 problems/day    | Topo sort = cycle detection + ordering. Union-Find = connectivity.                                          |

**Week 10 Milestone:** You should consistently solve Graph Mediums. At this point you are ready for ~70% of real interview questions.

---

## Phase 3: Advanced / DP (Weeks 11–16)

| Week              | Patterns                         | Daily Goal        | Focus                                                                        |
| ----------------- | -------------------------------- | ----------------- | ---------------------------------------------------------------------------- |
| **Week 11** | DP 1D + Kadane's                 | 2 problems/day    | State definition is everything. Write `dp[i] = ?`in English before code.   |
| **Week 12** | DP 2D/Grid + Knapsack            | 2 problems/day    | Draw the dp table. Fill it manually for a small example.                     |
| **Week 13** | DP LCS/LIS + Intervals           | 2 problems/day    | LCS/LIS are pattern templates — memorize the recurrence, not the code.      |
| **Week 14** | Greedy + Intervals (merge/sweep) | 2–3 problems/day | Greedy = make the locally optimal choice. Prove it works before coding.      |
| **Week 15** | Math & Bit Manipulation          | 2 problems/day    | XOR tricks, power of 2 checks, bit counting — know 10 core tricks cold.     |
| **Week 16** | Advanced (Bitmask DP, KMP, etc.) | 1–2 problems/day | Only if targeting top-tier companies. Skip and do mock interviews otherwise. |

**Week 16 Milestone:** You can recognize DP patterns within 2 minutes and write the recurrence. You're interview-ready.

---

## Phase 4: Mock Sprint (Weeks 17–20)

* **Daily:** 2 LeetCode problems under timed conditions (25–35 min each)
* **Twice a week:** Full mock interview on Pramp, Interviewing.io, or with a friend
* **Weekly:** Review all problems you got wrong that week — redo them the next day

---

---

# 🔁 The Revision System (Never Forget Again)

## Spaced Repetition Schedule

For every problem you solve, log it and revisit it on this schedule:

| Revision     | When             | What to Do                                                                                    |
| ------------ | ---------------- | --------------------------------------------------------------------------------------------- |
| **R1** | Day 1 (same day) | Re-read your solution and the key insight. Write it in your own words.                        |
| **R2** | Day 3            | Solve it again from scratch without looking at your solution.                                 |
| **R3** | Day 7            | Solve it again. If you do it in under 15 mins (Easy) / 25 mins (Medium), mark it ✅ mastered. |
| **R4** | Day 30           | One final check. If clean → move to "mastered" pile. If not → reset to R2.                  |

**Tool recommendation:** Use Anki, Notion, or even a plain spreadsheet with columns: `Problem | Pattern | Date Solved | R1 | R2 | R3 | R4 | Status`.

---

## The Pattern Flashcard (Front & Back)

**Front of card:**

```
Pattern: Sliding Window
Trigger: Longest/shortest contiguous subarray with condition X
```

**Back of card:**

```
Template:
  left = 0, state = {}
  for right in range(n):
    add arr[right] to state
    while window invalid:
      remove arr[left] from state
      left++
    update answer

Key problems: LC #3, LC #76, LC #239
Complexity: O(n) time, O(k) space
```

**Test yourself on:** Can you write the template blindfolded? Can you say when to use it in one sentence?

---

## How to Know You've TRULY Mastered a Pattern

You've mastered it when you can do **all three** of these:

1. **Recognize it cold:** Someone describes a problem you've never seen in 10 seconds and you correctly identify the pattern
2. **Write the template from memory** in under 2 minutes with no bugs
3. **Solve a new Medium** in that pattern under 20 minutes on your first attempt

If you can only do #2 but not #1 and #3 — you've memorized, not learned. Go solve 3 more new problems in that pattern.

---

## When You Blank on a Problem You've Solved

Don't panic. Follow this protocol:

1. **Read the problem constraints** — they almost always hint at the pattern (sorted array → binary search, subarray → sliding window, etc.)
2. **Ask: "What's the brute force?"** — then ask "What's wasteful about it?" — the answer points to the pattern
3. **Draw a small example by hand** — your subconscious remembers more than your conscious mind
4. **If still stuck after 10 min:** Look only at the  *pattern name* , not the solution. Try again.
5. **If still stuck after 20 min:** Read only the key insight. Code it yourself.

Blanking is normal. The goal is shortening the blank time from 20 min → 5 min → 30 sec.

---

## Before-Interview-Week Sprint Plan

Do this the week before any interview:

| Day             | Activity                                                                              |
| --------------- | ------------------------------------------------------------------------------------- |
| **Day 1** | Re-solve 2 problems from each of: Arrays, Two Pointers, Sliding Window, Binary Search |
| **Day 2** | Re-solve 2 problems from each of: Trees BFS, Trees DFS, Graphs                        |
| **Day 3** | Re-solve 2 problems from each of: Heap, Backtracking, Stack                           |
| **Day 4** | Re-solve 2 problems from each of: DP 1D, DP 2D, Knapsack                              |
| **Day 5** | Full mock interview × 2. Focus on talking out loud.                                  |
| **Day 6** | Review only your flashcard fronts (pattern triggers). Rest. No new problems.          |
| **Day 7** | Interview day. You're ready.                                                          |
