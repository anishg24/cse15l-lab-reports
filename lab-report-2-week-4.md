# CSE 15L Lab Report 2

## Purpose
To test our code efficiently.

**ALL "symptoms" ARE COMMENTED IN COMMIT HISTORY**

## Iteration 1 - Solving the Infinite Loop

[**Commit:**](https://github.com/anishg24/markdown-parse/commit/16d6850ce86eaa9b114852f2e7d49276ab901896)

![i1](https://anishg24.github.io/cse15l-lab-reports/assets/sc-2-1.png)

[**Failure Inducing File:**](https://github.com/anishg24/markdown-parse/blob/960724abcafe7b915051b8a361d02ff4e514bf87/breaking-test-file.md)
```markdown
# This test file will break the code

Sometimes I want to create some mayhem.
So lets take a look at this matrix: `[a b c d]` which is very trivial
(but not really). It is a cool matrix.
```
[**Symptom:**](https://github.com/anishg24/markdown-parse/commit/16d6850ce86eaa9b114852f2e7d49276ab901896#commitcomment-64745789)
```
$ javac -cp .:lib/junit-4.13.2:lib/hamcrest-core-1.3.jar MarkdownParse.java
$ java -cp .:lib/junit-4.13.2:lib/hamcrest-core-1.3.jar MarkdownParse.java

```
*(Program would hang in an infinite loop)*

The bug in this code was finding a valid breaking point for our while loop. In
the original provided code, the breaking point was reached because the test file
had a link at the end of the file. This file however has no links, and even then
it's not at the end of the file. Because of this, `currentIndex` would remain
within the bounds of the `while`-loop and would cause an infinite loop.

## Iteration 2 - Grabbing valid links

[**Commit:**](https://github.com/anishg24/markdown-parse/commit/077333c995f5811d17ec11903d32fb01c45f9906)

![i2](https://anishg24.github.io/cse15l-lab-reports/assets/sc-2-2.png)


[**Failure Inducing File:**](https://github.com/anishg24/markdown-parse/blob/077333c995f5811d17ec11903d32fb01c45f9906/breaking-test-file.md)
```markdown
# This test file will break the code

Sometimes I want to create some mayhem.
So lets take a look at this matrix: `[a b c d]` which is very trivial
(but not really). It is a cool matrix.

[Hello World](https://www.google.com)

There's More.
```

[**Symptom:**](https://github.com/anishg24/markdown-parse/commit/077333c995f5811d17ec11903d32fb01c45f9906#commitcomment-64746192)
```
$ javac -cp .:lib/junit-4.13.2:lib/hamcrest-core-1.3.jar MarkdownParse.java
$ java -cp .:lib/junit-4.13.2:lib/hamcrest-core-1.3.jar MarkdownParse.java
["but not really", "https://google.com"]
```

The bug in this code was that we were looking for `]` and `(` separately. While this can work in most cases, in cases where the `]` and `(` are more than 1 character apart, this will fail. It will return whatever is in the parentheses, even if it isn't meant to be returned. In our breaking test file, we can see this happen as we have a pair of square brackets and parentheses separated by 4 words. When we ran our old code, we'd get "but not really" even though we shouldn't.

## Iteration 3 - Seeing the bigger picture

[**Commit:**](https://github.com/anishg24/markdown-parse/commit/503b22f51cd69232ba55153130f10a58dfe08934)

![i3](https://anishg24.github.io/cse15l-lab-reports/assets/sc-2-3.png)

[**Failure Inducing File:**](https://github.com/anishg24/markdown-parse/blob/main/src/test/resources/markdown/test-file-7.md)
```markdown
# title

![link](page.com)
```

[**Symptom:**](https://github.com/anishg24/markdown-parse/commit/503b22f51cd69232ba55153130f10a58dfe08934#commitcomment-64746622)
```
$ javac -cp .:lib/junit-4.13.2:lib/hamcrest-core-1.3.jar MarkdownParse.java
$ java -cp .:lib/junit-4.13.2:lib/hamcrest-core-1.3.jar MarkdownParse.java
["page.com"]
```

The bug in this code is that we aren't checking if the provided link is an image or not. Our file provides a valid link, but the link is for an image. Since we don't check if the link is an image, our old code prints out this link despite us not wanting to.
