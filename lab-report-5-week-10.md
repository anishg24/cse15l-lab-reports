# CSE 15L Lab Report 5

## Purpose
To realize that maybe reinventing the wheel isn't the best idea.

## Concerning Files
The two files that cause different outputs with different bugs that I chose are:

`test_files/194.md`
```
[Foo*bar\]]:my_(url) 'title (with parens)'

[Foo*bar\]]
```

`test_files/201.md`
```
[foo]: <bar>(baz)

[foo]
```

### Outputs
Provided in the table below are the outputs of the source code and the outputs of my code

|              | **Source Code** | **My Code** |
|--------------|-----------------|-------------|
| **`194.md`** |      [url]      |      []     |
| **`201.md`** |      [baz]      |      []     |

### The Method behind My Madness
I didn't choose these files at random. I first started by modifying the provided `script.sh` file:

```bash
set -e
for file in test-files/*.md;
do
  echo "$(basename "$file") -> $(java MarkdownParse "$file")"
done
```

This way, whenever a test file runs, the output will be printed as:
```markdown
XXX.md -> [...]
```

After modifying and copying the new `script.sh` file to my local repository as well, I am able to create an output file for each repository by typing:

```
$ ./script.sh > out.txt
```

This creates a text file that looks like the following:
```markdown
1.md -> [...]
...
XXX.md -> [...]
```

Then, I can use my IDE to compare the files for me in a neat and orderly fashion that I can use to find any different outputs:

![sc](/assets/sc-5-1.png)

Using this, I could easily find the difference between the two files and check them to see if they are valid ones I can use for this lab writeup.


## Winners
Winners are determined by comparing whats in `<a href="[LINK]">...</a>` according to the [CommonMark's Live Preview](https://spec.commonmark.org/dingus/) on `HTML` mode.

#### `194.md`
The winner for this markdown file is neither my implementation nor the source code's implementation. When running the file through  the live preview, the `href` attribute was `my_(url)`, neither of which was the output for any of the implementations

#### `201.md`
The winner for this markdown file is my implementation. When running the markdown file through the live preview, there were no `<a>` tags, meaning no links were detected. This would be similar to getting a `[]` which my code did output on this file.

So the winner? Seems to be my code with a 1-0 lead in this extremely biased competition.

## Become Bob the Builder
We can fix it!

#### Fixing `194.md`
Since both implementations were wrong, and because I am a very benevolent and humble person, I will choose to fix the source code's implementation. Not because I am ashamed of looking at my code again, but solely because I am very generous

Anyways, its clear that our bug lies in the original `getLinks()` method, so we will start by looking there.

Most of the issue lies with determining the value of `openParen`, as that is the place where the substring starts. Its imperative that we choose openParen correctly such that it will grab the important part of the text. I think we should start checking for `:` as well as that's where the CommonMark's answer seems to start.

#### Fixing `201.md`
Since my implementation is right, I have no choice but to review the source code again.

Once again, we will be within the original `getLinks()` method. The biggest issue I see here is that the code doesn't account for a space between the `]` and `(`. If there is a space between those two brackets, then we shouldn't even consider it to be a link. If we add this conditional, we can easily fix this bug.
