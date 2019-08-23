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

## Update everything using branches
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

**NOTES:**
- Never do a "commit" to the gh-pages. ***Ever***.
- There is a reset step we will discuss later
- Once the PR is merged on the Carpentries site, delete your My-branch locally and on GitHub

**Now, you can make sure your repo is up to date with the Carpentries
as described below and go ahead and make another new branch
for your next contributions (if any)**

## Rationale for these details or TL;DR

During active lesson updates, it is common for **_your online_** GitHub repos to fall behind 
commits at the Carpentries. Updating your site incorrectly creates a HISTORY of the changes which 
are included with your well-intentioned Pull Request. Included history is a nuisance
There may be an automated way for these to be synced, but it's mysterious (to me).

After several attempts (like... two years), I found myself having to simply **delete and 
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
> **Use `git fetch` instead almost ALWAYS**.

*Maxim's advice worked!* (at least for my local repos)
*But remote GitHub Repos being "ahead" or behind the Carpentries repos was still a problem*. 

## Here's why:

*I was trying to update my `gh-pages` with pull requests. Or, I was updating my local repos, then updating my online (github) repos, after doing a "commit" on the `gh-pages`. It was so sad!*

*If only I had heard (or probably remembered) someone telling me: "Never use commits on the gh-pages! Ever!"*

***So for beginners: "You do not need to, and probably should not ever, do a commit or a PR on the gh-pages branch of a repo, when you are a contributor or maintainer"***

## The following is a description of why and what is confusing for a beginner using GitHub

### Common (or easy) misconceptions of GitHub Management:

1. One sees something that is an issue
2. One notices their online repo is behind the Carpentries repo by two (or more or less) commits
3. **Instinct** is to update your online GitHub repo `gh-pages` by just pulling changes from Carpentries, then merging them. 
4. **Instinct** is to use `gh-pages` so that the site will render correctly from your repo.
5. **This doesn't work well** because your online PR update from the Carpentries now shows up as a commit to your online repo (part of it's "history"). You will always be "one commit ahead" of the Carpentries repo.
6. *Any future PR from your repo will include the gh-pages update as a commit*.

**Specific example:** Looking at a comparison between repos, one Carpentries update pulled to an online GitHub repo included 2,048 additions and 1,617 deletions from earlier. This would then be sent, along with your *one* change, back to the maintainers! 

**Frustration:** One can't easily use the GitHub online GUI to fix the commit "history" of your repos `gh-pages` branch. 
- A history of any commits (pulls, pushes) to the `gh-pages` will become part of your lesson contributions to the Carpentries. 

**Problem:** How does one get their online repo (specifically the gh-pages) up to date without unneeded commit history?

**Disconnect** Experienced GIT-ers ***KNOW*** you don't have to keep your contributor site remote online repo's gh-pages branch updated ***at all***. (Deep breath) But if you WANT to, it's best done ONLY through your local repos (command line) and *never using a "commit" to the gh-pages branch*. 

**Answer:** You **must** use the local repos, and the `fetch` at command line. 
- BONUS: Once this becomes easy, you can move changes onto your remote GitHub repo where they render properly. 
- CAUTION: Getting Ruby installed to run Jekyll for local rendering of the page can be HARD, but gives better error messages (Thanks to Ethan for demonstrating this)

There are two detailed protocols:

1. [For beginner contributors](One-detailed-protocol-for-beginners)

2. [For beginner maintainers](One-detailed-protocol-for-beginner-maintainers)

After going through those protocols, and after everything is up to date and ready for any new changes, come back here for some insights into other potentially counfusing aspects of Github (and some solutions in shorter format). 

### A Simple Fix for a Common Problem

- If your upstream has some stupid commits you want to eliminate
because they were pulls or pushes after you made changes to SWC.
- First make your LOCAL repo up to date as specified above, then run this command: 

`$ git push -f upstream gh-pages`

NOW your upstream GitHub will be ***forced*** to look like your local repo and
will be up to date with SWC gh-pages

`____________________________________________`

### Same problem: Deciphering messages for beginners

Example:

1. Your local "shell-novice" repo is ***perfect***, because you just fetched and reset it from The Carpentries.

```
git checkout gh-pages
git fetch origin
git reset --hard origin/gh-pages
```

2. Now you want to make your GitHub upstream repo perfect, so it will render properly. But when you try you get an error: 

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

3. Checking your status might make things even more confusing: 

```
$ git status
On branch gh-pages
Your branch is up to date with 'origin/gh-pages'.     <== Ahah! This is only true for "origin"!

nothing to commit, working tree clean                 <== But it sure looks like everything is fine!

```

4. When you open your browser and check your GitHub online repo it reports something like:

	`"This is 2 commits behind swcarpentry:gh-pages"`         <==  This is true!!!!!

5. To decipher where the problem is change branches locally and compare to upstream (not origin) using `git diff`. 

For Example: (NOTE this is only an example)

```
$ git checkout My-branch
Switched to branch 'My-branch'
$ git diff upstream/My-branch
                                   <== nothing is returned = no differences! So now you change to gh-pages branch
$ git checkout gh-pages
Switched to branch 'gh-pages'

$ git diff upstream/gh-pages
diff --git a/aio.md b/aio.md		<== now you see differences!
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
<snip></snip> etc.
```

6. To ***fix differences*** in your upstream gh-pages branch you can FORCE (`-f`) the push upstream to that branch. This obliterates differences (good or bad) between your local repos, and your Github (online) repos. 

# Just make sure you don't do this to a Carpentries repo if you are a maintainer with write privledges!!!

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
