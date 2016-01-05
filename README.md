# gits
Get out of apocalypse

## Undo a git rebase
[Answer](http://stackoverflow.com/a/135614/2134124)

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
