# UNIX COMMANDS

`ls`: Shows files in directory. Use tab to autocomplete.
`ls -l`: Shows files in directory with additional info.
`ls -a`: Shows also hidden files.

`clear`: Clears the terminal. (ctrl-l)

`open .`: Opens directory as folder.
`open Desktop`: Opens desktop as folder. (Just an example you can pick whatever destination is needed.) 

`pwd`: Print Working Directory. Shows current location.

`cd`: Change directory. 
`cd..`: Go back one level.

`touch`: Create a file or files. (touch text1.txt text2.txt text3.py text4.pdf) To create file in different directory use touch command with path such as touch Desktop/Folder1/text1.txt

`mkdir`: Creates a new directory/folder (or directories)
 
`rm`: Delete a file or files. (Not to trash bin, permanently removes them)
 
`rm -rf`: Deletes a directory. (r: recursive, f: force)
 
# GIT COMMANDS

[Official Documentation](https://git-scm.com/docs)
 
`git status`: Gives information on the current status of a git repository and its contents.
 
`git init`: Create a new git repository. Do once per project and initialize in the top-level folder containing the project.
rm -rf .git deletes the folder that makes a folder git repository and we can check this with git status command.

## GIT ADD & GIT COMMIT

![gitLifecycle](https://user-images.githubusercontent.com/43893190/183290757-f387f46e-6f6f-4bd3-b1a0-725bc361e7d7.png)

Working Directory --git add--> Staging Area --git commit--> Repository

`git commit`: Every checkpoint made in the project. Command to actually commit changes from the staging area.
`git commit -m "my message"`: -m flag allows us to pass in an inline commit message. If this flag is not written a text editor would launch automatically and will wait for commit message.
`git commit -a`: Add all changes of tracked files and commit them.

`git add`: Stage changes to be committed. Group specific changes together, in preparation of committing. git add file1 file2
`git add .`: Stage all changes at once.
`git rm -r --cached .`: Remove all files from staging area. `git rm --cached file1`: Removes only specified file.

`git log`: Log of commits.
































