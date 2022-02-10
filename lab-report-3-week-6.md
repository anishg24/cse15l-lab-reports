# CSE 15L Lab Report 3

## Purpose
To avoid using GitHub because its owned by Microsoft, and instead `scp` our local code anywhere we want.

## Write Some Code
To copy your code everywhere you want, you will first need some code to even copy. So let's use a watered down and featureless `MarkdownParser` developed for college students.

![i1](/assets/sc-3-1.png)

Woah! That's a lot of files! I'd really love to not `scp` each file one by one over to my UCSD remote server! So lets make like a college algorithms class and use **recursion**.

## Recursively Copying Our Code
Alright, so recusively copying our files. How do we do that? ~~Obviously, we will write a C program to do this. We need a function to read all the files in a directory, then call the `scp` command on each file.~~ Turns out, programmers are incredibly lazy and we already have added this functionality, and all we have to do is type 2 extra characters, `-r`.

The `-` denotes a **flag**. Flags are commonly used in command line applications to provide extra features based on user input. It allows the user tell the program everything from what arguments we are passing in to how the program should run.

The `r` stands for **recursive**. In context of `scp` or even `cp`, it means to recursively copy the files in a directory to a destination.

So let's use other people's work and claim its our own hard work.
```
scp -r . <username>@ieng6.ucsd.edu:~/markdown-parse
```
where `<username>` is your UCSD assigned username. You should see something like this:

![i2](/assets/sc-3-2.png)

Now that we copied the code over, what should we do? Well what else do you do with code buddy? Make like an athelete and run it.

## Execute Order 66
Alright lets run our code. We first need to log into our server and run the file we want to. In our specific program, the code that we are interested in executing is `make test`. To those massive brains who actually read images, we have a `makefile` in the code's directory. We will be running that make file to run the JUnit testers that we implemented in the same project.

Now lets do this in one line shall we?
```
ssh <username>@ieng6.ucsd.edu "cd ~/markdown-parse; make test"
```
and we should see something like:
![i2](/assets/sc-3-3.png)
Now it will say that there is an error, and this is normal because the developer of this program (Anish) doesn't know what he's doing, and probably just forgot to import something correctly in his code.

But the point of this was that I should be able to remotely run my code without the use of the evil Microsoft overlords at GitHub. If I change my code locally, I'll have to do everything over again! Well yes, you're 100% right about that. But lets make it easier for your fragile and fraile fingers shall we?

## An Ineffective Attempt to Prevent Early Arthritis
Let's combined everything we worked into one easy command that we can run to both copy our code and run it on the server.

```
scp -r . <username>@ieng6.ucsd.edu:~/markdown-parse; ssh <username>@ieng6.ucsd.edu "cd ~/markdown-parse; make test""
```

This should combine everything we just did into one command that we can run, and get the following:
![i3](/assets/sc-3-4.png)
