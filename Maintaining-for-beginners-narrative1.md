### Problem to be addressed
- To send a properly formatted PR to a Carpentries Lesson

### Issues
- Need a perfectly "clean" online GitHub repo to make a clean PR
- Updates to lessons occur, making your online lessons "behind" the official repo
- Cannot update your GitHub repo from the Carpentries without creating an update "history"
- Update histories can be huge, making them unmanageable by maintainers

### Key Points
- How to create a clean online repo for making a pull request
- How to reset your local repos and GitHub repos after a PR

### How does GitHub REALLY work?

![The GIT Triangle](img/Git-triangle.png)
**The GIT Triangle** shows the three basic areas you'll be working

![The information flow 1](img/Picture3.png)

![The information flow 1](Picture2.png)

![The information flow 1](Picture1.png)

## Story

During active lesson updates, it is common for _your online_ GitHub repos to fall behind commits at the Carpentries. 
There may be an automated way for these to be synched, but it mysterious.

After several attempts to fix this, I found myself having to simply delete and 
recreate my online repos (by forking or cloning) before I could then make a change 
on a branch, and offer it as a PR to the Carpentries maintainer.  Being a maintainer 
myself, I often saw people include multiple "other" actions (adding files, changing files, 
correcting mistakes, etc.) that are included in PRs.

On the maintainers Slack channel I asked how to easily create a clean Github repo for a PR.

I got this helpful reply from Maxim Belkin:

> Use branches for pull requests:
> 
> ```
> git checkout -f gh-pages
> git fetch origin
> git reset --hard origin/gh-pages
> git branch my-changes
> git checkout my-changes
> # make changes
> git add -u
> git commit -m ...
> git push <your-fork>
> # submit pull request using "my-changes" branch
> git checkout gh-pages
> ```
>
> then, if you need to make changes to your pull request:
> 
> ```
> git checkout my-changes
> # make more changes
> git commit -am "message" # same as `git add -u` followed by `git commit -m`
> git push <your fork> # changes will appear on GitHub just fine
> git checkout gh-pages
> ```
> 
> `git pull` is needed only to update the main branch (`gh-pages`) or when you've done changes to your 
> "my-changes" branch on GitHub via web interface and would like to pull them to your computer 
> 

*Maxim's advice worked!*
*But I was still having problems with remote GitHub Repos already ahead or behind the Carpentries repos*

(We aren't teaching GitHub Desktop, probably for good reasons. 
So forget the GitHub Desktop for now)

## Example of wrong GitHub Management:

1. One sees something that is an issue
2. One notices their online repo is behind the Carpentries repo by two commits
3. Instinct is to update your online GitHub repo by just pulling changes from Carpentries, then merging them.
4. This doesn't work because your updated pull from the Carpentries now shows up as a part of your repo (part of it's "history")

** Specific example: ** Looking at the comparison between repos, one update pulled to your GitHub repo included 2,048 additions and 1,617 deletions from earlier. This would then be sent, along with your *one* change, back to the maintainers! 

This won't work. You need a clean online GitHub repository. 

### Bottom line: You can't use the GUI to get rid of the "history" of your repos, which will be included in your commits.

**Question:** How to get everything up to date?

**Answer:** You must use the local repos, and the command line.

**Problem:** Getting Ruby installed to run Jekyl for local rendering of the page is HARD

**Caveat:** Getting changes on your remote GitHub repo to render properly is EASY

### One detailed protocol: 

- Open GitBash whether updating an existing repo locally, or are cloning a new repo locally
- Run: `git init` inside the repo (folder/directory)
- Setup the remote "origin" (the Carpentries repo, for example the "wrangling-genomics")

`git remote add origin https://github.com/datacarpentry/wrangling-genomics`

- check the remote origin is correct

```
git remote -v
origin  https://github.com/datacarpentry/wrangling-genomics.git (fetch)
origin  https://github.com/datacarpentry/wrangling-genomics.git (push)
```

- Setup the remote "upstream" as **your** remote GitHub repo

`git remote add upstream https://github.com/hoytpr/wrangling-genomics`

- check the remote upstream is correct

```
git remote -v
origin  https://github.com/datacarpentry/wrangling-genomics.git (fetch)
origin  https://github.com/datacarpentry/wrangling-genomics.git (push)
upstream        https://github.com/hoytpr/wrangling-genomics.git (fetch)
upstream        https://github.com/hoytpr/wrangling-genomics.git (push)

```

- Make sure you have a "gh-pages" branch

```
$ git branch
* gh-pages
  test-branch
```

- You MUST have a gh-pages branch (and probably do), and a new branch (in my example it's named 'test-branch') to make changes for the PR.
- If you *don't* have a branch for making changes you can create one. For example:

```
$ git checkout -b test-branch
Switched to a new branch 'test-branch'
```

- Make *sure* you made the branch correctly.

```
$ git branch
  gh-pages
* test-branch
```

_*NOW*_ Use Max Belkims advice:

> ```
> git checkout -f gh-pages
> git fetch origin
> git reset --hard origin/gh-pages
> git branch
>    gh-pages
>    test-branch
> git checkout test-branch
> # make changes
> git add -u
> git commit -m ...
> git push upstream https://github.com/hoytpr/wrangling-genomics
> # submit pull request using "test-branch" branch
> git checkout gh-pages
> ```

### Remember:
- Make changes (to Carpentries lesson) using command-line on local repo using test-branch (not gh-pages)
- Push changes to same test-branch (not gh-pages) of your online GitHub repo
- Send in PR (to SWC) from remote Github repo using test-branch (not gh-pages)
- When accepted by maintainer and merged, *delete* test-branch (at SWC).

## To clean things up
- Go back to local command line

The remote setup should still look like this:

```
$ git remote -v
origin  https://github.com/datacarpentry/wrangling-genomics (fetch)
origin  https://github.com/datacarpentry/wrangling-genomics (push)
upstream        https://github.com/hoytpr/wrangling-genomics (fetch)
upstream        https://github.com/hoytpr/wrangling-genomics (push)
```

Clean up the local repo (FYI: To delete a branch; switch to a different branch, then use `-d`)

```
$ git checkout gh-pages
Switched to branch 'gh-pages'
$ git branch -d test-branch
Deleted branch test-branch (was 3a0742a).
```

Fetch the new Carpentries lesson and reset your gh-pages branch

```
git checkout gh-pages
git fetch origin
git reset --hard origin/gh-pages
```

Then push the new lesson up to your online Github repo, and make a new branch for changes

```
git push upstream
git checkout -b new-branch
Switched to a new branch 'new-branch'
git push --set-upstream origin new-branch
```

NOW everything should be perfect. 

If your upstream has some stupid commits you want to eliminate
because they were pulls or pushes after you made changes to SWC,
Once your LOCAL looks perfect from following above instructions, 

`$ git push -f upstream gh-pages`

NOW your upstream GitHub will be forced to look like your local
thus will be perfect with SWC gh-pages

`____________________________________________`

### Question

If I changed something on my remote GitHub page, how can I reset it?

### Answer is complicated:

1. Your local repo is great, as you just fetched and reset it from SWC.

```
git checkout gh-pages
git fetch origin
git reset --hard origin/gh-pages
```

2. You want to make your GitHub upstream repo perfect also, but: 

```
$ git push upstream
To https://github.com/hoytpr/shell-novice
 ! [rejected]        gh-pages -> gh-pages (fetch first)
error: failed to push some refs to 'https://github.com/hoytpr/shell-novice'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

3. And even more confusing:

```
$ git status
On branch gh-pages
Your branch is up to date with 'origin/gh-pages'.     <== this is only true for ORIGIN!

nothing to commit, working tree clean                 <==  But it looks like everything is fine!

```

4. But your GitHub online repo says something like:

	`"This is 2 commits behind <SWC-repo>"`         <==  This is the truth!!!!!

5. To fix this you can FORCE the push upstream

```
$ git push -f upstream gh-pages
Enumerating objects: 179, done.
Counting objects: 100% (176/176), done.
Delta compression using up to 6 threads
Compressing objects: 100% (58/58), done.
Writing objects: 100% (157/157), 24.52 KiB | 4.09 MiB/s, done.
Total 157 (delta 111), reused 145 (delta 99)
remote: Resolving deltas: 100% (111/111), completed with 15 local objects.
To https://github.com/hoytpr/shell-novice
 + c4ba13b...ff68e20 gh-pages -> gh-pages (forced update)
```

6. Then go to the GitHub online repo and refresh the screen. It'll say:

	`"This branch is even with swcarpentry:gh-pages."`

YAY!

### Other possible solutions found online:

**Example 1**
 
Steps to clear out the history of a local git/github repository

```
git-clearHistory
-- Remove the history from the repo 
rm -rf .git
-- recreate the repos from the current content only
git init
git add .
git commit -m "Initial commit"

-- push to the github remote repos and overwrite history. Might have to `git remote rm origin` if origin exists
git remote add origin git@github.com:<YOUR ACCOUNT>/<YOUR REPOS>.git
git push -u --force origin master
```

`_____________________________________`


**Example2**

Note: This might be problematic with repositories with git submodules.

```
git checkout --orphan newBranch
git add -A  # Add all files and commit them
git commit
git branch -D master  # Deletes the master branch
git branch -m master  # Rename the current branch to master
git push -f origin master  # Force push master branch to github
git gc --aggressive --prune=all     # remove the old files
```

`____________________________________`

**Example3**

"Example1 didn't work but the following worked with more attributes during the push."

```
git init
git add .
git commit -m 'Initial commit' 
git remote rm origin 
git remote add origin [repo_address]
git push --mirror --force
```

Just a small comment :
rm -rf .git <------ use this command from git bash.. NOT dos command prompt

`_________________________________________`


