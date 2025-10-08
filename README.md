# LEETCODE-Arrays-2300
Let’s **deeply dry run** it, step-by-step, so you understand exactly *what’s happening under the hood* — like how `l`, `r`, and `mid` change during the binary search.

---

## 🧩 Code Recap

```java
class Solution {
    public int[] successfulPairs(int[] spells, int[] potions, long success) {
        int n = spells.length;
        int m = potions.length;
        int[] x = new int[n];
        Arrays.sort(potions); // must be sorted for binary search

        for (int i = 0; i < n; i++) {
            x[i] = potions.length - popx(spells[i], potions, success);
        }
        return x;
    }

    public int popx(int spell, int[] potions, long success) {
        int l = 0;
        int r = potions.length;

        while (l < r) {
            int mid = (l + r) / 2;
            if ((long) spell * potions[mid] >= success)
                r = mid;   // move left → looking for FIRST success
            else
                l = mid + 1; // move right → skip failing potions
        }

        return l; // index of first success (or potions.length if none)
    }
}
```

---

## 🧠 Example Input

```java
spells = [5, 1, 3]
potions = [1, 2, 3, 4, 5]
success = 7
```

We’ll track:

* For each `spell`
* The binary search process (`l`, `r`, `mid`, product)
* Then calculate the final count of successful pairs

---

## ⚙️ Step 1: Sort `potions`

```
potions = [1, 2, 3, 4, 5]
```

(Already sorted ✅)

---

## ⚡ Spell = 5

Goal: find first index where
`5 * potions[mid] >= 7`

Initial values:

```
l = 0
r = 5
```

### 🔁 Binary Search Trace

| Iter |  l |  r | mid | potions[mid] | 5*potions[mid] | ≥7?   | Action        |
| ---- | -: | -: | --: | -----------: | -------------: | ----- | ------------- |
| 1    |  0 |  5 |   2 |            3 |             15 | ✅ yes | r = mid → 2   |
| 2    |  0 |  2 |   1 |            2 |             10 | ✅ yes | r = mid → 1   |
| 3    |  0 |  1 |   0 |            1 |              5 | ❌ no  | l = mid+1 → 1 |

Loop ends → `l = 1`

So, **first success index = 1**

Now,

```
successful potions = 5 (total) - 1 = 4
```

✅ Successful potions are `[2, 3, 4, 5]`.

So, `x[0] = 4`

---

## ⚡ Spell = 1

Goal: `1 * potion >= 7`

Initial:

```
l = 0, r = 5
```

### 🔁 Binary Search Trace

| Iter |  l |  r | mid | potions[mid] | 1*potions[mid] | ≥7?  | Action        |
| ---- | -: | -: | --: | -----------: | -------------: | ---- | ------------- |
| 1    |  0 |  5 |   2 |            3 |              3 | ❌ no | l = mid+1 → 3 |
| 2    |  3 |  5 |   4 |            5 |              5 | ❌ no | l = mid+1 → 5 |

Loop ends → `l = 5`

So, **first success index = 5**

Now,

```
successful potions = 5 - 5 = 0
```

✅ No potions successful.

So, `x[1] = 0`

---

## ⚡ Spell = 3

Goal: `3 * potion >= 7`

Initial:

```
l = 0, r = 5
```

### 🔁 Binary Search Trace

| Iter |  l |  r | mid | potions[mid] | 3*potions[mid] | ≥7?   | Action        |
| ---- | -: | -: | --: | -----------: | -------------: | ----- | ------------- |
| 1    |  0 |  5 |   2 |            3 |              9 | ✅ yes | r = mid → 2   |
| 2    |  0 |  2 |   1 |            2 |              6 | ❌ no  | l = mid+1 → 2 |

Loop ends → `l = 2`

So, **first success index = 2**

Now,

```
successful potions = 5 - 2 = 3
```

✅ Successful potions are `[3, 4, 5]`.

So, `x[2] = 3`

---

## 🧾 Final Output

```
x = [4, 0, 3]
```

✅ Matches the expected correct answer.

---

## 🧩 Understanding the Key Line

```java
x[i] = potions.length - popx(spells[i], potions, success);
```

* `popx(...)` → finds **first successful potion’s index**.
* All potions after that index are successful (because array is sorted).
* So the number of successful ones = `total - index`.

Example for `spell = 3`:

```
first success at index 2 → potions.length(5) - 2 = 3
=> 3 successful potions
```

---

## 🧮 Final Summary Table

| Spell | firstIndex | Successful potions | Successful values | x[i] |
| ----- | ---------- | ------------------ | ----------------- | ---- |
| 5     | 1          | 4                  | [2,3,4,5]         | 4    |
| 1     | 5          | 0                  | []                | 0    |
| 3     | 2          | 3                  | [3,4,5]           | 3    |

Final Output → ✅ `[4, 0, 3]`

---
