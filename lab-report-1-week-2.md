# CSE 15L Lab 2

## Purpose
To teach incoming and existing UCSD CSE 15L students using **macOS** machines
how to log into their `ieng6` account.

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
3. You will see a yellow warning pop up saying to change your password. Click on the link in that warning, and enter your **account** username and PID, and change your password
    - **Ensure you uncheck "Change MyTritonLink Password?"** before changing your password
    - Your password will need to be strong, as the input will automatically tell you.
    - Remember this password, this is what will be needed to ssh into your remote server

### Accessing Your Remote Server
1. Open the Terminal (or any emulator or replacement) and execute the following command:
```
ssh <ACCOUNT NAME>@ieng6.ucsd.edu
```
2. When prompted for a password, enter the password you set. Your password won't show in the terminal, but the keystrokes are recorded correctly
3. When prompted if you want to continue connecting, type `yes`. 




[1]: https://sdacs.ucsd.edu/~icc/index.php
