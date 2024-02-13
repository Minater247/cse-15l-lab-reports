# Lab Report 3 - Bugs and Commands

## Part 1 - Bugs
For this section, I chose to do the bug in the function reverseInPlace.

One failure-inducing input is an array containing items 0-99:
```java
    @Test
    public void testReverseInPlace() {
        int[] input = new int[100];
        for (int i = 0; i < 100; i += 1) {
            input[i] = i;
        }
        ArrayExamples.reverseInPlace(input);
        for (int i = 0; i < 100; i += 1) {
            assertEquals(99 - i, input[i]);
        }
    }
```

<hr>

One functional, non-failure-inducing input is an array containing only one item:
```java
    @Test
    public void testReverseInPlaceOneItem() {
        int[] input = { 1 };
        ArrayExamples.reverseInPlace(input);
        assertEquals(1, input[0]);
    }
```

<hr>

The symptom ends up being that half the array is copied correctly, before it simply mirrors the array and outputs the incorrect information.
![image](https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/63dff5c2-985b-4686-9fac-0f7117cbcd0f)

<br>

The initial code was this, as copied from the original repository:
```java
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

<hr>

The fixed code, with revisions, was this:
```java
    static void reverseInPlace(int[] arr) {
        int[] arrCopy = arr.clone();
        for (int i = 0; i < arr.length; i += 1) {
            arr[i] = arrCopy[arr.length - i - 1];
        }
    }
```

<hr>

Additionally, I learned that it is possible to do a diff in markdown, so here is a diff of the changes between these two:
```diff
  static void reverseInPlace(int[] arr) {
+   int[] arrCopy = arr.clone();
    for (int i = 0; i < arr.length; i+= 1) {
-     arr[i] = arr[arr.length - i - 1];
+     arr[i] = arrCopy[arr.length - i - 1];
    }
  }
```
<hr>
This fixes the issue because we had originally been copying from the same array we were overwriting. For example, given 4 items, we would copy item 3 to item 0 and item 2 to item 1, then try to read item 1 to item 2 and item 0 to item 3. Since we already overwrote these items, the numbers will be wrong, and we will just end up with a mirrored list. While this is an interesting effect, it is not the intended result, and copying from a copy of the array which we do not change resolves the issue.

# Part 2 - Researching Commands
