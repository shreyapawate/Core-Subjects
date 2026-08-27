````markdown
 11. Container With Most Water

Question

Given an integer array `height`, where each element represents the height of a vertical line, find two lines that together with the x-axis form a container that holds the **maximum amount of water**.

Example

text
Input:  [1,8,6,2,5,4,8,3,7]
Output: 49
````

The maximum area is formed using heights `8` and `7`.

```text
Area = min(8,7) × (8-1)
     = 7 × 7
     = 49
```

---

## 1. Brute Force

### Idea

Try every possible pair of lines.

For every `i` and `j`:

```text
height = min(height[i], height[j])
width = j - i

area = height × width
```

Keep track of the maximum area.

### Complete Java Code

```java
public class Main {
    public static int maxArea(int[] height) {
        int maxArea = 0;

        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int h = Math.min(height[i], height[j]);
                int width = j - i;
                int area = h * width;

                maxArea = Math.max(maxArea, area);
            }
        }

        return maxArea;
    }

    public static void main(String[] args) {
        int[] height = {1, 8, 6, 2, 5, 4, 8, 3, 7};

        System.out.println(maxArea(height));
    }
}
```

### Complexity

```text
Time:  O(n²)
Space: O(1)
```

---

## 2. Better Approach

There is no commonly useful distinct "better" asymptotic approach between brute force and two pointers for this problem.

The standard progression is:

```text
Brute Force → Two Pointers
O(n²)       → O(n)
```

So the **Two Pointer approach is the optimal approach**.

---

# 3. Optimal — Two Pointers ⭐

## Idea

Use two pointers:

```text
left  = 0
right = n - 1
```

Calculate:

```text
area = min(height[left], height[right]) × (right - left)
```

Then move the pointer having the **smaller height**.

### Why move the smaller pointer?

The area depends on:

```text
min(height[left], height[right]) × width
```

When we move a pointer, the width always decreases.

So moving the taller line cannot improve the limiting height.

Therefore, move the **smaller height** hoping to find a taller line.

---

## Complete Java Code

```java
public class Main {
    public static int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            int h = Math.min(height[left], height[right]);
            int width = right - left;

            int area = h * width;

            maxArea = Math.max(maxArea, area);

            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }

    public static void main(String[] args) {
        int[] height = {1, 8, 6, 2, 5, 4, 8, 3, 7};

        System.out.println(maxArea(height));
    }
}
```

---

## Dry Run

```text
height = [1,8,6,2,5,4,8,3,7]
```

Start:

```text
left = 0 → height = 1
right = 8 → height = 7
```

### Step 1

```text
height = min(1,7) = 1
width = 8

area = 1 × 8 = 8

1 is smaller → left++
```

### Step 2

```text
left = 1 → height = 8
right = 8 → height = 7

height = 7
width = 7

area = 7 × 7 = 49

7 is smaller → right--
```

Continue moving the smaller pointer.

The maximum found is:

```text
49
```

### Output

```text
49
```

---

## Complexity

```text
Time:  O(n)
Space: O(1)
```

## Interview Explanation

> "I use two pointers at both ends of the array. I calculate the area using the smaller height multiplied by the distance between the pointers. Then I move the pointer with the smaller height because the width will decrease, so only increasing the smaller height can potentially give us a larger area."

```
```
