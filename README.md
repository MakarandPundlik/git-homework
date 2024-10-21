Repository
A repository (or repo) is a storage space for files, directories, historical records, commits, and heads. It can be local (on your computer) or remote (on a server like GitHub).

Some components of the repository:

.git Directory: contains all the configurations, logs, branches, HEAD, and more
Working Tree: the directories and files in your repository
Index: is the staging area in git. It’s basically a layer that separates your working tree from the Git repository.
HEAD and head: HEAD is a pointer that points to the current branch. A repository only has 1 active HEAD. head is a pointer that points to any commit. A repository can have any number of heads.
Commit
A commit is a snapshot of a set of changes. For example, if you added 5 files, and removed 2 others, these changes will be contained in a commit. Each commit has a unique ID.

Branch
A branch is a parallel version of your project. By default, Git creates a main (or master) branch. Branches allow you to develop features independently from the main codebase.

Stages of Git
Modified: Changes have been made to a file but file has not been committed to Git Database yet
Staged: Marks a modified file to go into your next commit snapshot
Committed: Files have been committed to the Git Database
Merge
Merging is the process of combining changes from different branches. It integrates the work from one branch into another.

Clone
Cloning is copying a repository from a remote server to your local machine.

Pull and Push
Pull: Fetches changes from a remote repository and merges them into your local branch.
Push: Sends your committed changes to a remote repository.
Basic Commands
#Create an empty Git repository.
git init

# Set & Print Some Basic Config Variables (Global)

$ git config --global user.email "MyEmail@ncsu.com"
$ git config --global user.name "My Name"

# Quickly check available commands

$ git help

#To intentionally untrack file(s) & folder(s) from git. Based on different languages,
echo "temp/" >> .gitignore
echo "private_key" >> .gitignore
There are different styles of gitignore file. reference: https://github.com/github/gitignore. For example, Python developers might consider using https://github.com/github/gitignore/blob/main/Python.gitignore.

# Clone a repository

git clone <repository-url>

# Check the status of your files. Will display the branch, untracked files, changes and other differences

git status

# list existing branches & remotes

git branch -a

# create a new branch

git branch <myNewBranch>

# delete a branch

git branch -d <myBranch>

# rename a branch

git branch -m <myBranchName> <myNewBranchName>

# Add files to staging area

git add <file>

# Commit your changes

git commit -m "Commit message"

# By default, git push will push and merge changes from the current branch to its remote-tracking branch

git push

# By default, git pull will update your current branch

git pull

# Create a new branch

git checkout -b <branch-name>

# Switching Branches

git checkout <branch-name>

# Restoring Files to a Previous State

git checkout <branch-name> -- <file-name>

# Checking Out a Specific Commit

git checkout <commit-hash>

# Merge a branch into the current branch

git merge <branch-name>
Special situation
Stash
You’ve been doing some work in your git repo, but you want to pull from the remote. Since you have uncommitted changes to some files, you are not able to run git pull. Instead, you can run git stash to save your changes onto a stack!

git stash

git pull

git status #checking if everything is okay

git stash list

git stash pop
#or
git stash apply
Fix merge conflix
When we merge current branch to main branch, some conflicts might happen, like:

Auto-merging filename.txt
CONFLICT (content): Merge conflict in filename.txt
Automatic merge failed; fix conflicts and then commit the result.
This means that Git has identified conflicting changes in “filename” that need to be resolved.
Open the files with conflicts in your text editor or IDE. Conflicts sections shows your current branch, named “HEAD”, and the branch you try to merge, named “branch-name”.

Step:

Resolve the Conflict: Edit the file to resolve the conflict by choosing the right changes to keep.
Stage the Resolved Files: Once you have resolved the conflicts, you need to stage the changes.
Commit the Merge: After staging the resolved files, commit the merge.
Finish the Merge Process: After committing, the merge conflict resolution is complete. There are some good visual tools to help with dealing conlicts like VSCodes, IntelliJ. To minimize the chance of conflicts, pull changes from the upstream branches regularly and resolve smaller conflicts incrementally. When working in a team, communicate about major changes or rebase regularly to reduce the chances of conflicts.
Reverse
In Git, “reverse” typically refers to undoing changes or reverting a commit. There are several ways to reverse changes in Git:

Revert a Commit: Creates a new commit that undoes the changes made by a previous commit without rewriting history. This is useful when you want to keep the history intact and record the fact that a commit was reverted.
sh git revert <commit-hash>

But if you have merged a branch into your main branch, but later realized that you need to undo this merge.

#find the commit hash of the merge that you want to reverse
git log --oneline
#use "-m" option to specify the parent of the merge you want to keep. The "-m 1" option typically specifies that you want to keep the changes from the first parent (usually the main branch before the merge).
git revert -m 1 <merge-commit-hash>
Reset a Commit: Moves the HEAD pointer and potentially the branch pointer to a previous commit, effectively “erasing” commits from the history. This can be destructive as it changes the commit history, making it as though certain commits never happened.
The git reset command is used to undo commits by moving the HEAD and the current branch to a specified commit. There are three modes of reset:

soft: Moves the HEAD to a specified commit and leaves changes staged for commit.
mixed (default): Moves the HEAD to a specified commit, unstages changes (moves them to the working directory), but doesn’t touch the working directory.
hard: Moves the HEAD to a specified commit and deletes all changes in the working directory and index.
git reset [--soft | --mixed | --hard] <commit-hash>
Important: Using –hard will lose all changes after the specified commit, and they cannot be recovered easily.

Undo Changes in the Working Directory: Reverts changes made to files in the working directory that haven’t been staged or committed yet.
If you want to undo changes to a file that haven’t been staged or committed yet, you can use the “git checkout” (in older versions) or “git restore” (in newer versions) command. sh git restore <file> #or for older version git checkout -- <file>

Unstage Changes: If you’ve added changes to the staging area (using “git add”) and want to unstage them without losing changes in the working directory: sh git reset HEAD <file>

Reflog to Recover Lost Commits: If you accidentally use “git reset –hard” or otherwise lose commits, you can often recover them using the Git reflog, which tracks all movements of HEAD. sh git reflog Find the commit hash of the desired state, and then reset or checkout to that commit.
