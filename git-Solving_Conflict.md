# GIT - Sovling Conflict
Solving conflict through GIT.

## Table of contents:  
* [Linux Command](#linux-command)  
* [Solving Conflict](#solving-conflict) 

<br>

### Linux Command
First, we need to introduce some Linux command, **echo**, **touch**, **rm**, and **cat**. Then, some of the linux symbol will be mentioned.

The `touch` command is to create a file through the Terminal. The `echo` command is like the `print` command in Python. The `rm` command is to delete a file through the Terminal. And, finally, the `cat` command is one of the most widely used commands in Linux. The name of the cat command comes from its functionality to concatenate files. It can read and concatenate files, writing their contents to the standard output.

The shell redirection symbol, `>`, is to "redirect" this an output to a file. The `>>` symbol is to "append" a source text file to another. Where this kind of input/output redirection really shines is in the use of ***pipes***. The `|` operator is to “chain” programs such that the output of one is the input of another.

After knowing some of the basic command, it is time to create a file.

### Solving Conflict
Set up a repository:
```
    $ mkdir gittest
    $ cd gittest
    $ git init
```

Create a file:

1. Use `touch` and `echo` commands:

    ```
    $ touch myfile.txt | echo 'Hello !' > myfile.txt
    $ echo 'This is the second line.' >> myfile.txt
    ```

2. Use `cat` command, typing a text content and use `ctrl` + `D` to terminate input:

    ```
    $ cat > myfile.txt
    Hello !
    This is the second line.
    ```

Then, commit the new file.
```
    $ git add .
    $ git commit -a -m 'Initial commit'
    [master (root-commit) 5da7e00] Initial commit
     1 file changed, 1 insertion(+)
     create mode 100644 myfile.txt
```

Git is currently at the "master" branch. We are going to make some changes on that file on the "development" branch. (Specifying `-b` causes a new branch to be created as if git-branch were called and then checked out.):
```
    $ git checkout -b development
    Switched to a new branch 'development'
   
    $ cat > myfile.txt
    This is the first line.
    This is the second line.

    $ git commit -a -m 'Replace hello with first line'
    [development 3036384] Replace hello with first line
 	1 file changed, 1 insertion(+), 1 deletion(-)
```

Now we are going to make an conflict change to the same file on the "master" branch:
```
    $ git checkout master
    Switched to branch 'master'

    $ cat >> myfile.txt
    Hello ! This is the first line.
	This is the second line.

    $ git commit -a -m 'Added first line in master'
	[master ab8ff18] Added first line in master
 	1 file changed, 1 insertion(+), 1 deletion(-)

```

Git cannot solve the conflict since both branches have made changes on the first line. Theoretically, we could merge in either direction. But to ensure it has minimal problems, we should merge from "master" onto "development". Once the conflict is resolved, we can merge back the other way (with a fast forward).

Git scans text line by line. Therefore, if there is a difference between lines, then, it is considered as a conflict.

If we merge, Git will tell us there is a confict:
```
    $ git checkout development
    Switched to branch 'development'

    $ git merge master
    Auto-merging myfile.txt
    CONFLICT (content): Merge conflict in myfile.txt
	Automatic merge failed; fix conflicts and then commit the result.
```

The `git status` command tells what to do next if you have encountered a conflict:

```
    $ git status
    On branch development
    You have unmerged paths.
      (fix conflicts and run "git commit")
      (use "git merge --abort" to abort the merge)

    Unmerged paths:
      (use "git add <file>..." to mark resolution)

        both modified:   myfile.txt

    no changes added to commit (use "git add" and/or "git commit -a")
```

Use `cat` command to display the file, "myfile.txt":
```
    $ cat myfile.txt
	<<<<<<< HEAD
	This is the first line.
	=======
	Hello ! This is the first line.
	>>>>>>> master
	This is the second line.
```

The content shows that which line has conflic and it uses `=======` symbolto seperate the conflict in the same line between two branches. `<<<<<<<` symbol represents the content in the current branch.  `>>>>>>>` indicates the content in the master branch.

At this stage, DO NOT do `git add` and `git commit` until you have resolved the conflict.

Here, we can use any editor to fix the conflict. For the sake of learing command-line, we are going to use vim editor to solve the conflict by keeping the changes on master branch. First, type `i` to enter to insert mode. After finishing the edit, use `esc` to escape the insert mode and use `:wq` to save and quit.

```
	$ vim myfile.txt
	Hello ! This is the first line.
	This is the second line.

	$ git add myfile.txt

	$ git status
	On branch development
	All conflicts fixed but you are still merging.
	  (use "git commit" to conclude merge)

	Changes to be committed:
		modified:   myfile.txt

```

We have just resolved all the conflict and ready to commit the merge.

```
	$ git commit -a -m 'Done merging'
	[development 36100f5] Done merging
```

We can go back to "master" branch and merge "development" branch to **fast-forward** the history:
```
    $ git checkout master
    Switched to branch 'master'

    $ git branch
      development
	* master

    $ git merge development
    Updating ab8ff18..36100f5
	Fast-forward

	$ git status
	On branch master
	nothing to commit, working tree clean

	$ cat myfile.txt 
	Hello ! This is the first line.
	This is the second line.

	$ git branch -d development
	Deleted branch development (was 36100f5).
```
<br>

### Reference:  
* [Unix and Linux commands help](https://www.computerhope.com/unix.htm)  
* [Unix Command](http://boson4.phys.tku.edu.tw/UNIX/Unix%20Command/index_basic.htm)

