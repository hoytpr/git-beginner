[< Go Back](https://hoytpr.github.io/git_beginner/Maintaining-for-beginners-narrative1)

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
