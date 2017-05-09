# git-cheat-sheet
brain dump / rough notes for a Git newcomer

******************************************
Configure
 
git config --system
git config --global   <<user>>
git config <<project>>
 
git config -l          <<list>>
 
git config --global user.name "Graham-Phillips"
git config --global user.email ""
 
git config user.email

Configure command prompt to show branch:
prompt stored in variable PS1
echo $PS1
// to see what it is. google for info
export PS1='>>>>'

so in git bash do;
export PS1='\W$(__git_ps1 "(%s)") > '

export command doesnt persist beyond the window. Edit bash profile or rc file  

on Win have this

export PS1='\W$(__git_ps1 "(%s)") > '


in .bash_profile in user folder

*****************************
 
***********************
create:
git init
 
git add .
 
git commit -m "message"
 
see the commit log:-
git log
 
--since   --until=   --author=
 

******************
navigate commit tree
 
tree-ish - references tree of files in git
 
branch ref (tip of branch), tag ref (not in this tutorial)
 
Use any of above and then refer to ancestry
 
eg
Parent
-HEAD^   - the caret points to parent.
acf873264^
 
or use ~
HEAD~1  number of generations going up
 
Grandparent
HEAD~4
HEAD^^^^   -obvs using ~ is more compact
 
---------------------------------------
Listing the tree:
 
git ls-tree <tree-ish>
 
eg:
git ls-tree master
 
git ls-tree master assets/   - look in folder
 
git ls-tree master^ assets/   -- back one commit
 
 
***************************************
 
Using the log
 
git log --oneline
 
***************************************

Enable Collaborators
 
github, can add usernames in collaborators
 
1. make fork
if a github thing, post an issue documenting that you are doing a
feature
2. as you have forked you now have access on your own github
3. go to github for original, raise pull request
 
 
****************************************
Collaboration Workflow
 
git checkout master
git fetch
git merge origin/master
//ready to work as were in sync with remote
// create work branch
git checkout -b branch_name // create new branch
git add xxxxx
git commit -m "jkhjk"
git fetch
git push -u origin <branch_name>   // -u makes it a tracking branch
 
Other workers flow:
git checkout master
git fetch
git merge origin/master
// checkout the branch that was pushed up before
git checkout -b branch_name origin/branch_name
git log // to see what has happens
git show <sha> //or use HEAD or branch name for last
git commit -am "sdfdsfds" // a option adds to staging&commits
git fetch
git push
 
My work..
git fetch
// before merging take a look
git log -p <branch>..<origin/branch>    // -p=patch
// looks ok so merge
git merge origin/<branch>
// simple ffwd
// Next, feature is finished- merge our branch back into master as
we are done
git checkout master
git fetch
git merge origin/master // if htere were any changes in the fetch
git merge <branchname> // merging our branch into master
git push
 
**********************************
 
Tools, next steps
 
Kbd aliases- user dir, git config
 
user directory:gitconfig
 
git config --global alias.st status  // st is a new short alias for
status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.df diff
git config --global alias.dfs diff --staged  (diff between staging
index and repo)
 
git br -r  // u can use flags with abbreviated
 
 
git config --global alias.logg "log --graph --decorate --oneline
--abbrev-commit --all"
 
****************************
Using ssh keys to login, instead of having to dialog every time.
see github help, generating ssh keys
 
Remember on git hub to get project link in ssh format, not http
 
 
****************************
 
Tools...
 
GitWeb, needs a webserver
lets you see your git repo as a mini website
 
GUI app:
TortoiseGit
Git Extensions
SmartGit
GitHub client
 
git.wiki.kernel.org/index.php/InterfacesFrontendsAndTools
 
 
Hosting...  Gitolite -- make a private repo


**********************************************************

HEAD points to tip of current branch in repository
- eg, where commit writing will take place


See what has changed:-
git uses diff. 

git diff   << shows changes in workign directory>>
git diff --staged << shows only differences in staging dir >>

Deleting

method 1..  delete the file, then do
git rm <filename>
git commit...

method 2..easier
git rm <filename>
..deletes it and adds wit to staging so no need for a git add
git commit...

Safety feature: If file was modified and added to the index already, we must force its removal with -f flag.

Remove multiple files:
$ git rm log/\*.log
// removes all files that have the .log extension in the log/ directory

git rm \*~
// removes all files that end with ~.

Moving, Renaming
method 1, like delete, do in OS, then tell git
method 2, do it with git, again like delete

Rename a file in OS, git sees it as deleted, and an untracked file being present.
if we then do git add on the new file name and git rm on the old filename, it now sees it as a renamed file

method 2
(note rename and move are same)
git mv <oldname> <newname>
no need to add, automatically staged. Then git commit

 
When diffing files, uses less pager to show results - 
press: - shift-S folds long lines
-S  again

********************

git diff --color-words <<filename>>
.. this shows the changes in a more concise way

***********************
COMMITTING SHORTCUT

git commit -a 
... automatically does the staging before committing. Files that are not tracked or deletions are not included, so not always the best choice
git commit -am "message"

git add {directory name}/  add all files in this dir

UNDOING
******
-stuff in working, staging and repository

1 Undo in working dir
undo change to a file:
git checkout <<filename>>  
however, because checkout can work with branches as well as files, to ensure we arent checking out a branch, use -- to specify stay on same branch

git checkout -- <<filename>>

2 Undo staging changes
git reset HEAD <<filename>>
(git status reminds us of this)

git reset without --hard option only touches staging, not WD.

3 Undoing commits to repository..... more tricky

If a file's contents needs to be changed.. 
edit file, add tostaging
then: 
git commit --amend -m "msg"

Made an initial commit but forgot a file and dont want a seperate commit:-
git commit -m 'initial'
git add forgotten_file
git commit --amend
-2nd commit replaces result of first to end with a single commit



can also use to change commit message
git commit --amend -m "different msg"

//takes your staging area and uses it for the commit. If you’ve made no changes since your last commit (for instance, you run this command
immediately after your previous commit), then your snapshot will look exactly the same, and all you’ll change is your commit message

TO CHANGE OLDER COMMITS...
cant really do this, would change data integrity of a commit which is bound to a checksum, so better to do a new commit to undo older commits

So to change a committed file, we could manually make changes and commit the result. Another possibility is to check out an older version.
git log
copy checksum of the commit before the one where the change to revert is
dont need whole checksum, 10 or so chars uis usually ok

git checkout <<checksum>> -- {filename} 
( -- means current branch )
this puts it into staging area. if we now commit, we overwrite the current one with the old version 

to recap>
git reset HEAD {filename} - remove from staging, back in working directory
git checkout -- {filename} - removes from working diirectory
***********************

Another way to undo changes for commit- git revert
Does the opposite of the commit to reverse it
get part of the SHA ref

git revert <SHA> 
Does it all in one go. Pops up commit msg to edit

If other things have changed after the commit you wnt to revert, files nmay have moved etc...
Git uses merge rules.. ie merge between current branch and new set of changes.

**************************
Use reset to undo multiple commits
DANGEROUS!

git reset   changes where HEAD pointer points
reset options, soft, mixed, hard
git reset --soft   Just moves the HEAD pointer
does not change staging index or working dir, just resets repository

git reset --mixed    Default. Moves HEAD, and changes staging to match repository, but not working dir

git reset --hard    Moves HEAD, changes staging and working dir to match.. obliterates any changes that happened after the new HEAD point


***************************
Soft Reset
worth copying git log to a text file before moving HEAD

cat .git/head
refs/heads/master

git reset --soft <SHA>

After doing this we still have any changed files in staging index and working dir, which we can commit if we want.

You can undo the revert (if there have been no other commits)  by doing another git reset --soft and using the original HEAD sha

*******************************
Mixed reset

git reset --mixed <SHA>
Comes back with 'Unstaged changes after reset'
showing any changed files that were committed previously, now unstaged.

git diff will show the changes

So, same as reset soft, but with changes unstaged/uncommitted

Of course, we can use git reset to unstage a staged file, eg: git reset HEAD <filename>

Because we have not yet recorded any mmore commits the old commit is still there and we can go back to it with:

git reset --mixed <future version SHA>

**********************************
Hard reset
- doesnt keep changes in staging and WD... easiest way to lose data
So if you don't care about changed files and just want to go back to a precious commit, use this option

Old commit(s) are still there at this stage and we can go back to them with:
git reset --hard <SHA>
(eventually this would have been garbage collected. Note it would be difficult to go back to it without the SHA ref, so dont just rely on git log, take a copy of the log. 

If we make any changes and do a commit now, the old commits will now be lost.

**************************
Removing untracked files
Because it is destructive, we have cmd line flag for safety
-n test run...
git clean -n

tells you what it wiould remove
-f
git clean -f
-actually removes (destroys- permanently deleted) the files
Any files that were added to staging are safe

***************************

gitignore
comments: #

 - either filnames or simplified regex with basic wildcards:
* ? [aeiou] [0-9] !

ignore any .php file:
*.php
!index.php - but dont igore index.php

ignore a whole directory- use trailing slash:
assets/videos/

see github-
help.github.com/articles/ignoring-files
github.com/github/gitignore


gLOBAL IGNORES:

configure git to globally ignore:-
User specific, not repository specific

git config --global core.excludesfile ~/.gitignore-global

Good idea to locate it next to any global gitconfig

running above command adds the exclude file to yoru gitconfig file

******************************

Ignoring Tracked Files...
so a file that is tracked, change to ignored. Git will still track a file if it was added before the ignore rule was added

git rm --cached <filename>
remove file from staging index, not repository or WD

status will say it is deleted...
then commit... 
file will stay in directory

********************************
Tracking empty directories
Git ignores empty dirs.

well I think its very unlikely but you may have noticed I am the worlds number 1 worrier.

put a small dummy file, by convention call it 
.gitkeep

******************************
Navigating the commit tree

Referencing commits- "treeish": something that references part of the directory tree

tree-ish things: 
* full SHA1 hash
short SHA-1 hash - at least 4 chars on small project, unambiguous: 8-10 chars, massive project, use all

* HEAD pointer
* branch ref, tag ref



======================
git log   --see help pages for info

git log --oneline    -v useful for overview

git log --oneline -3   //show 3 commits

git log --since="2012-06-20"    //or use after
also until/before

Could do

git log --since="2 weeks ago" --until="3 days ago"

git log --since=2.weeks --until=3.days

git log --author="Graham-Phillips"

Can also grep commit msgs:--
git log --grep="temp"

SHA commit range:-
git log 2907d12..acf8750 --oneline

what has happened to index.html from a certain commit
git log c4b913e.. index.html

Get more details

git log -p   //p is patch option, shows diff of each change

git log --stat --summary  //shows stats, with a summary

git log --format=oneline   // returns full SHA as opposed to partial with --oneline

git log --format=short
git log --format=medium   //default
git log --format=full
git log --format=fuller
git log --format=email
git log --format=raw

git log --graph  // especially useful for branches

git log --oneline --graph --all --decorate

Format your own log output:
git log --pretty=format:"%h - %an, %ar : %s"
%h-abbreviated commit hash
%an-author name
%ar-author date relative
%s-subject
see help for others

Show last commit where a specific string was added or removed:

git log -S<string>
// handy for searching for when a func was added/removed

Using path as a filter to only log changes to a directory or fle(s). preceded path with -- and have as last param

limit options:
-(n) Show only the last n commits
--since, --after Limit the commits to those made after the specified date.
--until, --
before Limit the commits to those made before the specified date
--author Only show commits in which the author entry matches the
specified string.
--committer Only show commits in which the committer entry matches
the specified string.
--grep Only show commits with a commit message containing the
string
-S Only show commits adding or removing code matching the
string



*********************************
Look at what changed in a particular commit

git show <SHA>

git show --format=oneline HEAD
git show --format=oneline HEAD^^

etc

git ls-tree master
git show SHA  ///use a folder
-shows tree as it is a folder

*************************************
Comparing commits

Compare directory in the commit
Comparison is over time, or between branches. Use diff

git diff --satged or --cached // diff between repository and staging

So do a git log --onleline to get SHAs
git diff cdae0ed

returns all differences between commit at that time and WD

Can compare any 2 commits:
git diff cf324d4..d87722
just one particular file:
git diff cf324d4..d87722 myfile.txt

Can pass in any tree-ish
git diff de3233..HEAD
git diff HEAD^^
etc, see help for modifiers etc

git diff --stat --summary <SHA>..HEAD

git diff --b  etc    // b is ignore whitespace
git diff --w  etc    // b is ignore all space changes

*******************************
*******************************
BRANCHES

A branch in Git is a lightweight movable pointer to a commit. Default branchname is 'master'. As you make commits you are given a master branch that points to the last commit made. master is not special in any way, its just the default name.

when you create a new branch, you are creating a new pointer at the same commit that you are currently on:
git branch test // if you were on master, this points to the same commit as master
git branch doesnt switch you to the new branch, you also need to checkout. This moves HEAD to point to the new branch.

git log --decorate shows you where the branch pointers are pointing.
git log --oneline --decorate

// history of commits, showing where branch pointers are and how your history hass diverged


git log --oneline --decorate --graph --all

still one WD, git can context switch branches very quickly

commits to master branch. create new branch. commit changes in wd to new branch. can switch between both branches
commits contine in master
Need to merge branch in
HEAD pointer points to last commit in master. Once we have committed in a branch, HEAD points there. When we switch branches, HEAD changes
*************************
Create branch

see branches in repo:
git branch
asterisk indicates current branch

cat .git/HEAD
shows head ref

ls -la .git/refs/heads
gets dir listening of all branches
cat .git/refs/heads/master
- gets SHA of current branch commit

Create new branch:
git branch branch_name
git branch   //shows branches

ls -la .git/refs/heads
---can see new branch
cat .git/refs/heads/branch_name
returns same SHA as current branch head.. once we make commits, values change

****************************
Switch branch

git checkout branch_name

make a change in a file in branch
git commit -a -m "blah"    // commits and adds automatic

switch back to master branch:
git checkout master
--we're now back on master branch

***************************
Create/checkout branch at same time:

git branch
git log --oneline

create new branch and checkout
git checkout -b another_branch    // -b = create branch and switch to it

This makes a branch off whatever the current branch is, not necessarily master, so it is easy to branch of a feature branch

Remember we use checkout to discard changes in WD with the -- flag to show we are not trying to checkout a branch. 

makes changes, commit.
git log --oneline
git checkout <branch>   // change to another branch, do git log, and we dont see the previous commit as its on another branch

try 
git log --graph --onleline --decorate --all
gives commits with tips of each branch

::get practice switching branches etc, 


************************
WD needs to be clean to switch branches

git warns that local changes will be overwritten by checkout

option 1- scrap the changes by checking out the file again:
option 2- commit changes to current branch
option 3- stash changes (see later)

WD doesnt need to be completely clean, just enough to avoid conflicts
Now we can switch branches-
git checkout <branch>

***************************
Comparing branches

a branch is a tree-ish
git diff master..<branch_name>

the order of the 2 branches doesnt matter so much, it just changes which is the A (-) or B (+) entries in the log print

git diff --color-words <branch-1>..<branch-2>
- shows diff all on one line, can look clearer

A tree-ish is also the anccetors so can do:
git diff --color-words <branch-1>..<branch-2>^
// compares to previous commit to the HEAD of branch-2

See if one branch completely contains another, eg  one has been merged back in

git branch --merged
// shows all branches that are completely included in this branch

master
<branch-1>
*<branch-2>
-- current is branch-2, (*), so branch master and branch-1 are also in 2, so we could actually delete branch-1

if we switch to branch 1 and do git branch --merged:
master
*<branch-1>
So now we see branch-1 doesnt contain branch-2, so a merge would be needed if that's what we wanted

switch to master and do same check:
git checkout master
git branch --merged


************************
RENAME BRANCHES

git branch -m <branch-name> <new-name>   /// m to move, rename is move, like unix

************************
DELETING BRANCHES

git checkout master // switch to main
git branch branch-to-kill // create branch
git branch -d branch-to-kill // deletes the branch

Git has some checks to stop a dangerous delete, so
git branch branch-to-kill // create branch
git checkout branch-to-kill // this time check it out
git branch -d branch-to-kill // get error, cant delete current branch

IF you have checked stuff in to a feature branch and then try to delete it, error message: branch is not fully merged, use capital D
git branch -D <name>

**************************

MERGING
Simple example:
1. checkout the branch you wish to merge into
eg git checkout master  // if merging intp master branch
2. git merge <name_of_branch_to_merge_in>

if we now do a git log the top commit should have the comments of the last one of the branch that was merged in

If we do diff master..merged_branch or git branch --merged it will show that the branches are now the same, so we could delete the branch if we want.

Merging can get messy so be sure to do a git merge with a clean WD, with no uncommitted changes.

*****************************
Fast Forward merges vs Real/True merges
Previous example was super simple because nothing had changed in the master branch. As a result git did a 'fast forward' merge (it outputs this in the feedback during the merge operation)

When merging it looks from the tip of the branch being merged all the way back to its beginnings, and sees if at any point it has the HEAD of the master branch (or branch being merged into?). If it sees the HEAD it can just do a fastfwd merge, which simply involves moving the commits in the branch after the HEAD over to the master branch.

In this case we get the exact same SHA that was at the head of the feature branch as in the master.

Fast fwd options:
git merge --no-ff branch_name   // no fastfwd. Forces a merge commit with commit msg, youd do this for some sort of documentation rather than a quiet F.fwd merge.

git merge --ff-only branch_name // do the merge only if you can FFwd, otherwise abort.

TRUE MERGE
so master has had commit(s) 
git log --oneline -3  // see whats changed
git log <branch_name> --oneline -3  // compare the 2, there are commits after the common point on both branches

So when we do:
git merge <feature_branch>    // git pops up and asks for txt message

Now the feedback says 'Merge made by the recursive strategy'
If we do a git log on the master branch the last commit will be the merge.

**********************************
MERGE CONFLICTS

Sometimes git cant figure out how to merge. Changes to same lines in file(s)

if conflict detected merge fails and reports which files are conflicted, and branch changes to <branch | MERGING>

open file and edit... markes the different lines with =======  >>>>>>>


**********************************
RESOLVING MERGE CONFLICTS
3 options:
1. abort merge
2. resolve manually
3. use merge tool

Abort
git merge --abort
git status
git log --oneline

Resolve by hand
save file
git add <dodgy file>
git commit   // no need for commit message when in middle of conflict 

git branch --merged // displays merged
git log --graph --oneline --all --decorate
	-draws graphical rep of merging/branching

Merge Tools

git mergetool --tool=

get info:
git mergetool --tool-help

*************************
Mitigating Against Merge Conflicts

-keep lines short
-keep commits small & focussed
-beware stray edits to whitespace
-merge often, so do a merge, we go on making edits in the feature branch, merge again, etc
-track changes to master- so as changes happen in master, bring them into the branch:
 in other words merge master into the branch. This is known as tracking

**************************
STASHING

4th area, seperate from staging, repo, wd.
We dont put in commits but they are similar... no SHA associated

Stash is somewhere we store changes temporarily without needing to commit to repo

So make changes in a branch and then try to switch back to master:
get a file overwrite warning, please commit or stash

git stash save "changed file blah blah" // message doesnt have to be as good as a commit message

git log --oneline -3
//after it stashes your changes it runs git reset --hard
untracked files can be included with include-untracked option

********************
Viewing stash

git stash list

stash@{0} // this is how to refer to an item in stash
Stash is accessible even if switch branches so stash list reports <stash_ref>: On <branch_ref>: msg   // branch_ref is branch where stash was made from

so git stash list is same from whatever branch you are on

So if you have changes on one branch and realise you want to make the commit on another branch you can stash the changes, and pull them back out of the stash.

To get more info about a stash:
git stash show <stash_ref>  // eg stash@{0}

this gives a diff stat by default.
Use -p option to show it as a patch.. patch is section of code you can apply to modify/change things
git stash show -p stash@{0}

this gives us the more typical diff log to see whats changed

*****************************
Unstashing stash

-- like a merge there's the danger of conflicts

2 commands to pull changes from stash

git stash pop	// removes from stash as well - opposite of git stash save
git stash apply // leaves copy in stash

If we dont provide a stash ref it assumes we want the first one.
eg to get 3rd one:
git stash pop stash@{2}
if we accidentally popped it on the wrong branch we can undo the stash pop with
git stash save "change msg"
then checkout to the branch we want to be on..
git stash apply  // stash applied to current branch
git stash status
... still got stash because we used apply

*******************************
DELETE items in stash

git stash drop stash@{ref}

To drop everything in stash, ie several stash patches:

git stash clear

*******************************
REMOTE REPOSITORIES

push to server.

when we push, git makes another branch locally, origin/master (origin can be changed), and it references the remote branch, and tries to stay in sync. It doesnt duplicate resources, it uses pointers.

edit master, push, changes go to server and origin/master branch.
To get changes off the server we pull with a fetch, which syncs up origin/master with remote, but does not bring it into master branch, until we do a merge. When we have done the fetch the origin/master is pointing to the latest SHA, but master still points to the previous one, until we merge, which will be a fast forward.

Note, it is possible for someone to make a commit onto server while we are making a local commit, so after a fetch origin master will reflect the changes on the server but our master has diverged and looks like a seperate branch. We need to merge to bring them together...
process:
1 commit locally
2 fetch from remote server
3 merge any of your new work into what you got from server
4 and push the result up to the server

****************************

create github repos, based on existing

go to root of project..

git remote
//gives list of remotes it knows, works like git branch
// lists shortnames of each remote handle. If youve cloned a repo, it likely says 'origin'- default name

git remote add <alias> <url>
get url from github, default alias=origin

git remote add origin https://github...

git remote // tells u what remote there is
git remote -v // show fetch/push urls, usually the same, but u may be getting from  aread only one and pushing to a diferent one

git remote show [remote-name] // get more info about the remote.. eg: git remote show origin

cat .git/config shows what git does when fetching

git remote rename <oldname> <new_name> // change the alias

git remote rm <alias>  /// remove a remote

*************************************
PUSH COMMITS TO REMOTE

we push a branch to corresponding branch

git push -u origin <branch_name>  
// -u option sets up master branch to track origin
// without -u it pushes our code up, without creating a tracking branch

cat .git/config
we should see a master branch with remote = origin

ls -la .git/refs/remotes
cat .git/refs/remotes/origin/master
-- shows sha commit of tip of origin master

git branch -r // show remote branches
git branch -a // shows remote and local

****************************************
CLONE REMOTE REPO

git clone <path> {local_directory_name}
//local dir optional
// takes default branch off github, can specify a branch with -b

*************************************
TRACKING REMOTE BRANCHES

regularly pull updates from main

When we use git clone, we automatically get a tracking branch
cat .git/config to see if its a tracking branch

To makea nontracking branch into a tracking one:
git config branch.<branch_name>.remote origin
git config branch.<branch_name>.merge refs/head/master

or
git branch --set-upstream <branch_name> origin/<branch_name>
// same as -u option

With a tracking branch, we dont have to write git push origin master, we can simply do git push.

********************************
FETCH CHANGE
can practice by checking out 2 seperate repos from the same github one. make changes in one, push, fetch in the other...

git fetch origin
or just git fetch if there's only one remote repo
shows log of commit numbers and any new branch

now git branch shows just the one branch still, but git branch -r shows the remotes, and any new branch that was fetched

git log --oneline -5 master
we can see the fetch didnt bring the update into master branch, just into origin/master

So our WD isnt affected by a fetch, so fetch often.
- Always fetch before you work
- Fetch before you push
- Fetch often... its  not destructive so no reason not to 

So, we need to merge a fetch into our master branch...

********************************
MERGING IN FETCHED CHANGES

Git manages origin/master.. we cant check it out but its eactly like any other branch
So we need to merge with it just like any branch. Could be a F Fwd merge, or a commit merge, and there may be a conflict, just as usual.

in the project folder on master branch
git branch -a
git diff master..origin/master

git merge origin/master

git log --oneline -3 // see the merge is there

Before we merge, we should have done a fetch first, so our origin/master gets the latest from rmote.

************************************
PULL

-- git pull is a shortcut tht is equivalent to git fetch and git merge

v convenient, but for beginners it obscures what is happenning under the hood. If something goes wrong, it can be confusing.. so start with fetch and merge

***************************************
Checkout Remote Branch

git branch -r
//see remote branches

we cant check out a remote branch as git is in control, but we can create a branch from it:

we usually do something like:
git branch <name> HEAD
but to make a branch froma remote branch:
git branch <name> origin/<name>

git branch
cat .git/config  // we can see it will track the remote branch
if we delete the branch
git branch -d <name>
We could have also done:
git checkout -b <name> origin/<name>

*****************************************
 
Check out remote branches
git fetch
git checkout <name>
 
***************************************
Push to updated remote branch
 
If we try to push after other new stuff has been committed, git
doesnt know what to do. It never tried to merge on a push. We need
to fetch, merge locally, and push.
 
git fetch
git merge origin/master   // creates new merge commit. may be
conflicts
git push //
 
***************************************
 
DELETE REMOTE BRANCH
 
git branch -a
//see remotes
git push origin :<branch>              // colon means delete
 
this colon thing seems unintuitive.. But when we do a push we go
git push origin <branch_name>
which is shorthand for
git push origin <local_branch_name>:<remote_branch_name>
if we leave it off it assumes bot hare the same.
So
git push origin :<branch> //is saying push nothing to remote branch
 
another way:
git push origin --delete <branch>
 
****************************************

 

*****************************************
TAGS
- create tags at important points in history, eg release points
git tag // lists avail tags in alpha order

git tag -l 'v1.8.5*'  // just the 1.8.5 series

The above are lightweight tags, v simple pointers. Annotated tags are objects in git Db and contain much more data. use -a option for annotated...
git tag -a 'my version blah blah'

To see commit data for the tag, use
git show <tag_name>

lightweight tags are created by ommitting -a, -s, -m options. Git show on a lightweight tag just shows the commit info

You can tag a commit after the event:-

git tag -a <tag_name> commit_checksum

**********************
Sharing tags

git push doesnt transfer tags to remote servers. 

git push origin <tagname>

Easier: if you have several tags, use --tags with push:

git push origin --tags

************************
checkout a tag - can't be done, but you can create a new branch at a specific tag:
git checkout -b <branchname> <tagname>
// note that when you do a commit here, your branch will differ to your tag a it moves forward with the changes


************************
Aliases

git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

git ci // commit

Make usnatging files a bit more usable:
git config --global alias.unstage 'reset HEAD --'

so:
git unstage <filename>

Adding useful stuff:
git config --global alias.last 'log -1 HEAD'
git last // shows last commit

Use apostrophe to run external command:
git config --global alias.visual "!gitk"
