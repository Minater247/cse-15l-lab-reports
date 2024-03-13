# Lab Report 5 - Putting it All Together

## Part 1 - Debugging Scenario

### Student Post:
Hi all,
<br>
I mostly completed the code for lab 11, but I'm getting some weird entries that aren't words in my output. I'm guessing it has something to do with the regex, but this should be matching every letter and number in a word.

![image](https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/bd9e29cf-544e-49d4-bba7-6dc53429d97e)

### TA Response:
You're right in assuming the problem lies in the regex - the pattern you used appears to not match apostrophes, leading to some problems with words such as "that's" (which would split into "that" and "s"). Maybe another pattern in the [Java regex documentation](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html) could help? (Hint: The \s pattern in the "Predefined character classes" section seems interesting here...)

### Student Information from Changes:
![image](https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/86a4c37c-cf89-4dc5-bf88-14549d27fc6f)

The bug, in this case, was that the student was using a regex pattern incorrect for the given application. Behind the scenes, they were using some sort of pattern that matched only letters and numbers (such as `[\\W-]+`) rather than something that splits at whitespace to locate words. Splitting at whitespace still causes issues due to punctuation such as periods or commas, so we'll assume the student has implemented a proper regex pattern such as `[\\W-\\.\\,]+`, which caused the bug.

### Setup Information

Directory Structure:
```tree
.
├── contents/
│   ├── beemovie.txt
│   └── cars2.txt
├── Reader.java
└── common_in_dir.sh
```

`contents/beemovie.txt` contains the entire script to the Bee Movie, as defined at [this link](https://gist.githubusercontent.com/MattIPv4/045239bc27b16b2bcf7a3a9a4648c08a/raw/2411e31293a35f3e565f61e7490a806d4720ea7e/bee%2520movie%2520script)

`contents/cars2.txt` contains the entire script to Cars 2, as defined at [this link](https://s3-us-west-2.amazonaws.com/script-pdf/cars-2-script-pdf.pdf)

`Reader.java` contains the following code:
```java
// Read the contents of the files in contents/ and locate the top 10 most frequent words in the files.
// The words are case-insensitive and the output should be in lower case.

// Each file should say:
/*
    File: <filename>
    <word1> <count>
    <word2> <count>
    ...
    <word10> <count>
 */

import java.io.*;
import java.nio.file.*;
import java.util.*;

public class Reader {
    private static final int TOP_WORDS = 10;

    public static void main(String[] args) throws IOException {
        if (args.length != 1) {
            System.out.println("Usage: java Reader <filename>");
            return;
        }
    
        Path file = Paths.get(args[0]);
        System.out.println("File: " + file.getFileName());
        Map<String, Integer> wordCounts = countWords(file);
        printTopWords(wordCounts, TOP_WORDS);
    }

    private static Map<String, Integer> countWords(Path file) throws IOException {
        Map<String, Integer> wordCounts = new HashMap<>();
        try (BufferedReader reader = Files.newBufferedReader(file)) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] words = line.toLowerCase().split("[\\W-\\.\\,]+");
                for (String word : words) {
                    if (!word.isEmpty()) {
                        wordCounts.put(word, wordCounts.getOrDefault(word, 0) + 1);
                    }
                }
            }
        }
        return wordCounts;
    }

    private static void printTopWords(Map<String, Integer> wordCounts, int topWords) {
        PriorityQueue<Map.Entry<String, Integer>> pq = new PriorityQueue<>(
                (a, b) -> a.getValue().equals(b.getValue())
                        ? b.getKey().compareTo(a.getKey())
                        : a.getValue() - b.getValue()
        );

        for (Map.Entry<String, Integer> entry : wordCounts.entrySet()) {
            pq.offer(entry);
            if (pq.size() > topWords) {
                pq.poll();
            }
        }

        List<String> topWordsList = new ArrayList<>();
        while (!pq.isEmpty()) {
            Map.Entry<String, Integer> entry = pq.poll();
            topWordsList.add(0, entry.getKey() + " " + entry.getValue());
        }

        for (String wordCount : topWordsList) {
            System.out.println(wordCount);
        }
    }
}
```

`common_in_dir.sh` contains the following script:
```bash
# Bash script to run Reader.java on every file in contents/

javac Reader.java

for file in contents/*; do
    echo "Running Reader.java on $file"
    java Reader $file
done
```

<hr>

To trigger the bug, I ran the following command:
`bash common_in_dir.sh`

<hr>

The fix for the issue was actually quite simple, despite the complex-sounding issue - by changing the `\\W` in the regex pattern to `\\s`, the pattern now matches every non-whitespace character instead of every letter and number. By then excluding a few punctuation marks, as the student had done, the pattern correctly matches all the words on a line!
