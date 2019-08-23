---
title: "Beginner Git for Maintainers"
teaching: 10
exercises: 0
questions:
- "How to send a properly formatted PR to a Carpentries Lesson?"
---

[< Readme (home)](https://hoytpr.github.io/git_beginner/)

### Disclaimer:
- There are [other ways](https://hoytpr.github.io/git_beginner/Other-options) to do this, but a simple, consistent way needs to be spelled out. This is my suggestion. Some say that "revert" rather than "reset" is a better option but this is simple and works. If you know a better option, that's GREAT! Let's please put all good options here then on the Carpentries site.

### objectives: ###
- Describe exact commands to sync a personal online (GitHub copy of) a Carpentries Workshop.
- Describe exact commands to sync a local copy of a Carpentries Workshop.
- Describe the exact process of making changes on a branch locally
- Describe the exact process of pushing changes to your personal online repo
- Describe how the personal online repo is used to create a Pull Request to The Carpentries
- Describe how to **reset** your local and Github repos following a PR

### keypoints: ###
- Need a perfectly **clean** online GitHub repo to make a clean PR
- Updates to lessons occur, making your online lessons *behind* the official repo
- Cannot update your GitHub repo from the Carpentries without creating an update **history**
- Update histories can be huge, making them unmanageable by maintainers

**NOTE: We aren't teaching GitHub Desktop, so this won't cover any possible GitHub Desktop solutions**

### How does GitHub REALLY work? (The newbie perspective)

![The GIT Triangle](img/Git-triangle.png)
**The GIT Triangle** (above) shows the three basic areas you'll be working


* GitHub Online are your repositories on [GitHub](https://github.com/)
* Carpentries represents The Carpentries lesson sites, or other sites where you contribute
* Local Repos are kept on your personal computer, and accessed through a command prompt

### Basic flow ###

**There is an order to this process**
- The arrows are numbered to help guide you through a simple PR
- Notice that this diagram uses arrows that are **one way only**. This is intentional and (IMHO) accurately represents the process
- EDIT NOTE: The terms *origin* and *upstream* should be reversed according to convention. But they are okay for this lesson.

## Using branches (More arrows) ##
![The information flow 1](img/Picture3.png)

**SHOWN ABOVE** is how each basic area is divided into parts called branches
Most of you know this, but this just illustrates which branches are being used when you are pushing and pulling.  

## Steps ##
- Create a perfect copy of a Carpentries lesson (*e.g.* just downloaded or cloned). See detailed instructions for [users](One-detailed-protocol-for-general-beginners) or [maintainers](One-detailed-protocol-for-beginner-maintainers)

1. Checkout (create) a new branch (My-branch) and make changes to the lesson on this branch
2. Push your changes to the same branch on your Github (upstream) repo
3. Create a Pull-request using the changed (My-branch) branch by pointing the PR to the Carpentries lesson.
4. Once your changes are approved, they will be merged to the gh-pages branch at The Carpentries
5. To keep everything up-to-date, Pull the new merged changes to your gh-pages on your local repo
6. Then Push your up-to-date gh-pages branch to your upstream gh-pages branch. 

**NOTE:**
- There is a reset step we will discuss later
- Once the PR is merged on the Carpentries site, delete your My-branch locally and on GitHub

**Now, you can make sure your repo is up to date with the Carpentries
as described below and go ahead and make another new branch
for your next contributions (if any)**

## Rationale for these details

During active lesson updates, it is common for **_your online_** GitHub repos to fall behind 
commits at the Carpentries. Updating your site creates a HISTORY of the changes which 
are included with your well-intentioned Pull Request. Included history is a nuisance
There may be an automated way for these to be synced, but it's mysterious (to me).

After several attempts, I found myself having to simply **delete and 
recreate** my online AND my local repos (by forking and cloning) before making clean changes 
on a branch, to offer as a PR to the Carpentries maintainer.  Being a maintainer 
myself, it was common for multiple "other" action histories (adding files, changing files, 
correcting mistakes, etc.) to be included in PRs.

By asking how to easily create a clean Github repo for a PR on the maintainers Slack channel,  

Maxim Belkin gave a helpful reply: (Edited for continuity)

> Use branches for pull requests:
> 
> ```
> git checkout -f gh-pages
> git fetch origin
> git reset --hard origin/gh-pages
> git branch My-branch
> git checkout My-branch
> # make changes
> git add -u
> git commit -m ...
> git push <your-fork>
> # submit pull request using "My-branch" branch
> git checkout gh-pages
> ```
>
> then, if you need to make changes to your pull request:
> 
> ```
> git checkout My-branch
> # make more changes
> git commit -am "message" # same as `git add -u` followed by `git commit -m`
> git push <your fork> # (<your fork> should be "your repo/My-branch", or sometimes just "git push" works)
> git checkout gh-pages
> ```
> 
> `git pull` is needed only to update the main branch (`gh-pages`) or when you've done changes to your 
> "My-branch" branch on GitHub via web interface and would like to pull them to your computer <== **this is important!**
> 

*Maxim's advice worked!* (at least for my local repos)
*But remote GitHub Repos being ahead or behind the Carpentries repos was still a problem*

## The following is a description of why and what is confusing for a beginner using GitHub ##

## Common misconceptions of GitHub Management:

1. One sees something that is an issue
2. One notices their online repo is behind the Carpentries repo by two (or more or less) commits
3. **Instinct** is to update your online GitHub repo by just pulling changes from Carpentries, then merging them.
4. **This doesn't work well** because your online pull update from the Carpentries now shows up as a part of your online repo (part of it's "history")

**Specific example:** Looking at a comparison between repos, one Carpentries update pulled to an online GitHub repo included 2,048 additions and 1,617 deletions from earlier. This would then be sent, along with your *one* change, back to the maintainers! 

### Bottom line: You can't use the GitHub online GUI to get rid of the "history" of your repos. 
- History commits (pulls, pushes) will become part of your lesson contributions to the Carpentries. 

**Question:** How to get your online repo up to date without any unneeded history?

**Answer:** You **must** use the local repos, and the command line. 
- BONUS: Once this becomes easy, you can move changes onto your remote GitHub repo where they render properly. 
- CAUTION: Getting Ruby installed to run Jekyll for local rendering of the page can be HARD, but gives better error messages

### One detailed protocol for beginners: 

- (Find a cloning-forking-downloading-creating-importing-and-their-differences page if needed)
- Open GitBash or a Terminal whether updating an existing repo locally, or are cloning a new repo locally
- Run: `git init` inside the repo (folder/directory)
- Setup the remote "origin" to a Carpentries repo, for example the "wrangling-genomics"

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

- Make sure you have a "gh-pages" branch and a branch for making changes and PRs

```
$ git branch
* gh-pages
  My-branch
```

- You MUST have a gh-pages branch (and probably do). 
- If you don't have a gh-pages branch, or a branch for making changes (in my example it's named 'My-branch') you can create one. For example:

```
$ git checkout -b My-branch
Switched to a new branch 'My-branch'
```

- Make *sure* you made the branch correctly.

```
$ git branch
  gh-pages
* My-branch
```

_*NOW*_ Use Max Belkims advice:

> ```
> git checkout -f gh-pages
> git fetch origin
> git reset --hard origin/gh-pages
> git branch
>    gh-pages
>    My-branch
> git checkout My-branch
> # make changes
> git add -u
> git commit -m ...       <== add commit message here
> git push upstream https://github.com/hoytpr/wrangling-genomics
> # submit pull request to The Carpentries using "My-branch" branch      <== using your online GUI
> git checkout gh-pages
> ```

### Remember:
- Make changes to the Carpentries lesson using the command-line on **your local** repo using My-branch (not gh-pages)
- Push changes to same My-branch (not gh-pages) of your online GitHub repo
- Send in PR (to SWC lesson site) from **remote** Github repo (GUI) using My-branch (not gh-pages)
- When accepted by maintainer and merged, **delete your** My-branch (at SWC lesson site)
- (Deleting your branch which will show as an option on the PRs tab of the Carpentries lesson after merging.)

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

Clean up the local repo (FYI: To delete a branch; first switch to a different branch, then use `-d`)

```
$ git checkout gh-pages
Switched to branch 'gh-pages'
$ git branch -d My-branch
Deleted branch My-branch (was 3a0742a).
```

**Fetch** the new Carpentries lesson and **reset** your gh-pages branch

```
git checkout gh-pages
git fetch origin
git reset --hard origin/gh-pages
```

Then push the new lesson up to your online Github repo

`git push upstream`

Then make a new local branch for future changes
```
git checkout -b new-branch
Switched to a new branch 'new-branch'
git push --set-upstream origin new-branch
```

NOW everything should be up to date and ready for any new changes. 

### A Simple Fix for a Common Problem

- If your upstream has some stupid commits you want to eliminate
because they were pulls or pushes after you made changes to SWC.
- First make your LOCAL repo up to date as specified above, then run this command: 

`$ git push -f upstream gh-pages`

NOW your upstream GitHub will be ***forced*** to look like your local repo and
will be up to date with SWC gh-pages

`____________________________________________`

### Same Common Problem, with a more details for beginners

If I changed something on my remote GitHub page, how can I reset it?

### There are confusing messages within Git that you might see:

Example 

1. Your local "shell-novice" repo is perfect, because you just fetched and reset it from The Carpentries.

```
git checkout gh-pages
git fetch origin
git reset --hard origin/gh-pages
```

2. Now make your GitHub upstream repo perfect, but when you try you get an error: 

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

3. Checking your status makes things even more confusing: 

```
$ git status
On branch gh-pages
Your branch is up to date with 'origin/gh-pages'.     <== Ahah! This is only true for ORIGIN!

nothing to commit, working tree clean                 <== But it sure looks like everything is fine!

```

4. When you check your GitHub online repo it reports something like:

	`"This is 2 commits behind swcarpentry:gh-pages"`         <==  This is true!!!!!

5. To understand all this you must be on the same branch locally AND upstream. Then use `git diff`. 

For Example:

```
$ git checkout My-branch
Switched to branch 'My-branch'
$ git diff upstream/My-branch
                                          <== nothing returned = no differences
$ git checkout gh-pages
Switched to branch 'gh-pages'

$ git diff upstream/gh-pages
diff --git a/aio.md b/aio.md              <== now you see differences!
index 6d93852..a91fb0f 100644
--- a/aio.md
+++ b/aio.md
@@ -1,6 +1,6 @@
 ---
-layout: page
-permalink: /aio/
+layout: page
+title: Aio
 ---
 <script>
   window.onload = function() {
diff --git a/reference.md b/reference.md
index 87a20cc..6260be6 100644
--- a/reference.md
+++ b/reference.md
@@ -1,6 +1,6 @@
 ---
 layout: reference
-permalink: /reference/
+title: Reference
 ---

 ## Glossary
diff --git a/setup.md b/setup.md
index e5c3cf4..bc92b52 100644
--- a/setup.md
+++ b/setup.md
@@ -1,7 +1,6 @@
 ---
 layout: page
 title: Setup
-permalink: /setup/
 ---

 This lesson requires a working spreadsheet program. If you don't have a spreadsheet program already, you can use LibreOffice. It's a free, open source spreadsheet program.
```

6. To fix differences in your upstream gh-pages branch you can FORCE (`-f`) the push upstream to that branch.

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

- Then go to the GitHub online repo and refresh the screen. It'll say:

	`"This branch is even with swcarpentry:gh-pages."`

YAY! (Fin)
[< Readme (home)](https://hoytpr.github.io/git_beginner/)
