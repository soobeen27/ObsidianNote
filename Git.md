r# **Version using Git and the command line**

## **Working Directory**

`git init`

The working directory is the folder or the directory where you initialize your Git repository

## **Staging Area**

`git add` to push it to the staging area

> **Why there is this intermediate staging area?**

why not just go from working diretory straight to the repository

sometimes you might not want to add all of your files to be tracked or all of your files to be committed, So the staging area is a good place to try and figure out what are the things that you want Git to ignore and what are the thing that you want to be tracked.

## **Local Repository**

`git commit`

Our file is inside our local repository, so that .git, and that version is given a name through the commit message.

So that means that even if we have messed up our file, we can still use the last version that’s under version control and we can use a special command called “git checkout” to revert back or roll back to the last position in our local repository.

## **Remote Repository**

you can have a local repository completely in parallel with a remote repository.

You can also sync them or push things from your local repository to your remote repository.


깃 레포지터리로 사용할 폴더로 이동
- 깃 저장소 생성
	`git init`
- 현재 깃 상황 보기
	`git status a`
- 폴더 안에 있는 모든 파일 깃 등록
	`git add .`

- 깃 커밋
	`git commit -m “커밋 메세지”`
- 깃 로그 보기
	`git log`

“HEAD” this is the position or the current state that we are in

git diff **filename**

전 커밋과 파일 달라진 점 보기

git checkout **filename**

ask to roll back filename to the last version

**GitHub and remote repository**

`git remote add origin **URL of our remote repository on GitHub`

깃허브에 로칼 레포지토리 리모트하기

origin : the remote. origin말고 다른거 써도 되는데 어지간하면 오리진씀

git push -u origin master

it pushed your local repository to the remote repository using

the -u flag or the -u option which basically links up your remote and

your local repositories.

And then we are going to push it towards the remote that is called origin and we are going to push it to branch that’s called master

**master** : simply the default branch or the main branch of all of your commits

**Gitignore**

DS_Store : they are basically a settings file that saves certain thing like how your icons be arranged in a particular project folder

touch .gitignore

깃이그노어 파일 생성

git rm —cached -r .

staging area에서 파일들 지워주기

.gitignore 파일에 한줄에 한 파일씩 파일명 입력해주기

or *.txt 처럼 와일드 카드 이용 가능

git add .

다시 staging area에 넣어주기

[https://github.com/github/gitignore](https://github.com/github/gitignore)

여기서 다양한 언어들의 prebuilt template 확인 가능

**Cloning**

git clone **githubcloneurl**

xcode에서 실행하려면 Bundle Identifier랑 Team변경 해주어야함

**Branching and Merging**

git branch **branchName**

브랜치 생성

git branch

브랜치 리스트 보기 *붙은게 현재 브랜치

git checkout **branchName**

브랜치 스위치

git merge **branchName1 branchName2**

브랜치네임1을 브랜치네임2로 머지함

**Forking and Pull Requests**

Fork

you fork a **remote repository**, then you have **full permissions** to do

if you work within a team and you’re all working on a product, then everybody in the team probably has both read and write permissions on a single remote repository.

they can git clone and work on it locally, and then push it and resolve any sort of conflicts that way. it’s through **forking**and making **pull requests.**

**Git on Xcode**

Discard All Changes

마지막 커밋으로 돌리기

[덮어쓰기 not merge](https://www.notion.so/not-merge-5b4ddd01fd9142718cbcdba4059a94f2?pvs=21)