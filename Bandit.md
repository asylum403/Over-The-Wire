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

* Some folks might want a cleaner output, but not all the goodies on the same line. Here's something for those who just want to list out the contents with each line having it's own owner, try ```ls -1a``` 

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

# Bandit Level 1 - find Level 2 Key
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
## The key for the next level is stored in a hidden file in the inhere directory.

Once in bandit3, let's grab that list. We'll run a ```ls -1a``` and we see a directory named **inhere**.

When we ```ls -1a```, we see ...Hiding-From-You, which doesn't give any issues when you simply cat ...Hiding-From-You. 

The key to the next level has been obtained. It's time to continue our quest!

# Bandit Level 4
## The key is in a hidden file in the 'inhere' directory. 

Time to rock out with the first step, list everything in the directory once we change directory (cd) to the inhere directory. 

If we just run the 'ls' in the inhere directory, we get nothing back. Let's add some options, run ls -1ar and you'll see a file, ...Hiding-From-You. 

We've been runnign the same old stuff so far, so let's change it up. The hint says that we may need commands like file, du, or find, so let's use each one to find our hidden file. 

Let's run ``` file .* ``` from in the 'inhere' directory and it will bring you the hidden file we're looking for and it also lets you know that it's a ASCII text file, which is good info to have. This means we should be able to cat and read it straight up. You'll notice the .*, the . says to expand to all the dot-files (. and .. are both directories).

Maybe we want to find a file that has ASCII text, the we run ``` file .* | grep "ASCII text" ```, that should output all ASCII text files in that directory. 

Let's work with the power searcher, find. You can as for stuff like name, type, size, and more, so let's get cracking!

We'll run ``` find . -type f -name ".*" ``` and if we run in it without changing directory, it will still do the work we need. That first '.' says we'll be starting our search from the current directory and the -type f says to limit this search to regular files, not directories. Then we want to say search for any hidden files, so that would start with '.' because '.' denotes a hidden file (which was mentioned in the directions).

If you ran that from the directory you start in, your output will have ./inhere/...Hiding-From-You, which is our hidden file. 

Like Mr. Timberlake says, we're bringing sexy back, so let's get spread this out to be even cooler. ``` find . -type f -name ".*" -exec cat {} \; ``` which will run cat on any match and the curly brackets are the placeholder for the file name that makes the match. When you run this from the home directory, you'll get a larger output than if you cd into the inhere directory, then run it.

Now let's check out the du, disk useage, command. Let's start off with ``` du -a ``` and if we run this from that home directory, you'll get an output like this:

4       ./inhere/...Hiding-From-You
8       ./inhere
4       ./.profile
4       ./.bashrc
4       ./.bash_logout
24      .

The numbers refer to the size in KB or blocks, just depends on your system. That ...Hiding file looks promising doesn't it. 

Let's add some seasoning to that bland bit of work, ``` du -a | sort -n ```, this will sort the files numerically by size and if any hidden files have something to them, they'll appear here too. 

Any of these methods should help you find the key! Let's keep moving shall we!

# Bandit Level 5
## The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

Another set of instructions telling us to find the file in the 'inhere' directory again. It's gotta be human readable, so ASCII text for sure. 

Once we get in the directory 'inhere', we see quite a number of files. They all start with a dash, so 

If you recall on that last one, we used file. File did a great job of telling us what the file type was, so let's run ``` file ./-* ```, this will use file to inspect each file's magic bytes to guess what kind of file it is (ASCII would be human readable, but data would mean binary). That last part, ./-*, will match all the files starting with the - (the ./ prefix avoids shell interpretting the file name as an option flag instead of a possible text file, which would just complain at you that you didn't fulfill the paramaters and sell your soul). 

bandit4@bandit:~/inhere$ file ./-*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data

Well, looks like numero 7 is our lucky number! 

There's an even quicker way, ``` file -- -* ```, where the -- is a wildcard that says no more operations, then the -* to check all the files starting with the -. You'll get the same output.

Now, sometimes we want to be fancy, so if you want to get fancy, stick that pinky out and pipe that request to grep so you can auto-filter it. 
 
``` file -- -* | grep "ASCII text" ```

We want to use file here, we're looking for a specific type of file and if we had more directories and wanted to scale this up, we'd use something like ``` for f in ./-*; do file "$f"; done | grep "ASCII text" ```

Let's go a step further, let's output what we need. 
``` file ./-* | grep "ASCII text" | awk -F: '{print $1}' | xargs cat ```

That's a big step isn't it, let's break it down. We know the file starts with a dash, so ``` file ./-* ``` looks for all files starting with a dash. The grep is looking for ASCII text, which will be where our key is hiding. The ``` awk ``` splits the line on the colon (so './-file07: ASCII text' becomes './-file07') then passes that filename to cat in ``` xargs cat ```. 

# Bandit Level 5
## The key is stored in a file swomewhere under the inhere directory and has all of the following properties:
1. Human Readable
2. 1033 bytes in size
3. Not executable

We cd into 'inhere', as the clue says the key is hiding there. A quick ``` ls -1ar ``` and we get see a long list of directories. 

Let's keep things interesting, we have already went into a direcory called 'inhere', then we take a look to see a decent sized list of directories, but no files. Let's peak at one without leaving 'inhere'. ``` ls maybehere11 -1ar ``` you can use whatever number you want, but you'll get an output of that directories contents and not have to change directories multiple times. 

If you want to get deeper, you can absolutely walk through directories using a lot of different commands, I won't say all because I'm only going off of my studying. For the above, we're asking for a list of all things in that directory, once you tab the name out, you'll get the slash '/' and you can hit Tab twice to get an output of other directories. Then you can enter your choice, rinse and repeat. I came across this by using tab to autocomplete stuff in various places a lot, so when I saw this, I was so happy! Such a cool capability. Now back to your regularly scheduled command line lesson.

That's a lot of directories to go through, let's put something together that specicially looks for the 3 requirements. 
``` find inhere -type f -size 1033c ! -executable -exec file {} + | grep "ASCII text" | awk -F: '{print $1}' | xargs cat ```

As usual, time to get dirty! At the start, we say we want to look for this specific thing and we want to start at the directory 'inhere', but if you remove that, it would execute in whatever directory you run this in. Let's also be sure we're grabbing a file, not a directorye, so set -type to f. 

How the heck do we check for 1033 bytes? Each ASCII character is a byte, so if we said 1033 ASCII characters, it would also equal 1033 bytes. We don't want any executable files, so ! in front of -executable nulls out the executable perms (not runnable by anyone). -exec file {} + runs file on matches in batches, which can be efficient in big directories. 

The last parts should be familiar, we look for human readable with the ASCII text, grab the filename, then send it to cat with xargs. If you did this right, you should have the next key!! 


# Bandit Level 6
## The key is stored somewhere on the server and has all of the following properties:
1. Owned by user bandit7
2. Owned by group bandit6
3. 33 bytes in size




# Bandit Level #
## How to find key
