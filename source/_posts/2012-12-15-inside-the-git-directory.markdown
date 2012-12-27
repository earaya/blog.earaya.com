---
layout: post
title: "Inside the .git Directory"
date: 2012-12-15 16:33
comments: true
categories: [vcs, git, dvcs, source control]
---

Have you ever wondered how git works? Have you tried to figure out how git stores stuff?

Well, I have and this is what I discovered about the git object store.

## In the begining there was nothing.

Not really. In fact, there's quite a bit of stuff in an empty repo. On your command line, create a new repo and list its contents.

```bash
# Create the repository
$ mkdir mygitrepo && cd mygitrepo
$ git init
Initialized empty Git repository in /Users/earaya/Projects/gitrepo/.git/

# Show all files
$ find .
.
./.git
./.git/config
./.git/description
./.git/HEAD
./.git/hooks
./.git/hooks/applypatch-msg.sample
./.git/hooks/commit-msg.sample
./.git/hooks/post-update.sample
./.git/hooks/pre-applypatch.sample
./.git/hooks/pre-commit.sample
./.git/hooks/pre-rebase.sample
./.git/hooks/prepare-commit-msg.sample
./.git/hooks/update.sample
./.git/info
./.git/info/exclude
./.git/objects
./.git/objects/info
./.git/objects/pack
./.git/refs
./.git/refs/heads
./.git/refs/tags
```

As you can see, there's quite a bit of stuff there to begin with.

Something we learn right off the bat is that Git supports [hooks](http://git-scm.com/book/en/Customizing-Git-Git-Hooks). Take a look at some of the samples. At work, we use a pre-commit hook to run our linting before letting you commit. But I digress, I want to talk about the object store.

You'll see that initially, the `objects` directory is empty.

So, let's create a [git object](http://git-scm.com/book/en/Git-Internals-Git-Objects). In order for this to work, you have to type the commands as shown:

```bash
$ echo "git rocks" > test.txt
$ git add test.txt
```

As a result of adding the above object you should now see:

```bash
$ find .git/objects
.git/objects
.git/objects/f6
.git/objects/f6/8ce0a31a54e37649ee417d60e90911258f1043
.git/objects/info
.git/objects/pack
```

## And then there was a (SHA1) hash

You might now be wondering how I was so confident you'd get the same output I got on my machine.

The answer is simple: Git is, at its core, just a key-value store. Git doesn't care what you call your objects; Git only cares about the content of those objects.

To store an object, Git first peforms a few operations on the data. One of these operations is to calculate the SHA1 hash of the data and then store the data in the object store with a filename representing the hash.

At this point we have to make a brief but important aside. SHA1 has two properties that make it really useful for Git's use:

1. It's extremely unlikely that 2 different objects will have the same hash value.
2. Identical objects will always the same hash representation.


And so, `8ce0a31a54e37649ee417d60e90911258f1043` represents the SHA1 hash of "git rocks". That's how I knew you'd get exactly the same output I got.

## And finally, there was a tree.

So now that our file is tucked away in the object store, we have to wonder: What happened to its filename? After all, Git wouldn't be that useful if it didn't preserve folder structure and if it didn't let us find our files by name.

Git tracks pathnames through a [tree](http://git-scm.com/book/en/Git-Internals-Git-Objects#Tree-Objects).

Go back to your command line and type:

```bash
$ git write-tree
81cbaf28bc31ce9218d51b685e35a08bfea99599
$ find .git/objects
.git/objects
.git/objects/81
.git/objects/81/cbaf28bc31ce9218d51b685e35a08bfea99599
.git/objects/f6
.git/objects/f6/8ce0a31a54e37649ee417d60e90911258f1043
.git/objects/info
.git/objects/pack
```

`git write-tree` (a low level command) saves the state of the index (your staged files) to the object store. Thus, we now see a new object in the store: `.git/objects/81/cbaf28bc31ce9218d51b685e35a08bfea99599`. This new object is our tree.

Once you peek into the tree object, it'll immeidately make sense. You'll be able to see the file contents by typing the following git command:

```bash
$ git cat-file -p 81cbaf
100644 blob f68ce0a31a54e37649ee417d60e90911258f1043    test.txt
```

You might already have guessed it, but the first section of the file, the `100644` represents the file permissions (in octal). `f68ce0` represents the filename of the blob in the store, and `test.txt` is the filename.

Directory hierarchies are represented in a similar manner, of course.

## Conclusion

And there you have it folks. That's pretty much all there is to how Git stores objects. Pretty simple, huh?

I think it's amazing how Linus built such a powerful and useful system by elegantly using a simple hashmap. I wish my software was more like Git.

I hope you too gain an appreciation for using simple constructs in your own code.