## downloads

Git Bash: https://git-scm.com/downloads
Git LFS: https://git-lfs.github.com/
#git

## workflow
![workflow](/_res/Git/GitHub-Flow-1.png)

**copy files .gitignore * & .gitattributes**
`git lfs install`
`git add .`
`git status`
`git commit -am "initial commit"`
`git log`
`git branch -m master main`

## [GitHub.com]
Create repo
git remote add origin [repo url.git]
git push -u origin main

[Tip: if any errors/conflicts, clone the remote repo and use the .git folder to replace the local one]

## [GitHub Actions]
mkdir .github
mkdir .github\workflows
#gitactions #action 

1. Get activation yml file from https://game.ci/docs/github/activation
touch activation.yml and paste contents inside

2. push your changes to the remote and go to the repo > Actions > Acquire activation file and [Run workflow]
3. In Repo page, Click on [Acquire activation file] and download the  the .alf file from Artifacts at the bottom of page
4. upload the .alf file to: https://license.unity3d.com/manual and unzip the .ulf and 
5. copy the .ulf file contents to GitHub repo: Settings > Secrets > Actions > New Repo Secret

Name: [UNITY_LICENSE]
Value: open the alf file and copy the 
<Binding Key="1" Value="576562626572264761624c65526f7578" />

5. inside workflows dir, Create the main.yml and paste contents from complete example
https://game.ci/docs/github/builder

Rename branch:
`git branch -m oldName newName`
`git push origin newName:master`
`git push origin -u newName`
`git push origin -- delete oldName`
`git checkout -newName`
