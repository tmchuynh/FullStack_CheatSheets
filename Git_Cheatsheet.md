# What is a Git?
A Git repository is a virtual storage of your project. It allows you to save versions of your code, which you can access when needed. 

## Initializing a new repository: git init
### Versioning an existing project with a new git repository
This example assumes you already have an existing project folder that you would like to create a repo within. You'll first `cd` to the root project folder and then execute the `git init` command.
```
cd /path/to/your/existing/code 
git init
```

Initializing a new repository: git init
---------------------------------------

To create a new repo, you'll use the `git init` command. `git init` is a one-time command you use during the initial setup of a new repo. Executing this command will create a new `.git` subdirectory in your current working directory. This will also create a new main branch. 

### Versioning an existing project with a new git repository

This example assumes you already have an existing project folder that you would like to create a repo within. You'll first `cd` to the root project folder and then execute the `git init` command.

    cd /path/to/your/existing/code 
    git init


Pointing `git init` to an existing project directory will execute the same initialization setup as mentioned above, but scoped to that project directory.

    git init <project directory>


Cloning an existing repository: git clone
-----------------------------------------

If a project has already been set up in a central repository, the clone command is the most common way for users to obtain a local development clone. Like `git init`, cloning is generally a one-time operation. Once a developer has obtained a working copy, all [version control](https://bitbucket.org/product/version-control-software) operations are managed through their local repository.

    git clone <repo url>


`git clone` is used to create a copy or clone of remote repositories. You pass `git clone` a repository URL. Git supports a few different network protocols and corresponding URL formats. In this example, we'll be using the Git SSH protocol. Git SSH URLs follow a template of: `git@HOSTNAME:USERNAME/REPONAME.git`

An example Git SSH URL would be: `git@bitbucket.org:rhyolight/javascript-data-store.git` where the template values match:

*   `HOSTNAME: bitbucket.org`
*   `USERNAME: rhyolight`
*   `REPONAME: javascript-data-store`

When executed, the latest version of the remote repo files on the main branch will be pulled down and added to a new folder. The new folder will be named after the REPONAME in this case `javascript-data-store`. The folder will contain the full history of the remote repository and a newly created main branch.


Saving changes to the repository: git add and git commit
--------------------------------------------------------

Now that you have a repository cloned or initialized, you can commit file version changes to it. The following example assumes you have set up a project at `/path/to/project`. The steps being taken in this example are:

*   Change directories to `/path/to/project`
*   Create a new file `CommitTest.txt` with contents ~"test content for git tutorial"~
*   git add `CommitTest.txt` to the repository staging area
*   Create a new commit with a message describing what work was done in the commit

    cd /path/to/project 
    echo "test content for git tutorial" >> CommitTest.txt 
    git add CommitTest.txt 
    git commit -m "added CommitTest.txt to the repo"


After executing this example, your repo will now have `CommitTest.txt` added to the history and will track future updates to the file.


Repo-to-repo collaboration: git push
------------------------------------

It’s important to understand that Git’s idea of a “working copy” is very different from the working copy you get by checking out source code from an SVN repository. 

This makes collaborating with Git fundamentally different than with SVN. Whereas SVN depends on the relationship between the central repository and the working copy, Git’s collaboration model is based on repository-to-repository interaction. Instead of checking a working copy into SVN’s central repository, you push or pull commits from one repository to another.

Of course, there’s nothing stopping you from giving certain Git repos special meaning. For example, by simply designating one Git repo as the “central” repository, it’s possible to replicate a centralized workflow using Git. This is accomplished through conventions rather than being hardwired into the VCS itself.

Configuration & set up: git config
----------------------------------

Once you have a remote repo setup, you will need to add a remote repo url to your local `git config`, and set an upstream branch for your local branches. The `git remote` command offers such utility.

    git remote add <remote_name> <remote_repo_url>


This command will map remote repository at to a ref in your local repo under . Once you have mapped the remote repo you can push local branches to it.

    git push -u <remote_name> <local_branch_name>


This command will push the local repo branch under `< local_branch_name >` to the remote repo at `< remote_name >`.


In addition to configuring a remote repo URL, you may also need to set global Git configuration options such as username, or email. The `git config` command lets you configure your Git installation (or an individual repository) from the command line. 

Git stores configuration options in three separate files, which lets you scope options to individual repositories (local), user (Global), or the entire system (system):

*   Local: `/.git/config` – Repository-specific settings.
*   Global: `/.gitconfig` – User-specific settings. This is where options set with the --global flag are stored.
*   System: `$(prefix)/etc/gitconfig` – System-wide settings.


The first thing you’ll want to do after installing Git is tell it your name/email and customize some of the default settings. A typical initial configuration might look something like the following:

Tell Git who you are `git config`

    git --global user.name "John Smith" git config --global user.email john@example.com


Select your favorite text editor

    git config --global core.editor vim



This will produce the `~ /.gitconfig` file from the previous section. 

## Stashing your work
The `git stash` command takes your uncommitted changes (both staged and unstaged), saves them away for later use, and then reverts them from your working copy. For example:

```bash
$ git status
On branch main
Changes to be committed:

    new file:   style.css

Changes not staged for commit:

    modified:   index.html

$ git stash
Saved working directory and index state WIP on main: 5002d47 our new homepage
HEAD is now at 5002d47 our new homepage

$ git status
On branch main
nothing to commit, working tree clean
```

### Re-applying your stashed changes
You can reapply previously stashed changes with `git stash pop`:
```bash
$ git status
On branch main
nothing to commit, working tree clean
$ git stash pop
On branch main
Changes to be committed:

    new file:   style.css

Changes not staged for commit:

    modified:   index.html

Dropped refs/stash@{0} (32b3aa1d185dfe6d57b3c3cc3b32cbf3e380cc6a)
```

Popping your stash removes the changes from your stash and reapplies them to your working copy.

Alternatively, you can reapply the changes to your working copy and keep them in your stash with `git stash apply`:

```bash
$ git stash apply
On branch main
Changes to be committed:

    new file:   style.css

Changes not staged for commit:

    modified:   index.html
```

## .gitignore

Ignored files are usually build artifacts and machine generated files that can be derived from your repository source or should otherwise not be committed. Some common examples are:

- dependency caches, such as the contents of `/node_modules` or `/packages`
- compiled code, such as `.o`, `.pyc`, and `.class` files
- build output directories, such as `/bin`, `/out`, or `/target`
- files generated at runtime, such as `.log`, `.lock`, or `.tmp`
- hidden system files, such as `.DS_Store` or `Thumbs.db`
- personal IDE config files, such as `.idea/workspace.xml`

Pattern

Example matches

Explanation\*

`**/logs`

`logs/debug.log`  
`logs/monday/foo.bar`  
`build/logs/debug.log`

You can prepend a pattern with a double asterisk to match directories anywhere in the repository.

`**/logs/debug.log`

`logs/debug.log`  
`build/logs/debug.log`  
_but not_  
`logs/build/debug.log`

You can also use a double asterisk to match files based on their name and the name of their parent directory.

`*.log`

`debug.log`  
`foo.log`  
`.log`  
`logs/debug.log`

An asterisk is a wildcard that matches zero or more characters.

`*.log`  
`!important.log`

`debug.log`  
`trace.log`  
_but not_  
`important.log`  
`logs/important.log`

Prepending an exclamation mark to a pattern negates it. If a file matches a pattern, but _also_ matches a negating pattern defined later in the file, it will not be ignored.

`*.log`  
`!important/*.log`  
`trace.*`

`debug.log`  
`important/trace.log`  
_but not_  
`important/debug.log`

Patterns defined after a negating pattern will re-ignore any previously negated files.

`/debug.log`

`debug.log`  
_but not_  
`logs/debug.log`

Prepending a slash matches files only in the repository root.

`debug.log`

`debug.log`  
`logs/debug.log`

By default, patterns match files in any directory

`debug?.log`

`debug0.log`  
`debugg.log`  
_but not_  
`debug10.log`

A question mark matches exactly one character.

`debug[0-9].log`

`debug0.log`  
`debug1.log`  
_but not_  
`debug10.log`

Square brackets can also be used to match a single character from a specified range.

`debug[01].log`

`debug0.log`  
`debug1.log`  
_but not_  
`debug2.log`  
`debug01.log`

Square brackets match a single character form the specified set.

`debug[!01].log`

`debug2.log`  
_but not_  
`debug0.log`  
`debug1.log`  
`debug01.log`

An exclamation mark can be used to match any character except one from the specified set.

`debug[a-z].log`

`debuga.log`  
`debugb.log`  
_but not_  
`debug1.log`

Ranges can be numeric or alphabetic.

`logs`

`logs`  
`logs/debug.log`  
`logs/latest/foo.bar`  
`build/logs`  
`build/logs/debug.log`

If you don't append a slash, the pattern will match both files and the contents of directories with that name. In the example matches on the left, both directories and files named _logs_ are ignored

logs/

`logs/debug.log`  
`logs/latest/foo.bar`  
`build/logs/foo.bar`  
`build/logs/latest/debug.log`

Appending a slash indicates the pattern is a directory. The entire contents of any directory in the repository matching that name – including all of its files and subdirectories – will be ignored

`logs/`  
`!logs/important.log`

`logs/debug.log`  
`logs/important.log`

Wait a minute! Shouldn't `logs/important.log` be negated in the example on the left  
  
Nope! Due to a performance-related quirk in Git, you _can not_ negate a file that is ignored due to a pattern matching a directory

`logs/**/debug.log`

`logs/debug.log`  
`logs/monday/debug.log`  
`logs/monday/pm/debug.log`

A double asterisk matches zero or more directories.

`logs/*day/debug.log`

`logs/monday/debug.log`  
`logs/tuesday/debug.log`  
_but not_  
`logs/latest/debug.log`

Wildcards can be used in directory names as well.

`logs/debug.log`

`logs/debug.log`  
_but not_  
`debug.log`  
`build/logs/debug.log`

Patterns specifying a file in a particular directory are relative to the repository root. (You can prepend a slash if you like, but it doesn't do anything special.)

## Configuration & set up: git config
Once you have a remote repo setup, you will need to add a remote repo url to your local git config, and set an upstream branch for your local branches. The git remote command offers such utility.

```
git remote add <remote_name> <remote_repo_url>
```

This command will map remote repository at  to a ref in your local repo under . Once you have mapped the remote repo you can push local branches to it.

```
git push -u <remote_name> <local_branch_name>
```

Define the author name to be used for all commits in the current repository. Typically, you’ll want to use the --global flag to set configuration options for the current user.

Tell Git who you are git config
```
git --global user.name "John Smith" git config --global user.email john@example.com
```

Select your favorite text editor
```
git config --global core.editor vim
```

git log
-------

The `git log` command displays committed snapshots. It lets you list the project history, filter it, and search for specific changes. While `git status` lets you inspect the working directory and the staging area, `git log` only operates on the committed history.

Log output can be customized in several ways, from simply filtering commits to displaying them in a completely user-defined format. Some of the most common configurations of `git log` are presented below.

### Usage
```
git log
```
Display the entire commit history using the default formatting. If the output takes up more than one screen, you can use `Space` to scroll and `q` to exit.
```
git log -n <limit>
```

Limit the number of commits by . For example, `git log -n 3` will display only 3 commits.

Condense each commit to a single line. This is useful for getting a high-level overview of the project history.
```
git log --oneline
```

```
git log --stat
```

Along with the ordinary `git log` information, include which files were altered and the relative number of lines that were added or deleted from each of them.

    git log -p


Display the patch representing each commit. This shows the full diff of each commit, which is the most detailed view you can have of your project history.

    git log --author="<pattern>"


Search for commits by a particular author. The argument can be a plain string or a regular expression.

    git log --grep="<pattern>"


Search for commits with a commit message that matches , which can be a plain string or a regular expression.

    git log <since>..<until>


Show only commits that occur between `< since >` and `< until >`. Both arguments can be either a commit ID, a branch name, `HEAD`, or any other kind of [revision reference](http://www.kernel.org/pub/software/scm/git/docs/gitrevisions.html).

    git log <file>


Only display commits that include the specified file. This is an easy way to see the history of a particular file.

    git log --graph --decorate --oneline


A few useful options to consider. The --graph flag that will draw a text based graph of the commits on the left hand side of the commit messages. --decorate adds the names of branches or tags of the commits that are shown. --oneline shows the commit information on a single line making it easier to browse through commits at-a-glance.

The _Usage_ section provides many examples of `git log`, but keep in mind that several options can be combined into a single command:

    git log --author="John Smith" -p hello.py


This will display a full diff of all the changes John Smith has made to the file `hello.py`.

The .. syntax is a very useful tool for comparing branches. The next example displays a brief overview of all the commits that are in `some-feature` that are not in `main`.

    git log --oneline main..some-feature

Finding what is lost: Reviewing old commits
-------------------------------------------

The whole idea behind any version control system is to store “safe” copies of a project so that you never have to worry about irreparably breaking your code base. Once you’ve built up a project history of commits, you can review and revisit any commit in the history. One of the best utilities for reviewing the history of a Git repository is the `git log` command. In the example below, we use `git log` to get a list of the latest commits to a popular open-source graphics library.

    git log --oneline
    e2f9a78fe Replaced FlyControls with OrbitControls
    d35ce0178 Editor: Shortcuts panel Safari support.
    9dbe8d0cf Editor: Sidebar.Controls to Sidebar.Settings.Shortcuts. Clean up.
    05c5288fc Merge pull request #12612 from TyLindberg/editor-controls-panel
    0d8b6e74b Merge pull request #12805 from harto/patch-1
    23b20c22e Merge pull request #12801 from gam0022/improve-raymarching-example-v2
    fe78029f1 Fix typo in documentation
    7ce43c448 Merge pull request #12794 from WestLangley/dev-x
    17452bb93 Merge pull request #12778 from OndrejSpanel/unitTestFixes
    b5c1b5c70 Merge pull request #12799 from dhritzkiv/patch-21
    1b48ff4d2 Updated builds.
    88adbcdf6 WebVRManager: Clean up.
    2720fbb08 Merge pull request #12803 from dmarcos/parentPoseObject
    9ed629301 Check parent of poseObject instead of camera
    219f3eb13 Update GLTFLoader.js
    15f13bb3c Update GLTFLoader.js
    6d9c22a3b Update uniforms only when onWindowResize
    881b25b58 Update ProjectionMatrix on change aspect


Each commit has a unique SHA-1 identifying hash. These IDs are used to travel through the committed timeline and revisit commits. By default, `git log` will only show commits for the currently selected branch. It is entirely possible that the commit you're looking for is on another branch. You can view all commits across all branches by executing `git log --branches=*`. The command `git branch` is used to view and visit other branches. Invoking the command, `git branch -a` will return a list of all known branch names. One of these branch names can then be logged using `git log`.

When you have found a commit reference to the point in history you want to visit, you can utilize the `git checkout` command to visit that commit. `Git checkout` is an easy way to “load” any of these saved snapshots onto your development machine. During the normal course of development, the `HEAD` usually points to `main` or some other local branch, but when you check out a previous commit, `HEAD` no longer points to a branch—it points directly to a commit. This is called a “detached `HEAD`” state, and it can be visualized as the following:

![Git Tutorial: Checking out a previous commit](https://wac-cdn.atlassian.com/dam/jcr:9b234e0d-ee33-4463-ac14-298c9559015d/01%20Checking%20out%20a%20previous%20commit.svg?cdnVersion=1000)

Checking out an old file does not move the `HEAD` pointer. It remains on the same branch and same commit, avoiding a 'detached head' state. You can then commit the old version of the file in a new snapshot as you would any other changes. So, in effect, this usage of `git checkout` on a file, serves as a way to revert back to an old version of an individual file. For more information on these two modes visit the `git checkout` page

Viewing an old revision
-----------------------

This example assumes that you’ve started developing a crazy experiment, but you’re not sure if you want to keep it or not. To help you decide, you want to take a look at the state of the project before you started your experiment. First, you’ll need to find the ID of the revision you want to see.

    git log --oneline


Let’s say your project history looks something like the following:

    b7119f2 Continue doing crazy things
    872fa7e Try something crazy
    a1e8fb5 Make some important changes to hello.txt
    435b61d Create hello.txt
    9773e52 Initial import


You can use `git checkout` to view the “Make some import changes to hello.txt” commit as follows:

    git checkout a1e8fb5


This makes your working directory match the exact state of the `a1e8fb5` commit. You can look at files, compile the project, run tests, and even edit files without worrying about losing the current state of the project. Nothing you do in here will be saved in your repository. To continue developing, you need to get back to the “current” state of your project:

    git checkout main


This assumes that you're developing on the default `main` branch. Once you’re back in the `main` branch, you can use either `git revert` or `git reset` to undo any undesired changes.

Undoing a committed snapshot
----------------------------

There are technically several different strategies to 'undo' a commit. The following examples will assume we have a commit history that looks like:

    git log --oneline
    872fa7e Try something crazy
    a1e8fb5 Make some important changes to hello.txt
    435b61d Create hello.txt
    9773e52 Initial import


We will focus on undoing the `872fa7e Try something crazy` commit. Maybe things got a little too crazy.

How to undo a commit with git checkout
--------------------------------------

Using the `git checkout` command we can checkout the previous commit, `a1e8fb5,` putting the repository in a state before the crazy commit happened. Checking out a specific commit will put the repo in a "detached HEAD" state. This means you are no longer working on any branch. In a detached state, any new commits you make will be orphaned when you change branches back to an established branch. Orphaned commits are up for deletion by Git's garbage collector. The garbage collector runs on a configured interval and permanently destroys orphaned commits. To prevent orphaned commits from being garbage collected, we need to ensure we are on a branch.

From the detached HEAD state, we can execute `git checkout -b new_branch_without_crazy_commit`. This will create a new branch named `new_branch_without_crazy_commit` and switch to that state. The repo is now on a new history timeline in which the `872fa7e` commit no longer exists. At this point, we can continue work on this new branch in which the `872fa7e` commit no longer exists and consider it 'undone'. Unfortunately, if you need the previous branch, maybe it was your `main` branch, this undo strategy is not appropriate. Let's look at some other 'undo' strategies. For more information and examples review our in-depth `git checkout` discussion.

How to undo a public commit with git revert
-------------------------------------------

Let's assume we are back to our original commit history example. The history that includes the `872fa7e` commit. This time let's try a revert 'undo'. If we execute `git revert HEAD`, Git will create a new commit with the inverse of the last commit. This adds a new commit to the current branch history and now makes it look like:

    git log --oneline
    e2f9a78 Revert "Try something crazy"
    872fa7e Try something crazy
    a1e8fb5 Make some important changes to hello.txt
    435b61d Create hello.txt
    9773e52 Initial import


At this point, we have again technically 'undone' the `872fa7e` commit. Although `872fa7e` still exists in the history, the new `e2f9a78` commit is an inverse of the changes in `872fa7e`. Unlike our previous checkout strategy, we can continue using the same branch. This solution is a satisfactory undo. This is the ideal 'undo' method for working with public shared repositories. If you have requirements of keeping a curated and minimal Git history this strategy may not be satisfactory.

How to undo a commit with git reset
-----------------------------------

For this undo strategy we will continue with our working example. `git reset` is an extensive command with multiple uses and functions. If we invoke `git reset --hard a1e8fb5` the commit history is reset to that specified commit. Examining the commit history with `git log` will now look like:

    git log --oneline
    a1e8fb5 Make some important changes to hello.txt
    435b61d Create hello.txt
    9773e52 Initial import


The log output shows the `e2f9a78` and `872fa7e` commits no longer exist in the commit history. At this point, we can continue working and creating new commits as if the 'crazy' commits never happened. This method of undoing changes has the cleanest effect on history. Doing a reset is great for local changes however it adds complications when working with a shared remote repository. If we have a shared remote repository that has the `872fa7e` commit pushed to it, and we try to `git push` a branch where we have reset the history, Git will catch this and throw an error. Git will assume that the branch being pushed is not up to date because of it's missing commits. In these scenarios, `git revert` should be the preferred undo method.

Undoing the last commit
-----------------------

In the previous section, we discussed different strategies for undoing commits. These strategies are all applicable to the most recent commit as well. In some cases though, you might not need to remove or reset the last commit. Maybe it was just made prematurely. In this case you can amend the most recent commit. Once you have made more changes in the working directory and staged them for commit by using `git add`, you can execute `git commit --amend`. This will have Git open the configured system editor and let you modify the last commit message. The new changes will be added to the amended commit.

Undoing uncommitted changes
---------------------------

Before changes are committed to the repository history, they live in the staging index and the working directory. You may need to undo changes within these two areas. The staging index and working directory are internal Git state management mechanisms. For more detailed information on how these two mechanisms operate, visit the `git reset` page which explores them in depth.

The working directory
---------------------

The working directory is generally in sync with the local file system. To undo changes in the working directory you can edit files like you normally would using your favorite editor. Git has a couple utilities that help manage the working directory. There is the `git clean` command which is a convenience utility for undoing changes to the working directory. Additionally, `git reset` can be invoked with the `--mixed` or `--hard` options and will apply a reset to the working directory.

The staging index
-----------------

The `git add` command is used to add changes to the staging index. `Git reset` is primarily used to undo the staging index changes. A `--mixed` reset will move any pending changes from the staging index back into the working directory.

Undoing public changes
----------------------

When working on a team with remote repositories, extra consideration needs to be made when undoing changes. `Git reset` should generally be considered a 'local' undo method. A reset should be used when undoing changes to a private branch. This safely isolates the removal of commits from other branches that may be in use by other developers. Problems arise when a reset is executed on a shared branch and that branch is then pushed remotely with `git push`. Git will block the push in this scenario complaining that the branch being pushed is out of date from the remote branch as it is missing commits.

The preferred method of undoing shared history is `git revert`. A revert is safer than a reset because it will not remove any commits from a shared history. A revert will retain the commits you want to undo and create a new commit that inverts the undesired commit. This method is safer for shared remote collaboration because a remote developer can then pull the branch and receive the new revert commit which undoes the undesired commit.

Summary
-------

We covered many high-level strategies for undoing things in Git. It's important to remember that there is more than one way to 'undo' in a Git project. Most of the discussion on this page touched on deeper topics that are more thoroughly explained on pages specific to the relevant Git commands. The most commonly used 'undo' tools are `git checkout, git revert`, and `git reset`. Some key points to remember are:

*   Once changes have been committed they are generally permanent
*   Use `git checkout` to move around and review the commit history
*   `git revert` is the best tool for undoing shared public changes
*   `git reset` is best used for undoing local private changes

In addition to the primary undo commands, we took a look at other Git utilities: `git log` for finding lost commits `git clean` for undoing uncommitted changes `git add` for modifying the staging index.

Each of these commands has its own in-depth documentation. To learn more about a specific command mentioned here, visit the corresponding links.