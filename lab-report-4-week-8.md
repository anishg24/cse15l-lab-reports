# CSE 15L Lab Report 4

## Purpose
To realize that my code has a lot more work to be done.

## Sauce?
1. [My Markdown Parser](https://github.com/anishg24/markdown-parse)
2. [Reviewed Markdown Parser](https://github.com/codyprupp/markdown-parse)

## Markdown Grading Rubric
We will implement a common grading scheme seen in WI 22's CSE 12 class, either you get it right and get all the points, and if you so much as get one thing wrong you drop 4 letter grades.

The correct output that we expect from each input file is provided from the [CommonMark](https://spec.commonmark.org/dingus/) specification. Specifically, whatever is in the `href` attribute for any and all `<a>` tags.

**INPUT 1:**
```markdown
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```
**OUTPUT 1:**
`["\`google.com", "google.com", "ucsd.edu"]`

**INPUT 2:**
```markdown
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```
**OUTPUT 2:**
`["a.com", "a.com(())", "example.com"]`

**INPUT 3:**
```markdown
[this title text is really long and takes up more than
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than
one line](
    https://ucsd-cse15l-w22.github.io/
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



)

And then there's more text
```
**OUTPUT 3:**
`["https://ucsd-cse15l-w22.github.io/"]`

## The Examination Room
How do we test our programs with the inputs above? We will use the following code with a common Java testing framework: JUnit.

```java
// MarkdownParseTest.java
public class MarkdownParseTest {
    private final String MARKDOWN_FILE_LOCATION = "src/test/resources/markdown";
    private final ArrayList[] expectedResults = new ArrayList[]{
      // 10 other elements...
      new ArrayList<>(List.of("`google.com","google.com", "ucsd.edu")), // 11
      new ArrayList<>(List.of("a.com", "a.com(())", "example.com")),    // 12
      new ArrayList<>(List.of("https://ucsd-cse15l-w22.github.io/"))    // 13
    };

    private ArrayList<String> getLinks(int fileNumber) throws IOException {
        Path file = Path.of(MARKDOWN_FILE_LOCATION + "/test-file-" + fileNumber + ".md");
        return MarkdownParse.getLinks(Files.readString(file));
    }

    @Test
    public void testFile11() throws IOException {
        int fileNumber = 11;
        assertEquals(expectedResults[fileNumber - 1], getLinks(fileNumber));
    }

    @Test
    public void testFile12() throws IOException {
        int fileNumber = 12;
        assertEquals(expectedResults[fileNumber - 1], getLinks(fileNumber));
    }

    @Test
    public void testFile13() throws IOException {
        int fileNumber = 13;
        assertEquals(expectedResults[fileNumber - 1], getLinks(fileNumber));
    }
}
```
Using these 3 test methods, we can use it for my parser as well as the reviewer, with the only thing we need to change being the `MARKDOWN_FILE_LOCATION` constant. The code works exceptionally under assumption that the files are named sequentially as `test-file-11.md`, `test-file-12.md`, `test-file-13.md`. Each ones digit lines up with the input number in the *Markdown Grading Rubric* section

## Admitting My Mistakes
This is hard for me to do, but I have some work cut out for me, as we can see in the following comparisons:

`test-file-11.md: Failed`
```
java.lang.AssertionError:
Expected :[`google.com, google.com, ucsd.edu]
Actual   :[url.com, `google.com, google.com, ucsd.edu]
```

`test-file-12.md: Failed`
```
java.lang.AssertionError:
Expected :[a.com, a.com(()), example.com]
Actual   :[a.com, a.com((, example.com]
```

`test-file-13.md: Failed`
```
java.lang.AssertionError:
Expected :[https://ucsd-cse15l-w22.github.io/]
Actual   :[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/]
```

## Admitting Someone Else's Mistakes
This part is easier to do, here are the results:

`test-file-11.md: Failed`
```
java.lang.AssertionError:
Expected :[`google.com, google.com, ucsd.edu]
Actual   :[url.com, `google.com, google.com, ucsd.edu]
```

`test-file-12.md: Failed`
```
java.lang.AssertionError:
Expected :[a.com, a.com(()), example.com]
Actual   :[a.com, a.com((, example.com]
```

`test-file-13.md: Failed`
```
java.land.AssertionError:
Expected :[https://ucsd-cse15l-w22.github.io/]
Actual   :[
https://www.twitter.com
,
https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]
```
## Self Reflection
Let's see think about how we can better improve my code to pass all these test cases for each case above.

### How to Fix `test-case-11.md`
Right now my code doesn't account for any backtick handling, so the best first step would be to account for any backticks. The best way to do this is to assure that the `[]` aren't in a code block.

### How to Fix `test-case-12.md`
Something is wrong with how I detect closing parenthesis, so it would be in my best interest to debug the `getLinks` method and figure out what variables I am using to get a substring.

### How to Fix `test-case-13.md`
My code doesn't account for multiple line breaks. The best first step would be to first find any line breaks and based on if they have multiple line breaks, I should ignore that potential link area.
