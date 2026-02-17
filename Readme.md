### Setting Up Git

#### Installing Git

instructions on website - already installed

#### Configure Git with your username and email address

```bash
git config --global user.name "erezzoug"
git config --global user.email "rezougelmehdi0@gmail.com"
```

### Git commits to commit

#### establish a subdirectory named hello and generating a file hello.sh with the following content

```bash
mkdir hello
cd hello/
echo "Hello, World" > hello.sh
```

#### Initialize the git repository in the hello directory

```bash
git init
```

#### Check the status and act accordingly with the output of the executed command

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.sh

nothing added to commit but untracked files present (use "git add" to track)
```

```bash
git add hello.sh
```

#### Change the hello.sh content, stage and commit

```
#!/bin/bash

echo "Hello, $1"
```

```bash
git add hello.sh
git commit -m "changing hello.sh content 1"
git status
On branch master
nothing to commit, working tree clean
```

#### Modify the hello.sh file to include comments and stage it

```
#!/bin/bash

# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

```bash
git add -p hello.sh
```

The first commit should be for the comment in line 3

```bash
git commit -m "changing hello.sh content 2"
```

The second commit should include changes made to lines 4 and 5.

```bash
git add -p hello.sh
git commit -m "changing hello.sh content 2 --adding lines 4 and 5"
```

### History

#### Show the history of the working directory

```bash
git log
commit d15a0da272cb449850562bc17313c651e9608cf7 (HEAD -> master)
Author: erezzoug <rezougelmehdi0@gmail.com>
Date:   Fri Feb 6 18:21:38 2026 +0100

    changing hello.sh content 2 --adding lines 4 and 5

commit cd6cb8d210a9004392401aa7914d2904ad04e520
Author: erezzoug <rezougelmehdi0@gmail.com>
Date:   Fri Feb 6 18:03:37 2026 +0100

    changing hello.sh content 2

commit fc283f0511c0eb175ca8e21388f34d435405ef2d
Author: erezzoug <rezougelmehdi0@gmail.com>
Date:   Fri Feb 6 17:49:30 2026 +0100

```

#### Show One-Line History

```bash
git log --oneline
d15a0da (HEAD -> master) changing hello.sh content 2 --adding lines 4 and 5
cd6cb8d changing hello.sh content 2
fc283f0 changing hello.sh content 1
```

#### Controlled Entries

the last 2 commits made within the last 5 minutes

```bash
git log -2 --since="5 minutes ago"
```

#### Personalized Format

commit hash, date, message, branch information, and author name, resembling

`* e4e3645 2023-06-10 | Added a comment (HEAD -> main) [John Doe]`

```bash
git log --pretty=format:"* %h %ad| %s %d [%an]" --date=short
* d15a0da 2026-02-06| changing hello.sh content 2 --adding lines 4 and 5  (HEAD -> master) [erezzoug]
* cd6cb8d 2026-02-06| changing hello.sh content 2  [erezzoug]
* fc283f0 2026-02-06| changing hello.sh content 1  [erezzoug]
```

### Check it out

#### Restore First Snapshot

```bash
git checkout fc283f0
cat hello.sh
```

```
#!/bin/bash

echo "Hello, $1"
```

#### Restore Second Recent Snapshot

```bash
git checkout master~1
cat hello.sh
```

```
#!/bin/bash

# Default is "World"
```

#### Return to Latest Version

```bash
git checkout master
cat hello.sh
```

```
#!/bin/bash

# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

```bash
git status
On branch master
nothing to commit, working tree clean
```

### TAG me

#### Referencing Current Version

Tag the current version of the repository as v1

```bash
git tag v1
```

##### Tagging Previous Version

```bash
git tag v1-beta HEAD^
```

#### Navigating Tagged Versions

Move back and forth between the two tagged versions, v1 and v1-beta

```bash
git checkout v1-beta
Previous HEAD position was d15a0da changing hello.sh content 2 --adding lines 4 and 5
HEAD is now at cd6cb8d changing hello.sh content 2
```

```bash
git checkout v1
Previous HEAD position was cd6cb8d changing hello.sh content 2
HEAD is now at d15a0da changing hello.sh content 2 --adding lines 4 and 5
```

```bash
git checkout v1-beta
Previous HEAD position was d15a0da changing hello.sh content 2 --adding lines 4 and 5
HEAD is now at cd6cb8d changing hello.sh content 2
```

```bash
git checkout v1
Previous HEAD position was cd6cb8d changing hello.sh content 2
HEAD is now at d15a0da changing hello.sh content 2 --adding lines 4 and 5
```

#### Listing Tags

Display a list of all tags present in the repository to verify successful tagging

```bash
git tag
v1
v1-beta
```

### Changed your mind?

#### Reverting Changes

Modify the latest version of the file with unwanted comments, then revert it back to its original state before staging

```
#!/bin/bash

# This is a bad comment. We want to revert it.
name=${1:-"World"}

echo "Hello, $name"
```

```bash
git restore hello.sh
cat hello.sh
```

```
#!/bin/bash

# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

#### Staging and Cleaning

Introduce unwanted changes to the file, stage them, then clean the staging area to discard the changes.

```
#!/bin/bash

# This is an unwanted but staged comment
name=${1:-"World"}

echo "Hello, $name"
```

```bash
git restore --staged hello.sh
git restore hello.sh
cat hello.sh
```

```
#!/bin/bash

# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

#### Committing and Reverting

Add the following unwanted changes again, stage the file, commit the changes, then revert them back to their original state.

```
#!/bin/bash

# This is an unwanted but committed change
name=${1:-"World"}

echo "Hello, $name"
```

```bash
git add hello.sh
git checkout HEAD^
git commit -m "unwanted but committed change"
git checkout HEAD^
cat hello.sh
```

```
#!/bin/bash

# Default is "World"
name=${1:-"World"}
echo "Hello, $name"
```

#### Tagging and Removing Commits

Tag the latest commit with oops, then remove commits made after the v1 version. Ensure that the HEAD points to v1.

```bash
git tag oops
git reset v1
```

#### Displaying Logs with Deleted Commits

```bash
git log --all --oneline
```

```
6274c5f (tag: oops) unwanted but committed change
d15a0da (HEAD -> master, tag: v1) changing hello.sh content 2 --adding lines 4 and 5
cd6cb8d (tag: v1-beta) changing hello.sh content 2
fc283f0 changing hello.sh content 1
```

#### Cleaning Unreferenced Commits

Ensure that unreferenced commits are deleted from the history, meaning there should be no logs for these deleted commits

```bash
git tag -d oops
git gc --prune=now
git log --all --oneline
```

```
d15a0da (HEAD -> master, tag: v1) changing hello.sh content 2 --adding lines 4 and 5
cd6cb8d (tag: v1-beta) changing hello.sh content 2
fc283f0 changing hello.sh content 1
```

#### Author Information

Add an author comment to the file and commit the changes.

```
#!/bin/bash

# Default is World
# Author: Jim Weirich
name=${1:-"World"}

echo "Hello, $name"
```

```bash
git add hello.sh
git commit -m "adding an author comment"
```

#### Oops the author email was forgotten

Oops the author email was forgotten, update the file to include the email without making a new commit, but include the change in the last commit.

```
#!/bin/bash

# Default is World
# Author: Jim Weirich
# Email: jim@gmail.com
name=${1:-"World"}

echo "Hello, $name"
```

```bash
git commit --amend --no-edit
git log --oneline
```

```
13f4350 (HEAD -> master) adding an author comment
d15a0da (tag: v1) changing hello.sh content 2 --adding lines 4 and 5
cd6cb8d (tag: v1-beta) changing hello.sh content 2
fc283f0 changing hello.sh content 1
```

### Move it

#### Moving hello.sh

Using Git commands, move the program hello.sh into a lib/ directory, and then commit the move.

```bash
mkdir lib
mv hello.sh lib/
git add lib/
git rm hello.sh
git commit -m "moving hello.sh to lib directory"
```

Create a Makefile in the root directory of the repository with the provided content and commit it to the repository

```bash
touch Makefile
```

```
TARGET="lib/hello.sh"

run:
	bash ${TARGET}
```

```bash
git add Makefile
git commit -m "adding Makefile"
```

### blobs, trees and commits

#### Exploring .git/ Directory

#### Latest Object Hash

Find the latest object hash within the .git/objects/ directory using Git commands and print the type and content of this object using Git commands.

```bash
git rev-parse HEAD
f0312c8514a0ccd4e66e86ca5df6e2f7b4a4804b
git cat-file -t f0312c8514a0ccd4e66e86ca5df6e2f7b4a4804b
```

```bash
git cat-file -t f0312c8514a0ccd4e66e86ca5df6e2f7b4a4804b
commit
```

```bash
git cat-file -p f0312c8514a0ccd4e66e86ca5df6e2f7b4a4804b
tree 8f9f976d92fb83910410302ad23ce223d4f77282
parent 0c3d3442afd1a5c46b7a69529cd3b4b5af4ba430
author erezzoug <rezougelmehdi0@gmail.com> 1770413209 +0100
committer erezzoug <rezougelmehdi0@gmail.com> 1770413209 +0100

adding Makefile
```

#### Dumping Directory Tree

Use Git commands to dump the directory tree referenced by this commit.

```bash
git cat-file -p 8f9f976d92fb83910410302ad23ce223d4f77282
100644 blob ea5f53171f00e91a8384b7854278b09bd0669373    Makefile
040000 tree ff6f1177c672dfeade001a9d5880c51cd7ef658e    lib
```

Dump the contents of the lib/ directory and the hello.sh file using Git commands.

```bash
git cat-file -p ff6f1177c672dfeade001a9d5880c51cd7ef658e
100644 blob 84a3d9f6511bb898ddac3549ed14f3f779c9bcbe    hello.sh
```

```bash
git cat-file -p ea5f53171f00e91a8384b7854278b09bd0669373
TARGET="lib/hello.sh"

run:
        bash ${TARGET}
```

### Branching

#### Create and Switch to New Branch

Create a local branch named greet and switch to it.

```bash
git branch greet
git switch greet
```

#### Create lib/greeter.sh

In the lib directory, create a new file named greeter.sh and add the provided code to it. Commit these changes.

```bash
touch ./lib/greeter.sh
```

```bash
vim lib/greeter.sh
#!/bin/bash

Greeter() {
    who="$1"
    echo "Hello, $who"
}
```

```bash
git add lib/greeter.sh
git commit -m "creating in lib directory a new file named greeter.sh"
```

#### Update the lib/hello.sh

Update the lib/hello.sh file by adding the content below, stage and commit the changes.

```bash
vim lib/hello.sh
#!/bin/bash

source lib/greeter.sh

name="$1"
if [ -z "$name" ]; then
    name="World"
fi

Greeter "$name"
```

```bash
git add lib/hello.sh
git commit -m "updating lib/hello.sh file"
```

#### Update Makefile

```bash
vim Makefile
# Ensure it runs the updated lib/hello.sh file
TARGET="lib/hello.sh"

run:
	bash ${TARGET}
```

```bash
git add Makefile
git commit -m "updating makefile"
```

#### Differences between master and greet

Switch back to the main branch, compare and show the differences between the main and greet branches for Makefile, hello.sh, and greeter.sh files.
(uncomplete - continue from here)

```bash
git diff greet -- Makefile lib/hello.sh lib/greeter.sh
```

```
diff --git a/Makefile b/Makefile
index 660574d..ea5f531 100644
--- a/Makefile
+++ b/Makefile
@@ -1,5 +1,4 @@
-# Ensure it runs the updated lib/hello.sh file
-# TARGET="lib/hello.sh"
-#
-# run:
-#      bash ${TARGET}
+TARGET="lib/hello.sh"
+
+run:
+       bash ${TARGET}
\ No newline at end of file
diff --git a/lib/greeter.sh b/lib/greeter.sh
deleted file mode 100644
index d5456ce..0000000
--- a/lib/greeter.sh
+++ /dev/null
@@ -1,6 +0,0 @@
-#!/bin/bash
-
-Greeter() {
-       who="$1"
-       echo "Hello, $who"
-}
diff --git a/lib/hello.sh b/lib/hello.sh
index 56a4589..84a3d9f 100644
--- a/lib/hello.sh
+++ b/lib/hello.sh
@@ -1,10 +1,8 @@
 #!/bin/bash

-source lib/greeter.sh
+# Default is World
+# Author: Jim Weirich
+# Email: jim@gmail.com
+name=${1:-"World"}

-name="$1"
-if [ -z "$name" ]; then
-    name="World"
-fi
-
-Greeter "$name"
\ No newline at end of file
+echo "Hello, $name"
\ No newline at end of file
(END)
```

#### generate README.md file

```bash
touch README.md
vim README.md
```

```bash
This is the Hello World example from the git project.
```

```bash
git add README.md
git commit -m "readme file commit"
```

#### Draw a commit tree diagram

Draw a commit tree diagram illustrating the diverging changes between all branches to demonstrate the branch history.

```bash
git log --oneline --graph  --all
```

```bash
* cc6b928 (HEAD -> master) readme file commit
| * 2929c40 (greet) updating makefile
| * a2cbeca updating lib/hello.sh file
| * 43efad5 creating in lib directory a new file named greeter.sh
|/
* f0312c8 adding Makefile
* 0c3d344 moving hello.sh to lib directory
* 13f4350 adding an author comment
* d15a0da (tag: v1) changing hello.sh content 2 --adding lines 4 and 5
* cd6cb8d (tag: v1-beta) changing hello.sh content 2
* fc283f0 changing hello.sh content 1
```

### Conflicts, merging and rebasing

#### Merge Main into Greet Branch

Start by merging the changes from the main branch into the greet branch.

```bash
git switch greet
git merge master
```

Switch to main branch and make the changes below to the hello.sh file, save and commit the changes.

```bash
git switch master
vim lib/hello.sh
```

```
#!/bin/bash

echo "What's your name"
read my_name

echo "Hello, $my_name"
```

#### Merging Main into Greet Branch (conflict)

Attempt to merge the main branch into greet. Bingooo! There you have it, a conflict.

```bash
git merge master
Auto-merging lib/hello.sh
CONFLICT (content): Merge conflict in lib/hello.sh
Automatic merge failed; fix conflicts and then commit the result.
```

Resolve the conflict (manually or using graphical merge tools), accept changes from main branch, then commit the conflict resolution.

```bash
vim lib/hello.sh
```

```
cat lib/hello.sh
echo "What's your name"
read my_name

echo "Hello, $my_name"
```

```bash
git add lib/hello.sh
git commit -m "conflict  resolved"
```

#### Rebasing Greet Branch

Go back to the point before the initial merge between main and greet.

```bash
git reset --hard 2929c40
```

Rebase the greet branch on top of the latest changes in the main branch.

```bash
git rebase master
```

Merging Greet into Main

```bash
git merge greet
```

#### Understanding Fast-Forwarding and Differences

Explain fast-forwarding and the difference between merging and rebasing.

### Local and remote repositories

In the work/ directory, make a clone of the repository hello as cloned_hello. (Do not use copy command)

```bash
git clone hello cloned_hello
```

Show the logs for the cloned repository

```bash
cd cloned_hello/
git log --oneline
```

```
96ef84c (HEAD -> master, origin/master, origin/greet, origin/HEAD) updating makefile
24940cf updating lib/hello.sh file
0f335e4 creating in lib directory a new file named greeter.sh
39eeb11 updating hello.sh
cc6b928 readme file commit
f0312c8 adding Makefile
0c3d344 moving hello.sh to lib directory
13f4350 adding an author comment
d15a0da (tag: v1) changing hello.sh content 2 --adding lines 4 and 5
cd6cb8d (tag: v1-beta) changing hello.sh content 2
fc283f0 changing hello.sh content 1
```

Display the name of the remote repository and provide more information about it

```bash
git remote
```

```
origin
```

```bash
git remote show origin
```

```
* remote origin
  Fetch URL: C:/Users/rezou/OneDrive/Documents/zone01/work/hello/
  Push  URL: C:/Users/rezou/OneDrive/Documents/zone01/work/hello/
  HEAD branch: master
  Remote branches:
    greet  tracked
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

List all remote and local branches in the cloned_hello repository

```bash
git branch -a
```

```
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/greet
  remotes/origin/master
```

Make changes to the original repository, update the README.md file with the provided content, and commit the changes.

```bash
vim README.md
```

```
This is the Hello World example from the git project.
(changed in the original)
```

```bash
git add README.md
git commit -m "updating readme content"
```

Inside the cloned repository (cloned_hello), fetch the changes from the remote repository and display the logs. Ensure that commits from the hello repository are included in the logs.

```bash
git fetch origin
git log --oneline --all
```

```
cc37f50 (origin/master, origin/HEAD) updating readme content
96ef84c (HEAD -> master, origin/greet) updating makefile
24940cf updating lib/hello.sh file
0f335e4 creating in lib directory a new file named greeter.sh
39eeb11 updating hello.sh
cc6b928 readme file commit
f0312c8 adding Makefile
0c3d344 moving hello.sh to lib directory
13f4350 adding an author comment
d15a0da (tag: v1) changing hello.sh content 2 --adding lines 4 and 5
cd6cb8d (tag: v1-beta) changing hello.sh content 2
fc283f0 changing hello.sh content 1
```

Merge the changes from the remote main branch into the local main branch.

```bash
git merge origin/master
```

Add a local branch named greet tracking the remote origin/greet branch.

```bash
git branch --track greet origin/greet
```

Add a remote to your Git repository and push the main and greet branches to the remote.

```bash
git remote add github https://github.com/elmehdi-rezoug/cloned_hello.git
git push github master greet
```

### Bare repositories

What is a bare repository and why is it needed?
Create a bare repository named hello.git from the existing hello repository.

```bash
git clone --bare hello hello.git
```

Add the bare hello.git repository as a remote to the original repository hello.

```bash
git remote add bare-remote ../hello.git/
```

Change the README.md file in the original repository with the provided content

```bash
vim README.md
```

```
This is the Hello World example from the git project.
(Changed in the original and pushed to shared)
```

Commit the changes and push them to the shared repository.

```bash
git add README.md
git commit -m "updating the readme file"
git push bare-remote master
```

Switch to the cloned repository cloned_hello and pull down the changes just pushed to the shared repository.

```bash
cd cloned-hello
git remote add bare-remote ../hello.git/
git pull bare-remote master
```
