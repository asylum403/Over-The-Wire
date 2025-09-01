# Bandit Level 0
## The answer is in the description and once in, your terminal should show the bandit drawing and ask for your password.
If your password is correct, you'll see the OTW displayed and a Welcome to OverTheWire! with more information. At the bottom
you'll notice it say Enjoy your stay! then show you logged in as bandit0@bandit:~$

If you saw that, CONGRATS! 

Now to find the password for Level1. 

## Find password to Level 1
We're in a new place, let's see what's here. We decided to run:
```
ls -a
```
```ls``` = list directory contents (will be use a lot).

```ls -a``` = do not ignore entries starting with a period, dot, decimal, whatever you want to call this -> .

We could also do:
``` ls -lar ```  = 'l' is for the long format, so instead of just names, we can also see permissions, owners, etc. 'a' gets all the files, 
even the hidden ones. 'r' is just for fun and sets the list in reverse order, putting the '.', &'..' a the bottom of the list instead 
of on top.

The files we see are:
1. readme
2. .profile
3. .bashrc
4. .bash_logout
5. ..
6. .

Might as well check out that readme file, so let's use:

```cat readme```
You should find the key to the next level here!

# Bandit Level 1
## The key is said to be stored in a file called - and would be located in the home directory. That's right, a dash is the name.
Let's start off the same way, with ```ls -lar``` and see what we can get. 

Because the filename is a character, which is a placeholder for standard input (stdin), so in order to escape this and get cat to 
output the contents of the file, we need to tell cat to treat '-' as a literal filename. We can do this by giving it a path, so ~ 
expands to the home directory, /home/YOU/-, is what we're saying when we pass the ~/- to cat and finally find our key to the next level!

# Bandit Level 2
## The key to the next level is in a file called --spaces in this filename-- that lives in the home directory.

This is just like the last one almost, there's symbols and spaces, so we used the last method where we ran ```cat ~/-``` and since ~ isn't a key I use a lot, I wanted to find another way. 

Another method to solve for cat to read the file is replacing the ~ with a . and it would look like this ```cat ./--``` and I simply Tab out to the end of the file name, if you hit Tab once and nothing happens, try using Tab twice in a quick succession and you should see an output of the files that might have naming convention clashes. It's like you cat for a file that has an almost matching name, where the first 8 characters are exactly the same and you've only entered 7 or less characters, then you would need to hit the Tab twice, but if the characters you've entered don't have a match in that directory, a single Tab will autocomplete typing out the name. 

# Bandit Level 3
