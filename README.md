# gits
(Pronounced as git-yes)
Get out of apocalypse

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
