# UNIX COMMANDS

`ls`: Shows files in the directory. Use tab to autocomplete.
`ls -l`: Shows files in the directory with additional info.
`ls -a`: Shows also hidden files.

`clear`: Clears the terminal. (ctrl-l)

`open .`: Opens directory as a folder.
`open Desktop`: Opens desktop as a folder. (Just an example you can pick whatever destination is needed.) 

`cat file.txt`: View the contents of the file in the terminal.

`code .`: Opens directory in vs code.

`pwd`: Print Working Directory. Shows the current location.

`cd`: Change directory. 
`cd..`: Go back one level.

`touch`: Create a file or files. (touch text1.txt text2.txt text3.py text4.pdf) To create a file in a different directory use the touch command with a path such as a touch Desktop/Folder1/text1.txt

`mkdir`: Creates a new directory/folder (or directories)
 
`rm`: Delete a file or files. (Not to the trash bin, permanently removes them)
 
`rm -rf`: Deletes a directory. (r: recursive, f: force)
 
# GIT COMMANDS

[Official Documentation](https://git-scm.com/docs)
 
`git status`: Gives information on the current status of a git repository and its contents.
 
`git init`: Create a new git repository. Do once per project and initialize in the top-level folder containing the project.
rm -rf .git deletes the folder that makes a folder git repository and we can check this with the git status command.

## GIT ADD & GIT COMMIT

![gitLifecycle](https://user-images.githubusercontent.com/43893190/183290757-f387f46e-6f6f-4bd3-b1a0-725bc361e7d7.png)

Working Directory --git add--> Staging Area --git commit--> Repository

The commits should be atomic. A commit should encompass a single feature, change or fix. It makes it much easier to undo or roll back changes. 

`git commit`: Every checkpoint made in the project. Command to commit changes from the staging area.
`git commit -m "my message"`: -m flag allows us to pass in an inline commit message. If this flag is not written a text editor would launch automatically and will wait for the commit message.
`git commit -a`: Add all changes to tracked files and commit them.

`git add`: Stage changes to be committed. Group specific changes together, in preparation for committing. git add file1 file2
`git add .`: Stage all changes at once.
`git rm -r --cached .`: Remove all files from staging area. `git rm --cached file1`: Removes only specified files.

`git log`: Log of commits. (press q to exit log window in terminal later)
`git log --oneline`: Shorter commit messages.

### Amending Commits

Sometimes the wrong commit is made (forgotten files or typo in the commit message). Rather than creating a new separate commit, redo the previous commit using the `--amend` option. It is only useful to redo the mistake you did one commit ago.

If you forgot files, add forgotten files in a staging area before calling the command `git command --amend`. If you only want to change the commit message just call the command. 

### Ignoring Files with .gitignore

Ignoring the files and directories in a given repository using a .gitignore file. By making this change git will no longer track the changes made in those files. This is useful for files like:
- Secrets, API keys, credentials
- OS files
- Log files
- Dependencies & packages

Create a file called .gitignore in the root of a repo. (you can put it anywhere but the root is common practice)
- .abc will ignore files named .abc
- folderName/ will ignore an entire directory
- *.log will ignore files with .log extension

## GIT BRANCH

Alternative timelines for a project. If a merge does not happen changes made in one branch do not affect the other branches.

### Head -> master

The head is the pointer that refers to the current location in the repository. When using the command git log this can be seen at the most recent commit. The example above head tells us that we are at the master branch.

`git branch`: List of existing branches. * indicates the branch you are currently on. `git branch -v`: Last commit for every branch. 

`git branch <branch-name>`: Creates a new branch upon the current head but does not switch to this branch (head stays the same). 

`git switch <branch-name>`: Switch to the given branch.
`git switch -c <branch-name>`: Creates a new branch and switches to it. (`git checkout -b <branch-name>` does the same)  

`git checkout <branch-name>`: Historically, this command was used to switch branches. It still works but unlike switch, it has other functionalities.

`git branch -m <branch-name>`: You have to be on the branch you want to rename.

**Important fact:** If changes are not committed or stashed all progress would be lost during the branch switching. Files that are not in the old branch but created during the new branch act differently. If these files are not committed then they will follow through new branch.  

### Deleting branches

[Delete branches](https://www.cloudbees.com/blog/git-delete-branch-how-to-for-both-local-and-remote)

`git branch -d <branch-name>`: Deletes the branch with the given name. (commits are still there only reference is deleted)

- Will give an error if you try the delete the branch you are currently on.  (Cannot delete branch <branch-name> checked out at 'path')

- If you switch to a different branch and try to delete the branch you tried before then a different error would show up. (The branch <branch-name> is not fully merged)

`git branch -D <branch-name>`: Allow deleting the branch irrespective of its merged status.

## GIT MERGE

[Git Merge Tutorial](https://www.atlassian.com/git/tutorials/using-branches/git-merge)

- Branches are merged, not specific commits.
- Always merge to the current head branch.
- Merging does not mean that the previous branch whose information was added is lost. We can continue to develop it as it is a separate branch.

Two steps in merge:

1. Switch (or checkout) to the branch you want to merge the changes into. 

2. Use the `git merge` command to merge changes from a specific branch into the current branch. 

### Fast Forward Merge

<img src="https://user-images.githubusercontent.com/43893190/183354974-63b2ac8f-f713-4c19-a32a-c1fd6de52585.jpg" width="380" height="500" />

### Typical Merge

Rather than performing a simple fast forward, git makes a new commit on the master branch and will expect a commit message from you. This commit unlike others has two parents. 

### Merge Conflicts

Depending on the specific changes, Git may not be able to automatically merge. These merge conflicts should be manually resolved. Git changes the content of the files to indicate the conflicts that it wants you to resolve. 

- The content came from the Head branch is displayed **between the <<<<<HEAD and ====== symbols.**

- The content from the branch you are trying to merge from is displayed **between the ====== and <<<<< symbols.**

1. Open the file or files with a merge conflict.
2. Edit the file or files to remove the conflict. Decide which branch's content you want to keep in each conflict. (or keep both contents) It is not just and, or operation you can type whatever you want in the file.
3. Remove the conflict markers in the document.
4. Add changes and make a commit.

## GIT DIFF

- Lists all the changes in the working directory that are not staged for the next commit.

- View changes between commits, branches, files, working directories and more. 

- It does not impact the repository just gives info like git log or git status command.

Explanation of the output of git diff:

`diff --git a/diff_test.txt b/diff_test.txt`: Comparison unit (which files are being compared, usually two versions of the same file)

`index 6b0c6cf..b37e70a 100644`: File metadata

`--- a/diff_test.txt`: Legend that assigns symbols to each diff input source. Changes from a/diff_test.txt are marked with a ---.

`+++ b/diff_test.txt`: Changes from b/diff_test.txt are marked with the +++ symbol.

The remaining diff output is a list of diff *chunks*. Not the entire content of the file just the modified parts. 

`@@ -1 +1 @@`: Each chunk is prepended by a header enclosed within @@ symbols. The content of the header is a summary of changes made to the file.

For example if this would be like this `@@ -34,6 +34,8 @@` then it would mean that 6 lines have been extracted starting from line number 34. Additionally, 8 lines have been added starting at line number 34.

`-this is a git diff test example`: Line that begins with - comes from file A.

`+this is a diff example`: Line that begins with + comes from file B.

`git diff HEAD`: Lists all changes in the working tree since the last commit. (git diff only includes unstaged changes, this also includes staged changes) 

It is not possible to view a newly created file either inside or outside the staging area with the `git diff` command. But if the newly created file is transferred into the staging area, we can see the change that a new file has been created with the `git diff HEAD` or `git diff --staged` command.

`git diff --staged` or `git diff --cached`: List the changes between the staging area and the last commit. (shows what will be included in the commit if we run it right now)

`git diff HEAD filename`  or  `git diff --staged filename`: View the changes within a specific file. To view multiple files separate them by spaces.

`git diff branch1 branch2`: List the changes between the tips of branch1 and branch2. 

`git diff commit1 commit2`: Compare two commits by giving commit hashes. (To view commit hashes use command git log --oneline)

 ## GIT STASHING
 
Save the changes made in the working directory or staging area (changes that are not committed yet) and go to a different branch. In different branch you can continue your work, you can create new branches or commits, they do not affect these stashed files.

What happens if we have uncommitted changes in one branch and we want to switch to the other branch?
 
There are two scenarios:

1. Changes come to the destination branch. 
 
2. Git won't let to switch if it detects potential conflicts. (Error: Please commit your changes or stash them before you switch branches.)
 
`git stash` or `git stash save`: Saves changes without committing them. You can stash changes and then come back to them later. This command will take all changes (staged and unstaged) and stash them. 

`git stash pop`: Remove the most recently stashed changes in your stash and re-apply (to a different branch or the same one) them to your working copy.
 
 `git stash apply`: Similar to pop, it will apply whatever was stashed away, the difference is that this command does not remove anything from the stash. This is useful if you want to apply stashed changes to multiple branches. 
 
 `git stash drop stash@{2}`: Deletes a particular stash. 
 
 `git stash clear`: Clears all stashes.
 
 ### Stashing Multiple Times
 
 Stashes can added onto the stack of stashes. They will be stashed in the order you added them. 
 
 `git stash list`: Viewing what is in the stash.
 
 `git stash apply stash@{2}`: With `git stash apply` the most recent stash would be applied but we can also specify a particular stash like this.
 
 ## UNDOING CHANGES
 
 `git checkout commit-hash`: View the previous commit. (7-digit commit-hash is enough) You can just observe the previous commit or you can go back to that commit and create a new branch from there (head will be automatically re-attached). 
 
 `git checkout HEAD~1`: We can also reference the previous commit relative to the HEAD. `HEAD~1` refers to the commit before HEAD (parent), `HEAD~2` refers to the 2 commits before HEAD (grandparent)
 
 When running this command **detached HEAD** warning will be displayed. This means that head is no longer referencing a branch (head --> master --> last commit in master), head now referencing a commit directly (head --> commit that is specified with commit hash). 

 `cat .git/HEAD`: This shows where head is currently referencing. If it references a branch output will be something like refs/heads/master but in the detached head state output will be a commit hash. 

`git switch branch-name`: Re-attaches head. `git switch -`: Re-attaches the head to the last branch we were on. 

`git checkout HEAD <file>` or `git checkout -- <file>` or `git restore <file>`: Discard changes in a file. Restores the files with the content of the last commit (only the files that are not in the staging area).

`git restore --source HEAD~1 file`: By default `git restore <file>` restores using HEAD as the default source. With this command we can change the source and restore files not only from the last commit. (also commit hash can be used for source)

`git restore --staged <file>`: Remove the files that have been added to the staging area and unstage them.

`git reset <commit-hash>`: Resets the repo back to a specific commit. (all the other commits after that are gone) It just removes the commits but the changes are still in the working directory. It is useful when you want to transfer the work to another branch because the work is not lost, just commits are deleted. 
 
`git reset --hard <commit>`: Similar to the previous command but the working directory changes too. Changes are also removed from the working directory. 

`git revert <commit-hash>`: Similar to git reset, both of them undo the changes but the way they accomplish it differs. `git reset` moves the branch pointer backwards and treats the commits that came after like they never existed. `git revert` creates a new commit and undo the changes there thus it excepts a commit message. 

## REMOTE REPOSITORIES 

`git clone <url>`: Clone a remote repository hosted on Github or similar websites. 

`git remote`: View any existing remotes for your repository. (`git remote -v` for more info)

`git remote add <name> <url>`: Adding a new remote. Remote is a connection between git and Github. Origin is a conventional git remote name. 

You can have more than one remote. For example, if you forked a project from Github, you will have two different versions of the project. One is your fork and the other one is where you forked this project from. If you want to get the updated version of the project git pull origin command will not be enough because it will only bring the changes from the forked repo. You need to add an additional remote (for remote name upstream is recommended).

`git remote remove <name>`: Deletes remote.

`git remote rename <old> <new>`: Renames the remote.

`git push <remote> <branch>`: Adding (pushing) some work up to the Github. You don't have to be on the branch you are currently pushing to the github. Use `git push <remote> <local-branch>:<remote-branch>` command if our local branch has a different name than a remote branch. 

`git push -u origin master`: -u allows us to set the upstream of the branch we are pushing. Without this command we need to specify the names of the branches in git push command. We need to run this command only once and branches would be connected. 

### Remote Tracking Branch

In remote repositories we have 2 different references. One reference is our reference in local and the other one is the remote tracking branch. It is a reference to the state of the branch on the remote. It can not move by itself, it is just a last known commit to us on this branch on origin. 

`git branch -r`: View the remote branches our local repo knows about. 

`git checkout origin/master`: You can checkout to the remote branch pointer, this way you can view how did repo looked when you cloned it last time. (origin/master is just an example it could be origin/main or any other branch)

### When cloning where is other branches?

When the repo is cloned all data and Git history will be available on the local machine but this does not mean that the whole project will be in our workspace. For example if we clone URL link of the main branch, other branches would not be visible on our machine. We can see remote branches with `git branch -r` command but `git branch` command will only show one record. 

By default master branch is already tracking origin/master.
To make a connection to other branches we just need to run the command `git switch <remote-branch-name>`. It will create a local branch and sets it up to track the remote branch origin/branch.

### GIT FETCH & GIT PULL

`git fetch <remote>`: Download changes from a remote repository but do not integrate them with working files. `git fetch <remote> <branch>` fetch a specific branch from a remote. This command will create a different branch than the current one. To view the changes use the command `git checkout origin/main`. (origin/main is just an example)

`git pull <remote> <branch> `: Unlike fetch, pull updates our head branch with changes retrieved from the remote. Like git merge it matters where we run this command because we run it where we want to changes be merged into. So this command can result in merge conflict too. 

`git pull`: Shorter syntax. Git assumes that remote is origin and branch is whatever tracking connection is configured for our current branch. 

<img src="https://user-images.githubusercontent.com/43893190/183743818-54a9dc34-de45-4603-ab9c-53258df82cfb.png" width="600" height="450" />

## GIT REBASE

First, switch to the branch you want to rebase and then rebase this branch onto some other branch (in the example above it is the master).

```
git switch feature
git rebase master
```

Two main ways to use the git rebase command:

1. Alternative to merging
2. Cleanup tool

<img src="https://user-images.githubusercontent.com/43893190/183910115-422f66ee-2a4f-4673-97bd-b92ae4188ca7.png" width="400" height="250" />

When the master branch is very active then the feature branch's history will be very complex because if it wants to keep up with the master it should merge the master branch's work to itself regularly. We can instead use rebase. When we rebase the feature branch onto the master branch it moves the entire feature's history at the tip of the master branch. 

Instead of commit, it rewrites history by creating new commits at the end of the original.

**Never rebase the commits that you already shared with someone. Because you are changing history with rebase and commit hashes would be different too.**

### Interactive rebase
 
Rewrite, delete, rename and reorder commits before sharing them with git rebase. Running git rebase with -i will enter the interactive mode. 
 
`git rebase -i HEAD~4`: We are not rebasing onto another branch. We are rebasing a series of commits onto the HEAD they are currently on. (with HEAD~4 will go to the 4th commit from top) Commit hashes of all the commits after selected one will change. 
 
After running the command above text editor will open. To alter the commits we need to specify some commands. The list of commonly used commands is shown below. 
 
 - **pick:** Use the commit.
 - **reword:** Use the commit but edit the commit message.
 - **edit:** Use commit but stop for amending.
 - **fixup:** Use commit contents but meld it into the previous commit and discard the commit message.
 - **drop:** Remove commit.
 
 ## GIT TAGS
 
 ![git_workflow](https://user-images.githubusercontent.com/43893190/184199189-d50f375c-1160-405e-b148-199b97ed8ff9.png)
 
 There are two types of Git tags.
 
 1. Lightweight tag: Name/label that points to a particular commit.
 
 2. Annotated tag: Stores extra data such as the author's name, email, date and tagging message. 
 
 ### Semantic versioning
 
[Documentation about semantic versioning](https://semver.org/)

Tags are mostly used for different versions of the project. It is important to know what semantic versioning means. For example, if we have a release like 1.3.6 digit 1 specifies the **major release**, 3 specifies the **minor release** and 6 specifies the **patch release**.

Patch releases do not contain new features, they are typically used for changes like bug fixes that do not impact how code is used. 

Minor releases include new features or new functionality. Everything is backward compatible so there are no breaking changes. When there is a minor release you should reset your patch number back to 0.
 
Major releases signify changes that are no longer backward compatible. With a major release, both patch number and minor release number should reset to 0. 
 
 ### Tag commands

`git tag`: List all the tags in the current repository. 
 
`git tag -l "*beta*"`: Search tags that match a particular pattern. (*beta* is just one example of wildcard patterns)

`git checkout <tag>`: Views the commit with the specified tag. 
 
`git diff <tag1> <tag2>`: Difference between two commits with specified tags.
 
`git tag <tagname>`: Creates a lightweight tag. A tag will be created wherever HEAD is currently referencing.
 
`git tag -a <tagname>`: Creates an annotated tag. 
 
`git show <tagname>`: More information about the tag. 
 
`git tag <tagname> <commithash>`: Tag not only commit what is HEAD showing. With this command you can tag any commit.
 
`git tag <tagname> <commithash> -f`: Tag names should be unique. If we put a tag in wrong commit, we can move it with this command. 
 
`git tag -d <tagname>`: Deletes a tag. 
 
 ### Pushing tags
 
 By default, the git push command does not transfer the tags to remote servers. 
 
`git push --tags`: Transfers all of the new tags to the remote server.
 
`git push origin <tagname>`: Transfers specified tag. 
 
 ## GIT REFLOGS
 
 Reflogs are only local, they are not shared with collaborators. Also they expire (around 90 days).
 
 Git also keeps track of references history. For example HEAD was pointing to commit B, then HEAD was pointing to commit C and in C reference was switch from branch feature to main. Briefly, git stores all the places HEAD reference was. 
 
 To view this reflog file go to the .git folder. There is a directory named logs. 
 
 `git reflog show HEAD`: Will show the log of a specified reference. (by default HEAD) To view the main branch instead of HEAD search for main. 
 
 `name@{qualifier}`: Access specific git ref. For example `git checkout HEAD@{2}` is different than HEAD~2. In ~2 notation we are referencing where HEAD was 2 commits ago but in @{2} notation there can be a thing other than commits like switching branches, some rebasing even detached head warnings.
 
 `git diff HEAD HEAD@{yesterday}`: We can also use the time to view changes. 
 
 `git reflog show HEAD@{3.days.ago}`: Shows all the changes in the HEAD reference starting from 3 days ago. 

With reflogs, we can access commits that seem lost and do not appear in git log. If we used git reset --hard the commit we deleted are no longer in git log. To get the commit information back we can use git reflog to get commit hash. To get the commit back use `git reset --hard master@{1}` command. master@{1} just a reference to that commit in git reflog.
