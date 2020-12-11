---

---

[Readme (home)](./README.md)

## One Detailed Protocol for Beginner Maintainers: 

- **Primary rule: Never "commit" a change to the `gh-pages` branch. *Ever***.
	- Maintainers only change the `gh-pages` branch by merging PRs.
- **Secondary rule: Create an "Issue" before sending a substantial PR**
	- Exceptions are for typos
- **Tertiary rule: Never merge your own PR**
	- No exceptions!
		- *Except* if no other maintainers are participating

### Steps:

- Open GitBash or a Terminal window (This assumes "Git" is installed)
	- If creating a new repo, create the directory using `mkdir`, then `cd` into the directory, and run `git init` to initialize.
- New maintainers should clone the repo you will be maintaining to your local computer
	- Example: `git clone https://github.com/datacarpentry/wrangling-genomics`
	- Generic example: `git clone https://github.com/<organization>/<repo-name>`
- Check your remote connection
	- `git remote -v`
		- Should see something like:
```
git remote -v
origin  https://github.com/datacarpentry/wrangling-genomics.git (fetch)
origin  https://github.com/datacarpentry/wrangling-genomics.git (push)
```	
- Check which branches exist:

```
$ git branch
* gh-pages
 my-branch
```
- Delete any branches except `gh-pages`
	- Example: `git branch -D my-branch`
	- Generic example: `git branch -D <not-gh-pages>`
		
- Always update your existing local repo.
	- This may not be needed if you just now cloned the repo, but do it anyway
	- Example: `git fetch origin`
		- Note that `origin` is the "alias" for your **remote** repo at the carpentries, and was set up automagically when you cloned the repo.
			- (There is no generic example. Type `git fetch origin`)
- Connect to **your personal** github repo with the same name
	- Example:
		- `git remote add upstream https://github.com/hoytpr/wrangling-genomics`
	- Generic example:
		- `git remote add upstream https://github.com/<your-github-username>/<repo-name>`
	- NOTE: You are connecting to your **remote** repo, AND you are assigning  `upstream` as the "alias" for that repo.
- Check your remote connections again
	- git remote -v
		- Should see something like:

```
git remote -v
origin  https://github.com/datacarpentry/wrangling-genomics.git (fetch)
origin  https://github.com/datacarpentry/wrangling-genomics.git (push)
upstream        https://github.com/hoytpr/wrangling-genomics.git (fetch)
upstream        https://github.com/hoytpr/wrangling-genomics.git (push)
```	
   - Generic example:

```
git remote -v
origin  https://github.com/<organization>/<repo-name>.git (fetch)
origin  https://github.com/<organization>/<repo-name>.git (push)
upstream        https://github.com/<your-github-username>/<repo-name>.git (fetch)
upstream        https://github.com/<your-github-username>/<repo-name>.git (push)
```	

<!--

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

-->

- You MUST have a `gh-pages branch` (and probably do). 
- If you don't have a `gh-pages` branch, or a different branch for making changes (in this example it's named 'My-branch') you can create one. For example:

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

- NOTE: The asterisk indicates you are on the new `My-branch` branch and are NOT on the `gh-pages` branch.

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
> # submit pull request to The Carpentries using "My-branch" branch using your **online** GUI
> ```

### Remember:
- ALWAYS make changes to the Carpentries lesson using the command-line on **your local** repo using a new branch like `My-branch` (not `gh-pages`)
- Push changes to same `My-branch` (not `gh-pages`) of **your online** GitHub repo (your ***remote*** repo)
- Send in PR (to Carpentries lesson site) from **your remote** Github repo (the GUI) using `My-branch` or similar named branch (but **NOT** `gh-pages`)
- When accepted by another maintainer and merged, **delete your** My-branch (at the Carpentries lesson site)
- (Deleting your branch which will show as an option on the PRs tab of the Carpentries lesson after merging.)

### To clean things up
- Go back to local command line

The remote setup should still look like this:

```
$ git remote -v
origin  https://github.com/datacarpentry/wrangling-genomics (fetch)
origin  https://github.com/datacarpentry/wrangling-genomics (push)
upstream        https://github.com/hoytpr/wrangling-genomics (fetch)
upstream        https://github.com/hoytpr/wrangling-genomics (push)
```

Clean up the local repo by deleting ALL other branches (FYI: To delete a branch; first switch to a different branch, then use `-D`)

```
$ git checkout gh-pages
Switched to branch 'gh-pages'
$ git branch -D My-branch
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

Then make a **new** local branch for future changes
```
git checkout -b new-branch
Switched to a new branch 'new-branch'
git push --set-upstream origin new-branch
```

Your **new** branch will be exactly like the updated `gh-pages` branch. 
NOW everything should be up to date and ready for new changes. 

## A Common Problem <a name="wrong-ahead"></a>

- If your upstream `gh-pages` branch has some commits generated online using the Github interface, you will need to eliminate them! This is ***important*** for beginner maintainers. Commits to `gh-pages` in your Github repo always stay "ahead" of the Carpentries repo you are maintaining.

- This can also happen if you are working on your local repo and **commit** a change while in the `gh-pages` branch. 

- It is possible that the number of contributors that use the `gh-pages` branch, rather than 
submit PRs using a different branch, is because they are not aware of, or skilled in, this process, 
it becomes frustrating, and in the rush to contribute, 
they simply use the `gh-pages` of their online Github repos.
	- As a *compounding* problem, their first PR is met with a rejection by the maintainers 
	for not using the `gh-pages` branch! 

- Teaching about `patches` might help.

## A Simple Fix when `gh-pages` are "ahead" 

- First make your LOCAL repo up to date as specified above, then run this command: 

`$ git push -f upstream gh-pages`

NOW your upstream GitHub will be ***forced*** to look like your local repo and
will be up to date with SWC gh-pages


[Readme (home)](./README.md)
