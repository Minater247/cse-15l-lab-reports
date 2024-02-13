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
This fixes the issue because we had originally been copying from the same array we were overwriting. For example, given 4 items, we would copy item 3 to item 0 and item 2 to item 1, then try to read item 1 to item 2 and item 0 to item 3. Since we already overwrote these items, the numbers will be incorrect, and we will end up with a mirrored list. While this is an interesting effect, it is not the intended result, and copying from a clone of the array which we do not change resolves the issue.

# Part 2 - Researching Commands
I found the command `grep` very interesting! Here are a few interesting flags/use cases for the command:

<hr>

The `-E` and `--extended-regexp` flags allow using extended regular expressions to search for a specific pattern of characters. This is useful for locating more complex patterns, such as - in this case - things that are related to endotoxins, but that are not endotoxins themselves. This command searches through the `./technical/biomed/` directory for all `.txt` files, and uses the pattern `'endotoxin-[[:alnum:]]+'` to locate all hyphenated words starting with 'endotoxin'. Notably, being 'extended' means that several additional flags are available - such as a logical OR and the :alnum: field.
```console
[n2reed@ieng6-201]:docsearch:401$ grep -E 'endotoxin-[[:alnum:]]+' ./technical/biomed/*.txt
./technical/biomed/1471-2121-3-11.txt:        other cell types such as endotoxin-stimulated monocytes [
./technical/biomed/1471-2202-2-3.txt:          at -80°C in endotoxin-free siliconized microfuge tubes
./technical/biomed/1476-9433-1-2.txt:        endotoxin-stimulated macrophages release soluble lymphocyte
./technical/biomed/1476-9433-1-2.txt:        mitogen/endotoxin-stimulated macrophage-derived products,
./technical/biomed/rr171.txt:        pathophysiology of endotoxin-induced lung injury [ 22 ] .
./technical/biomed/rr171.txt:        endotoxin-induced increases in neutrophil TNF-α, as well as
./technical/biomed/rr171.txt:        these signaling molecules in endotoxin-induced inflammatory
```

<hr>

Another application of the `-E` flag is searching for emails throughout a document! This command searches through the `./technical/biomed/` directory for all `.txt` files again, but this time checks for the pattern `[[:alnum:]._-]+@[[:alnum:].]+`, which matches an email with letters, numbers, underscores, and/or hyphens in the username, followed by an `@` symbol, and finally any number of alphanumeric characters as well as a dot. This isn't a very robust email finder, but it does fully find all emails in this folder.
```console
[n2reed@ieng6-201]:docsearch:415$ grep -E '[[:alnum:]._-]+@[[:alnum:].]+' ./technical/biomed/*.txt
./technical/biomed/1471-2105-3-2.txt:        , "A Story" [ 63 ] , and "AA.AG@helix.ends" [ 64 ] ) and
./technical/biomed/1471-2148-1-4.txt:          should be addressed to S.B.H. (e-mail: sbh1@psu.edu) or
./technical/biomed/gb-2002-3-9-research0046.txt:          mged-mage@lists.sourceforge.net, which is available as a
```
Additionally, here is a screenshot so the highlighted sections, with color, are visible.
![image](https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/747095e9-8b2b-4fd7-ba9b-cc99e032f3c8)

<hr>

The `-c` and `--count` flags are also very useful, as they allow you to output the number of matching indices! In this case, I am searching for the same pattern as the first command, but now checking how many entries the file contains. The use of `awk` here is only to remove entries with 0 instances of `endotoxin-*`. This is useful for figuring out which files contain more instances of endotoxin-*related* words, for applications such as research papers or locating experts on the topic.
```console
[n2reed@ieng6-201]:docsearch:472$ grep -Ec 'endotoxin-[[:alnum:]]+' ./technical/biomed/*.txt | awk -F: '$NF!=0{print $0}'
./technical/biomed/1471-2121-3-11.txt:1
./technical/biomed/1471-2202-2-3.txt:1
./technical/biomed/1476-9433-1-2.txt:2
./technical/biomed/rr171.txt:3
```

<hr>

The `-c` and `--count` flags have another interesting use-case: we can use them to replace the line-count function of `wc`! This is of course useful, as it allows us to determine the reading time and general size of a file from the command line. The use of `awk` here simply sums up the output of `grep`, and does no more complex calculations. The empty pattern of `''` passed to grep will match anything, meaning every line matches - and since the -c prints the number of matching lines, this counts the number of lines in the file.
```console
[n2reed@ieng6-201]:docsearch:474$ grep -c '' ./technical/biomed/*.txt | awk -F: '{total += $2} END {print "Total number of lines in biomed research papers:", total}'
Total number of lines in biomed research papers: 490673
```
