# gits
(Pronounced as git-yes)
Get out of apocalypse

## Note: 
In most of the scenarios, it is assumed that the commits are not pushed to origin (remote) yet. 
If you are going to do force push, please know the consequences of doing so before going ahead.

## Undo a git rebase
[Answer by Charles Bailey](http://stackoverflow.com/a/135614/2134124)

Do git reflog and find the point where you want to go. (Ex: HEAD@{10})
You will find something like the below in ```git reflog```

    git reflog
    <sha> HEAD@{9}: rebase: action
    <sha> HEAD@{10}: commit: action

Use the value to do a hard reset.

    git reset --hard HEAD@{10}
    
The rebase you did is undone.

## Undo git merge since it fast forwarded whereas a merge commit was required

You can reset to the previous known commit in that branch, before the merge was done and after that
try merging with ```--no-ff``` argument, which will force git to add a merge commit.

    git reset --hard <sha>


    git merge test --no-ff
    
Or you can use git reflog to go back to the previous sane action and do merge again with ```--no-ff``` argument.
[See](https://github.com/Dineshs91/gits#undo-a-git-rebase) this for more information.


## Hunting a mysterious bug in your git history ?

Try git's bisect functionality.

    git bisect
    
## Modify the commit message of a specific commit

    git rebase -i head~2
    
Display interactive rebase for the last 2 commits from head. Choose the number based on where your commit
lies from head. If it is the 10th commit, choose head~10.
You will get something like below

    pick <sha> <commit message>
    pick <sha> <commit message>
    
Change the command to edit correspoding to the commit which you want to modify

    pick <sha> <commit message>
    edit <sha> <commit message>
    
Save and the rebase will stop at the 2nd commit. You can amend the commit to change the
commit message. Do git rebase --continue to finish the rebase.

    git commit --amend
    git rebase --continue

## Revert last commit

You have made a commit, but now you want to modify some changes that went into that commit.

[Answer by Esko Luontola](http://stackoverflow.com/questions/927358/how-do-you-undo-the-last-commit/927386#927386)

    git reset --soft HEAD~1
    
This will remove the commit, but the changes will be staged.

## Discard all unstaged changes

[Answer by Greg Hewgill](http://stackoverflow.com/a/52719/2134124)

    git stash save --keep-index
    git stash drop

## Recover a dropped stash

[Answer by Aristotle Pagaltzis](http://stackoverflow.com/questions/89332/how-to-recover-a-dropped-stash-in-git/91795#91795)

You have dropped a stash, but now you want it back.
If you know the hash of the dropped stash you can do the below.

    git stash apply $stash_hash

When you drop a stash, a hash of it is printed to the screen. If you haven't closed your terminal, you
can locate it by scrolling to the top. If you can't find it, execute the below command.

    gitk --all $( git fsck --no-reflog | awk '/dangling commit/ {print $3}' )

This will open gitk (git gui). To spot stash commits, look for commit messages of this form:

    WIP on somebranch: commithash Some old commit message

Once you know the hash of the commit you want, you can apply it as a stash

    git stash apply $stash_hash

## Copy a file from one branch to another branch.

[Answer by Madlep](http://stackoverflow.com/a/307872/2134124)

If you want to copy an entire file from one branch into another branch, do the below

    git checkout bugfix README.md
    
Execute the above command from the branch, where you want the file to be copied.

## Display the changes in a particular commit

If you want to review the changes done in a particular commit.

    git show <sha>

## Revert changes to a particular file using commit hash.

[Answer by mgalgs](http://stackoverflow.com/a/7196615/2134124)

Say you committed changes of 2 files. Now you want to revert the changes of one of the files.

    git show <sha> -- some_file.c | git apply -R
    
This command will undo the changes, but these are not staged. So you have to add and commit these.


## Get the no of commits (Commit count)

[Answer by Benjamin Atkin](http://stackoverflow.com/a/4061706/2134124)

To get the count for a specific revision

    git rev-list --count <revision>
    
To get the count across all branches

    git rev-list --all --count
