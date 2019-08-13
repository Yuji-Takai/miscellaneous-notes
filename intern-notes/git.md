## Steps to make a Pull Request
1. Checkout master by clicking "Check out from Version Control" after opening IntelliJ, select "git", paste the url for the git repository, and click "Clone"
    - **[NOTE]** if one has never logged into their GitHub account on the IntelliJ, he/she must log in by clicking "Log in to GitHub"
2. Make changes to the files as desired 
3. Go to the terminal on IntelliJ
4. `git branch`: Check which branch one is currently in (should be master if checked out master) as well as all the other local branches
5. `git pull`: Make sure that the local branch is up-to-date
6. `git checkout -B BRANCH_NAME`: Create a new local branch 
    - **[NOTE]** Name the branch so that the team could understand what this branch is for
7. `git status`: Check which files were modified, added, deleted
8. `git add FILE_NAME or PATTERN`: Stage the modified files that will be pushed to remote
9. `git status`: Make sure all of the desired files are staged
10. `git commit -m "MESSAGE"`: Always address what the commit is for
11. `git push --set-upstream origin BRANCH_NAME`: Push the commit and create a remote branch with the same name as the local branch (need to do `--set-upstream origin BRANCH_NAME` for the first push)

## Amending commits
If one committed some files but one forgot to do a small change, to clean the imports, or to follow the check style, instead of making a brand new commit, amend to the last commit

1. Make modifications
2. `git log`: check the previous commits, make sure the commit that one wants to amend to is the last one
3. `git status`: check the modified files
4. `git add --FILES`: stage all the modified files to be committed
5. amend
    - `git commit --amend "MESSAGE"`: when one wants to edit the commit message
    - `git commit --amend --no-edit`: when no need to edit the commit message
6. `git status`: Should see "Your branch and 'origin/BRANCH_NAME' have diverged, and have 1 and 1 different commits each, respectively"
7. `git push -f`: Force the override of the last commit

## Resolving Merge conflicts
1. `git pull BRANCH_TO_MERGE_TO`: Pull the branch to merge to → should see merge conflict showing up
2. On IntelliJ, right click on the project folder and select Git and then Resolve Conflict
3. Double click on the file to resolve the merge conflict
4. Click on arrows to import the changes from one's local file and from the conflicting file

## Checking out remote branch 
1. `git branch -r`: see all of the available remote branches
2. `git checkout -b NEW_LOCAL_BRANCH_NAME origin/REMOTE_BRANCH_NAME`: checks out the remote branch into the new local branch

## Cloning Tag
1. `git clone REPOSITORY -b TAG_NAME`

## Deleting local branch
1. `git checkout master`: checkout master
2. `git branch -D BRANCH_NAME_TO_DELETE` 

## Creating a new repository 
After one has created a new repository on GitHub, created a local project, and wants to make that the master of the repository:
1. `git init`: initialize a new local repository
2. `git add *`: stage all of the files to be committed 
3. `git commit -m "MESSAGE"`
4. `git remote add origin URI`: set the remote repository uri
5. `git push -u origin master`: push code to the master branch of the remote repository (sets current local branch to the remote master branch)

## Squashing Commits
After a pull request is approved, one should squash all the commits related to the PR into one commit so that it will look clean when the merge is done.
1. Rebasing
    - `git rebase -i HEAD~n`: rebasing from the first commit to nth commit
    - `git rebase -i 87ceb7~n`: rebasing n commits from the specified commit
2. vim view will show up
3. hit i to apply changes → instead of pick (`p`), insert squash (`s`) for all the commits that will be squashed
4. hit esc after finish editing and :wq
5. hit i to apply changes to the commit messages → delete all of them and just write one that summarizes all of the commits
6. hit esc after finish editing 
7. `git push -f`

## Always Maven Check!
1. ./mvnw clean verify