# Advanced Git - Functionality and Features
#### Brent Laster, SAS

## Agenda
* core concepts refresh
* merging and rebasing
* stash
* reset and revert
* rerere
* bisect
* worktrees
* subtrees
* interactive rebase
* notes (a way to augment the commit without altering the commit itself)
* grep

## Centralized vs distributed VCS

Centralized has a server which has to be connected.

distributed has everything local, like bazaar, hg, & git

## Git in One Picture
```
 public remote repository             {server}
  ^
 prod   local repository (commit to)  {
  ^
 test   staging area                   local machine
  ^
 dev    working directory             }
```

## Git granularity (what is a unit?)
* traditional source control is usually a file
* in Git, the unit is a tree

## Merging: what is fast forward
```
git checkout master
git merge hotfix
```

```
C0 <- C1 <- C2 <- C4
             \
              --- C3
```

## 3 way merge

Takes C2, C4, C5 and creates a merge commit for C6

```
C0 <- C1 <- C2 <- C4    C6
             \
              --- C3 <- C5
```

## Merging: what is a rebase?
Merge with history
```
                  master
C0 <- C1 <- C2 <- C4
             \
              --- C3 <- C5
```

```
C0 <- C1 <- C2 <- C4 <- C3' <- C5'
             \          /
              --- C3 <- C5
```

## Command: git stash
Takes work in progress and puts it in a staging area, and cleans out your area
back to the last commit

* `git stash -u` -- stashes even untracked files

* `git stash save "cool feature comment"`

* `git stash apply stash@{1}` - will put that stash back into your working area, I
can pick any one from the list.

* `git stash pop stash@{2}` - will take it out of the stash and puts it back into
your working area.

## Git reset
Like a rollback and puts you back to a previous commit
warning: --hard overwrites everything

Moves the branch pointer back

## git revert
More of an undo vs rollback (reset). Revert tries to cancel out the changes.
Adds a new commit that cancels out the changes of the commit I am reverting

## git rerere
reuse recorded resolution

teaching git when I see a conflict like this do it like this

enabled via in gitconfig rerere.enabled = 1

## git rerere 1

```
git checkout master
git config -global rerere.enabled 1
git merge feature -- during a conflict puts it into a staging area, but you have
            to resolve the conflicts
```

With rerere on, it records before you fix the conflict.

```
fix conflicts
git add .
git commit -m "finalize merge"
```

if you have the rerere, git records a post image after you resolved the
conflict. What is this good for?

## Git rerere 2
```
git reset --hard HEAD~1
git checkout master
git merge feature
   Merge conflict in "File B"
   Resolved 'File B' using previous resolution
```

## git bisect
binary search

## switching branches

## worktrees
multiple branches without cloning several clones. 
git worktree,

separate working directories tied back to the same working
repositories

```
 remote repository
 local repository (commit to)
 staging area
 working directory
```

```
 git worktree add -b exp tree1
 git worktree add -b prod tree2
```

## subtrees
git subtree add, allows including a acopy of a separate repository with your
current repository.

notes:
 * no links like a submodule - just a copy in a subdirectory
 * advantage - no links to maintain
 * disadvantage - extra content to carry around with your project

## Subtree overall view
```
                  parent project  \
subtree \                      proj_dir
     proj_dir/mod1
```

## subtree - adding a project as a subtree
`git clone foo.git myproject`

## interactive rebase
`git rebase -i` (I already use this)

## notes
`git notes` - add additional information to objects in the git repo or look at
such information

use case - at some point after making a commit, you may decide that there are
additional comments or other non-code information that you'd like to add with
commit

create a note - git notes add -m "this is an example of a note" 2f2ea1e

show a note - git notes show 2f2ea1e

## git grep
`git grep` - similar options to grep
