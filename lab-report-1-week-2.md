# CSE 15L Lab Report 1

## Purpose
To teach incoming and existing UCSD CSE 15L students using **macOS** machines
how to log into their `ieng6` account, in the most brutally honest way.

## Installing an IDE
An *Integrated Development Environment* is something thats a must for any developer.

The one I use and recommend is literally any one by (JetBrains)[https://www.jetbrains.com/].
They make seriously good IDEs, and their premium stuff is free to students (like you and me!).
You can apply for a free license (here)[https://www.jetbrains.com/shop/eform/students].
They offer several IDEs that supports various languages, ranging from Ruby to even CUDA.
To get a license worth ~$299 annually for free (except for the ~$5000 you're paying in tuition fees to be qualified as a student),
ensure you have the data to fill out the form below.

![JetBrains](https://anishg24.github.io/cse15l-lab-reports/assets/sc3.png)

What lots of people will say they use is (Visual Studio Code)[https://code.visualstudio.com/].
Visual Studio Code is a **code editor, not an IDE**. What's the difference? An IDE contains a code editor,
but a code editor doesn't contain an IDE.
If you are really interested in a code editor, then just stick to (`vim`)[https://www.vim.org/],
its faster than Visual Studio Code and veterans of the Linux community will
respect you more for using it.

Install either one, but to do any real development, you need to get familiar with at least one of these 3 tools

## Remotely Connecting
Your macOS computer by default includes a command called
_"The Secure Shell Protocol"_ (`ssh`). This is what we will be using to access
the remote server.

But what server? Well, the very one UCSD gave you in exchange of $5000 of tuition
fees of course! How do you get the most out of your money?

### Getting Your `ieng6` Account
1. Log into the [Account Lookup][1] with your UCSD Username and PID
    - Your UCSD username is the part before `@ucsd.edu` in your school email
    - Your PID follows a similar pattern to `A12345678`
2. Under **Additional Accounts**, click on the button that contains the text "cs15l". Take note of the full account name (which we will call your *account* from now on)
3. You will see a yellow warning pop up saying to change your password. Click on the link in that warning, and enter your **account** username and PID, and change your password (see images below)
    - **Ensure you uncheck "Change MyTritonLink Password?"** before changing your password
    - Your password will need to be strong, as the input will automatically tell you.
    - Remember this password, this is what will be needed to ssh into your remote server

![Warning](https://anishg24.github.io/cse15l-lab-reports/assets/sc1.png)
![Login](https://anishg24.github.io/cse15l-lab-reports/assets/sc2.png)

### Accessing Your Remote Server
1. Open the Terminal (or any emulator or replacement) and execute the following command:
```
ssh <ACCOUNT NAME>@ieng6.ucsd.edu
```
2. When prompted for a password, enter the password you set. Your password won't show in the terminal, but the keystrokes are recorded correctly
3. When prompted if you want to continue connecting, type `yes`.

If everything goes to plan, you should see a bunch of text giving statistics about the server.
After that, you should be in your remote account! Now what?

## Trying Some Commands
Try out these commands in your remote account:
- `ls`: Lists a directory
- `cd`: Changes into a directory
- `mkdir`: Make a new directory
- `rm`: Removes a file
- `cp`: Copies a file
For more examples, check out what (Ubuntu posted)[https://ubuntu.com/tutorials/command-line-for-beginners#1-overview]!
You can see below (don't worry if your terminal doesn't look like mine) of some commands run
![Examples](https://anishg24.github.io/cse15l-lab-reports/assets/sc4.png)

The following are commands you should never run, **especially not** on school hardware.
Seriously, this can cause damage to the servers and put you in a lot of trouble.
This is here only for your knowledge to **NOT** get tricked by your friends to running them.
***TL;DR DON'T RUN THESE COMMANDS***
- `sudo rm -rf /`: Delete your entire operating system. **Don't run this command.**
- `:(){ :|:& };:`: Creates a program that eats up all resources on the computer. **Don't run this command**

Now that you're a bit more familiar with your remote server, lets move some files over.

## Moving Files With SCP
No, we aren't talking about the (SCP Foundation)[https://en.wikipedia.org/wiki/SCP_Foundation]. This is
the *Secure CoPy* (`scp`) protocol. We use `scp` to transfer files from our computer to the remote server.
To do this, we need to run:
```
scp file.txt <ACCOUNT NAME>@ieng6.ucsd.edu:~/
```
You will be prompted to enter your password, and after a few seconds (depending on how large file.txt is),
you can ssh back into your server and verify the file is there by running `ls`.

An example of moving a C++ file over:
![C++](https://anishg24.github.io/cse15l-lab-reports/assets/sc5.png)

But logging in and entering your password is very annoying is it not? We can fix that by creating some

## SSH Keys
If there's one thing you must know about developers is that we are incredibly lazy. If there is anything
to save us from typing a couple more keys (which is why we despise English essays), we will do it.
SSH keys are a testament to this fact.

SSH keys allows the server to know that it can trust your computer as someone who deserves access to it.
Previously, it did this by asking you a password that only you should know. Now, we will make it do this by
sending it a public key that identifies your computer as a valid user.

To start we will run `ssh-keygen` on your **local** computer. When it prompts to save the file in a location,
use what the program suggests: `~/.ssh/id_rsa`. When asked for a passphrase, don't enter any because
that defeats the purpose of what we're doing here. Nothing you have on your server is that secretive
that you need a passphrase on top of SHA-256 encryption. If you do, I have no idea why you're looking at this guide.

![Key](https://anishg24.github.io/cse15l-lab-reports/assets/sc7.png)

After you run `ssh-keygen`, it will create 2 files on your computer,
- `~/.ssh/id_rsa`: Your private key
- `~/.ssh/id_rsa.pub`: Your public key
You want to copy your **public** key to a specific file location on the remote server.
The best way to do this? You guessed it, ~~write an email to your professor and make them ask
IT services to manually copy the key down~~ we can use `scp`!

Simply run `scp ~/.ssh/id_rsa.pub <ACCOUNT NAME>@ieng6.ucsd.edu:~/.ssh/authorized_keys`. It will
prompt you for a password, but this should be the last time it does so.

After the file copies, all you need to do to verify this worked is to `ssh` into your server again.
If it doesn't ask you for a password, then you did it!

## Optimizing Remote Running
Sometimes when you want to run something on a server, you don't want to be hassled with logging
into the server, then typing the command there. Don't worry, us lazy developers figured that out too!
You can run:
```
ssh <ACCOUNT NAME>@ieng6.ucsd.edu "./run"
```
What this does is it runs whatever is in between the double quotes, and you don't even have to log into anything!

But what if you're using a compiled language, like C++ or FORTRAN? How can you compile, link, and run all at the same time?
Well, we can use something very predominant in C++, the semicolon.
```
ssh <ACCOUNT NAME>@ieng6.ucsd.edu "gcc main.cpp -o main; ./main"
```
Notice the addition of the semicolon. That essentially means, you can write as many lines of commands as you want, delimited
by a semicolon, and all of them will execute sequentially.

You can see an example here:
![C++](https://anishg24.github.io/cse15l-lab-reports/assets/sc6.png)

[1]: https://sdacs.ucsd.edu/~icc/index.php
