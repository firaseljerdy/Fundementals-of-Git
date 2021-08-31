# Git Fundamentals

## Introduction

This is a documentation for all things related to git. From beginner to advanced, trying to understand it formally. This is as a personal reference, however feel free to use it as well!

## 1. How git works

### How it works on an internal level

Pretty sure git users are aware of commads used normally in day to day versioning control activities. These are know as **porcelain git commands**, they are the following.

 - `git add`
 - `git commit`
 
And if you are working on a remote repo

 - `git branch`
 - `git push` and `git pull`
 - `git switch <branch name>` or if you are on an older git version `git checkout`
 - `git merge` and `git rebase`

These are the foundational elements of essential git commands. 

However, we might use more raw low-level git commands such called **plumbing commands** and they are less frequently used. These can be the following.

 - `git cat-file`
 - `git hash-object`
 - `git count-objects`

> If you want to master git, don't worry about learning the commands, instead learn to understand their models.

Once you understand their models and how they are shaped, any git command becomes extremely easier to learn and implement. 

## 2. Wrapping Our Heads Around Git

Git is a distributed revision control system. At first, that may sound weird and jargon-heavy. But I assure you this is just a glorifed content manager. I'll show you why that is. 

Let us imagine that git is not distributed for a second. Assume we only have one computer in the world and a repository is on it. It becomes a revision control system. It loses its distributed property, retaining only it's revisioning aspects of it. Which are `branches`, `history` and `merge`. 

These are still complex features that git possesses. So to make it simple, assume we don't have them as well. Git changes from a revision control system to a mere simple content tracker. Meaning, all it does is track files and directories. 

### But how does it track them?
S
Going back to its essence and its core, let us forget about versioning as well. So no commits. This is in its most raw form. 

What git is in that state is nothing but a persistent map. Persistent is when we choose to allow it to write on our system. We have the option not to. 

So essentially, it is nothing but a map!
A basic structure that maps keys to their correponding values. 

## Values and Hashes

It is a table that contains keys and values. The values are a sequence of bytes that represent the contents of a file. Any sequence of given bytes will generate a unique hashed key. To hash bytes, git uses the famous `SHA1` hash function. 

The `SHA1` value of the input string *git* is 46f1a0bd5592a2f9244ca321b129902a06b53e03. 

There is only one hash for this value, the odds for getting the same key for another  value is highly inprobable.  So this is the key that git uses to store the content in the map. 

## Git is Made To Store Things

 We can calculate the hash of a string using git by using the `git hash-object`.

```git
$ git hash-object firas 
```
```
fatal: Cannot open 'Desktop/git-test/firas': No such file or directory
```

The problem with this is that we don't have a repository to write in. Since git only works with files and directories. 

Instead we can use the following command ```echo``` to log it. 

```git
$ echo "firas"
$ echo "firas" | git hash-object --stdin
```
This will output the hash.

```git
41743e6ccfc83ffc8743ab676db91f3e6913dbd6
```
Initially, git was creating as version control system and is used to store things. We can store the following output by writing it. 

```git
$ echo "firas" | git hash-object --stdin - w
```

This will give the following error. 

```git
fatal: Cannot open 'Desktop/git-test/-': No such file or directory
```
That is because we have not yet setup a git repository in the file directory. It can be easily done by 

```git
$ git init
$ git branch -m master
```

They will set up the local git repository in the file directory and create a branch called ```master```. Once a local repo is created, a hidden file gets initialized. To see the it we run ` ls -a`. 

![image](https://drive.google.com/uc?export=view&id=16yh17J55XU8I_gSqj3bBM1UCwhJEbGSD)

Opening the .git file with `open .git` shows us the folder in which git is initialised in. 

![image](https://drive.google.com/uc?export=view&id=1Rq0nAxMw8jV4O3Qk3mv7P7JWNfim2yZl)

What we really care about is the objects folder. Heading inside of it we can see that we have 2 folders; info and pack, they are not really that important for now. Let us now go back to the terminal and run some few commads to save into the repository. 

Since now we have a local repository, we can save stuff in. NEAT!

![image](https://drive.google.com/uc?export=view&id=11-daP0tmV4ZcCDGFI23Bi3QNkVRp4VIT)

 As you can see, we don't have a fatal error message any longer. If we go back to the objects folder we were in. 

![image](https://drive.google.com/uc?export=view&id=1rGIx9JWlxeggHJhdeRZh4dUkRqWwIUQM)

A new folder has been added named '41'.  And inside of it contains a file 

![image](https://drive.google.com/uc?export=view&id=1eZ_36Vh1TE4bp-3T3YlpsBHVD34pg6co)

This is the same as the hash value that we got when we echoed and wrote 'firas' string. The folder name is the 2 numbers at the beginning of hash value, and the rest is the name of the file in the folder. This is done by git for organisational purposes. If we do open the file, we won't get anything readable. instead we can run the following command `git cat-file <hash value> -t`

![image](https://drive.google.com/uc?export=view&id=11HzhH9aOYViK4NGiqWSuDcb4_vQDw4V4)

 
The -t stands for type. As in, get the type of the stored object, it returns blob. This is what git calls objects that are hashed and stored, because it is unreadable. 

If we run in again but with -p at the end. `git cat-file <hash value> -p`

![image](https://drive.google.com/uc?export=view&id=1GVwjd6QxwBP5V0Opp9eNjZxe1kwaTW0n)

It will print out the contents of the blob file in a pretty and readable way. 
