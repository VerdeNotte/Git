
# Version Control
A **version control system** is a tool that manages changes made to the files and directories in a project.

## Where does Git store information?

Each of your Git projects has two parts: the files and directories that you create and edit directly, and the extra information that Git records about the project's history. The combination of these two things is called a  **repository**.

Git stores all of its extra information in a directory called  `.git`  located in the root directory of the repository. Git expects this information to be laid out in a very precise way, so you should never edit or delete anything in  `.git`.

`git status` checks the status of your repository

Git has a  **staging area**  in which it stores files with changes you want to save that haven't been saved yet. Putting files in the staging area is like putting things in a box, while  **committing**  those changes is like putting that box in the mail: you can add more things to the box or take things out as often as you want, but once you put it in the mail, you can't make further changes.

`git status` shows you which files are in this staging area, and which files have changes that haven't yet been put there. In order to compare the file as it currently is to what you last saved, you can use `git diff filename`. `git diff` without any filenames will show you all the changes in your repository, while `git diff directory` will show you the changes to the files in some directory.

You commit changes to a Git repository in two steps:

1.  Add one or more files to the staging area.
2.  Commit everything in the staging area.

To add a file to the staging area, use  `git add filename`.

To compare the state of your files with those in the staging area, you can use `git diff -r HEAD`. The `-r` flag means "compare to a particular revision", and `HEAD` is a shortcut meaning "the most recent commit".
### Committing Changes 
To save the changes in the staging area, you use the command  `git commit`. It always saves everything that is in the staging area as one unit: as you will see later, when you want to undo changes to a project, you undo all of a commit or none of it.

When you commit changes, Git requires you to enter a  **log message**. This serves the same purpose as a comment in a program: it tells the next person to examine the repository why you made a change.

By default, Git launches a text editor to let you write this message. To keep things simple, you can use  `-m "some message in quotes"` 

If you accidentally mistype a commit message, you can change it using the `--amend` flag.

### View Repository

The command `git log` is used to view the **log** of the project's history.
The `commit` line displays a unique ID for the commit called a **hash**; we will explore these further in the next chapter. The other lines tell you who made the change, when, and what log message they wrote for the change.

A project's entire log can be overwhelming, so it's often useful to inspect only the changes to particular files or directories. You can do this using `git log path`, where `path` is the path to a specific file or directory. The log for a file shows changes made to that file; the log for a directory shows when files were added or deleted in that directory, rather than when the contents of the directory's files were changed.

To view a specific file's history

`git log path` where `path` is the path to a specific file or directory. The log for a file shows changes made to that file; the log for a directory shows when files were added or deleted in that directory, rather than when the contents of the directory's files were changed.

### How to write a better log message?
using `git commit -m "message"` is good for a one-line log message. Useful for small changes but collaborators (including yourself) will appreciate more info.  Running `git commit` without `-m "message"` will launch a text editor. 

# How does Git store information?

You may wonder what information is stored by each commit that you make. Git uses a three-level structure for this.

1.  A  **commit**  contains metadata such as the author, the commit message, and the time the commit happened. In the diagram below, the most recent commit is at the bottom (`feed0098`), underneath its parent commits.
2.  Each commit also has a  **tree**, which tracks the names and locations in the repository when that commit happened. In the oldest (top) commit, there were two files tracked by the repository.
3.  For each of the files listed in the tree, there is a  **blob**. This contains a compressed snapshot of the contents of the file when the commit happened (blob is short for  _binary large object_, which is a SQL database term for "may contain data of any kind"). In the middle commit,  `report.md`  and  `draft.md`  were changed, so the blobs are shown next to that commit.  `data/northern.csv`  didn't change in that commit, so the tree links to the blob from the previous commit. Reusing blobs between commits help make common operations fast and minimizes storage space.
# What is a hash?

Every commit to a repository has a unique identifier called a  **hash**  (since it is generated by running the changes through a pseudo-random number generator called a  **hash function**). This hash is normally written as a 40-character hexadecimal string like  `7c35a3ce607a14953f070f0f83b5d74c2296ef93`, but most of the time, you only have to give Git the first 6 or 8 characters in order to identify the commit you mean.

Hashes are what enable Git to share data efficiently between repositories. If two files are the same, their hashes are guaranteed to be the same. Similarly, if two commits contain the same files and have the same ancestors, their hashes will be the same as well. Git can therefore tell what information needs to be saved where by comparing hashes rather than comparing entire files.

### Viewing a specific commit
To view the details of a specific commit, you use the command `git show` with the first few characters of the commit's hash.

# What is Git's equivalent of a relative path?

A hash is like an absolute path: it identifies a specific commit. Another way to identify a commit is to use the equivalent of a relative path. The special label  `HEAD`, which we saw in the previous chapter, always refers to the most recent commit. The label  `HEAD~1`  then refers to the commit before it, while  `HEAD~2`  refers to the commit before that, and so on.

Note that the symbol between  `HEAD`  and the number is a tilde  `~`,  _not_  a minus sign  `-`, and that there cannot be spaces before or after the tilde.

# How can I see who changed what in a file?

`git log`  displays the overall history of a project or file, but Git can give even more information. The command  `git annotate file`  shows who made the last change to each line of a file and when.

Each line contains five elements, with elements two to four enclosed in parentheses. When inspecting the first line, we see:

1.  The first eight digits of the hash,  `04307054`.
2.  The author,  `Rep Loop`.
3.  The time of the commit,  `2017-09-20 13:42:26 +0000`.
4.  The line number,  `1`.
5.  The contents of the line,  `# Seasonal Dental Surgeries (2017) 2017-18`.

# How can I see what changed between two commits?

`git show`  with a commit ID shows the changes made  _in_  a particular commit. To see the changes  _between_  two commits, you can use  `git diff ID1..ID2`, where  `ID1`  and  `ID2`  identify the two commits you're interested in, and the connector  `..`  is a pair of dots. For example,  `git diff abc123..def456`  shows the differences between the commits  `abc123`  and  `def456`, while  `git diff HEAD~1..HEAD~3`  shows the differences between the state of the repository one commit in the past and its state three commits in the past.

1. Check Status
2. `git add` to add files to the staging area
3. `git commit -m "message` to save the staged files
### How do I tell Git to ignore certain files?

Data analysis often produces temporary or intermediate files that you don't want to save. You can tell it to stop paying attention to files you don't care about by creating a file in the root directory of your repository called  `.gitignore`  and storing a list of  **wildcard**  patterns that specify the files you don't want Git to pay attention to.

### How can I remove unwanted files?

Git can help you clean up files that you have told it you don't want. The command  `git clean -n`  will show you a list of files that are in the repository, but whose history Git is not currently tracking. A similar command  `git clean -f`  will then delete those files.

_Use this command carefully:_  `git clean`  only works on untracked files, so by definition, their history has not been saved. If you delete them with  `git clean -f`, they're gone for good.

### How can I see how Git is configured?

Like most complex pieces of software, Git allows you to change its default settings. To see what the settings are, you can use the command  `git config --list`  with one of three additional options:

-   `--system`: settings for every user on this computer.
-   `--global`: settings for every one of your projects.
-   `--local`: settings for one specific project.

Each level overrides the one above it, so  **local settings**  (per-project) take precedence over  **global settings**  (per-user), which in turn take precedence over  **system settings**  (for all users on the computer).

Config settings are useful for storing your name and email address (to identify you in commit logs), choosing your favorite text editor and diff view tools, and customizing things just how you like them.

# How can I change my Git configuration?

Most of Git's settings should be left as they are. However, there are two you should set on every computer you use: your name and your email address. These are recorded in the log every time you commit a change, and are often used to identify the authors of a project's content in order to give credit (or assign blame, depending on the circumstances).

To change a configuration value for all of your projects on a particular computer, run the command:

```
git config --global setting value

```

Using this command, you specify the  `setting`  you want to change and the  `value`  you want to set. The settings that identify your name and email address are  `user.name`  and  `user.email`, respectively.

### How can I commit changes selectively?

You don't have to put all of the changes you have made recently into the staging area at once. For example, suppose you are adding a feature to  `analysis.R`  and spot a bug in  `cleanup.R`. After you have fixed it, you want to save your work. Since the changes to  `cleanup.R`  aren't directly related to the work you're doing in  `analysis.R`, you should save your work in two separate commits.

The syntax for staging a single file is  `git add path/to/file`.

If you make a mistake and accidentally stage a file you shouldn't have, you can unstage the additions with  `git reset HEAD`  and try again.

### How can I undo changes to unstaged files?
This is about undoing changes that weren't staged yet.
Suppose you have made changes to a file, then decide you want to  **undo**  them. Your text editor may be able to do this, but a more reliable way is to let Git do the work. The command:

```
git checkout -- filename

```

will discard the changes that have not yet been staged. (The double dash  `--`  must be there to separate the  `git checkout`  command from the names of the file or files you want to recover.)

_Use this command carefully:_  once you discard changes in this way, they are gone forever.

### How can I undo changes to staged files?

At the start of this chapter you saw that  `git reset`  will unstage files that you previously staged using  `git add`. By combining  `git reset`  with  `git checkout`, you can undo changes to a file that you staged changes to. The syntax is as follows.

```
git reset HEAD path/to/file
git checkout -- path/to/file

```

(You may be wondering why there are two commands for re-setting changes. The answer is that unstaging a file and undoing changes are both special cases of more powerful Git operations that you have not yet seen.)

# How do I restore an old version of a file?

You previously saw how to use  `git checkout`  to undo the changes that you made since the last commit. This command can also be used to go back even further into a file's history and restore versions of that file from a commit. In this way, you can think of committing as saving your work, and  **checking out**  as loading that saved version.

The syntax for restoring an old version takes two arguments: the hash that identifies the version you want to restore, and the name of the file.

For example, if  `git log`  shows this:

```
commit ab8883e8a6bfa873d44616a0f356125dbaccd9ea
Author: Author: Rep Loop <repl@datacamp.com>
Date:   Thu Oct 19 09:37:48 2017 -0400

    Adding graph to show latest quarterly results.

commit 2242bd761bbeafb9fc82e33aa5dad966adfe5409
Author: Author: Rep Loop <repl@datacamp.com>
Date:   Thu Oct 16 09:17:37 2017 -0400

    Modifying the bibliography format.

```

then  `git checkout 2242bd report.txt`  would replace the current version of  `report.txt`  with the version that was committed on October 16. Notice that this is the same syntax that you used to undo the unstaged changes, except  `--`  has been replaced by a hash.

Restoring a file doesn't erase any of the repository's history. Instead, the act of restoring the file is saved as another commit, because you might later want to undo your undoing.

One more thing: there's another feature of  `git log`  that will come in handy here. Passing  `-`  followed by a number restricts the output to that many commits. For example,  `git log -3 report.txt`  shows you the last three commits involving  `report.txt`.

### How can I undo all of the changes I have made?

So far, you have seen how to undo changes to a single file at a time using  `git reset HEAD path/to/file`. You will sometimes want to undo changes to many files.

One way to do this is to give  `git reset`  a directory. For example,  `git reset HEAD data`  will unstage any files from the  `data`  directory. Even better, if you don't provide any files or directories, it will unstage everything. Even even better,  `HEAD`  is the default commit to unstage, so you can simply write  `git reset`  to unstage everything.

Similarly  `git checkout -- data`  will then restore the files in the  `data`  directory to their previous state. You can't leave the file argument completely blank, but recall from  [Introduction to Shell for Data Science](https://www.datacamp.com/courses/introduction-to-shell-for-data-science)  that you can refer to the current directory as  `.`. So  `git checkout -- .`  will revert all files in the current directory.
> Written with [StackEdit](https://stackedit.io/).
