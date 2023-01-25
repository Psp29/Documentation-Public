
* ## 3.2 Creating a Local Repository (Empty)

#### *git init /path/to/directory*
Initializes a git repository, either by creating new directory or by adding the git repository files to an existing directory.

$ `git init`
![](Assets/Pasted%20image%2020230104185653%201.png)

$ `ls -la`
![](Assets/Pasted%20image%2020230110100645.png)

$ `ls -a .git`
![](Assets/Pasted%20image%2020230110100815.png)


#### *git init --bare /path/to/directory*
Initializes a bare git repository, for larger projects, contains no working area.

$ `git init --bare <path/directory>`
![](Assets/Pasted%20image%2020230110102151.png)

$ `ls -a <path/dir>`
![](Assets/Pasted%20image%2020230110102318.png)

* ## 3.3 Basic Config of GIT

$ `git config --global user.name "<user-name>"`

$ `git config --global user.email "<user-email>"`

$ `git config --lis`
![](Assets/Pasted%20image%2020230110103824.png)

$ `git config --global core.editor </path/to/editor>`
![](Assets/Pasted%20image%2020230110104256.png)


* ## 3.4 Adding Files to a project

$ `ls`
![](Assets/Pasted%20image%2020230110105145.png)

$ `git add <fileName>`

$ `cat .git/index`
![](Assets/Pasted%20image%2020230110105319.png)

$ `git status`
![](Assets/Pasted%20image%2020230110105507.png)

$ `touch test1`

$ `git add test1`

$ `git status`
![](Assets/Pasted%20image%2020230110105853.png)

$ `git rm test1`

$ `git rm -f test1`

$ `git status`
![](Assets/Pasted%20image%2020230110105938.png)


* ## 3.5 Status of the Repo/Project

$ `git status -s`

$ `echo "This is README file" >> README.md`

$ `git status -s`
![](Assets/Pasted%20image%2020230110110331.png)

$ `git add README.md`

$ `git status -s`
![](Assets/Pasted%20image%2020230110112510.png)

$ `git status -v`
![](Assets/Pasted%20image%2020230110112714.png)


* ## 3.6 Commiting to Git

$ `git commit`
![](Assets/Pasted%20image%2020230110113221.png)

$ `git log`
![](Assets/Pasted%20image%2020230110113439.png)

$ `git add <file or dir name>`

$ `git commit -m "commit message"`
![](Assets/Pasted%20image%2020230110113859.png)

$ `git rm --cached <file>`

*Note: use '-r' if the target is a directory.*
![](Assets/Pasted%20image%2020230110114214.png)

*Note:* `git rm` DOES NOT delete actual file from directory. It just removes it from project and stage the removal for commit.
$ `ls`
![](Assets/Pasted%20image%2020230110114420.png)

$ `git status -s`
![](Assets/Pasted%20image%2020230110115202.png)

$ `git commit`
![](Assets/Pasted%20image%2020230110120825.png)

$ `git add <dir>`

$ `git commit -a -m "Commit Message"`

*Note: '-a' is used to commit Modified files into staging area.*
![](Assets/Pasted%20image%2020230110121112.png)


* ## 3.7 Ignoring certain file types

$ `touch .gitignore`

$ `git add .gitignore`

$ `git status`
![](Assets/Pasted%20image%2020230110121550.png)

$ `git commit -m "Commit Message"`
![](Assets/Pasted%20image%2020230110121741.png)

![](Assets/Pasted%20image%2020230110122207.png)

$ `echo ".obsidian/*" >> .gitignore`

$ `git -a -m "commit message"
![](Assets/Pasted%20image%2020230110123029.png)
![](Assets/Pasted%20image%2020230110123110.png)


# Lab 1

## Learning Objectives
* Global Configuration File for Git Created
	$ `ls -la /home/<username> | grep ".gitconfig"`
![](Assets/Pasted%20image%2020230110125010.png)

* Global Settings Have Been Applied
 $ `cat /home/<username>/.gitignore`
![](Assets/Pasted%20image%2020230110124722.png)

# Lab 2

## Learning Objectives
* Create Directory for the Repository
	$ `mkdir <new/path>`
![](Assets/Screenshot%20from%202023-01-10%2012-56-35.png)

* Initialize the New Git Repository
	$ `git init`
![](Assets/Pasted%20image%2020230110145132.png)

	$ `ls -la`
![](Assets/Pasted%20image%2020230110145218.png)

* Repository README File Created
	$ `echo "Sapmple-Repo" >> README.md`

	$ `ls -la`
![](Assets/Pasted%20image%2020230110145431.png)

* README File Added and Committed to the Repository
	$ `git add README.md`
	
	$ `git commit -m "Initial commit"`
![](Assets/Pasted%20image%2020230110150833.png)
	$ `git status`
![](Assets/Pasted%20image%2020230110153000.png)

