# ✨ Overview Git & Github ✨

## 👉Git & Github
![alt text](https://www.freecodecamp.org/news/content/images/size/w2000/2021/11/g1117.png) 

## 1. Introduction to Git and GitHub

### ❓ What is Git?
- Git is a distributed version control system. 
- Tracks changes in source code during software development.
- Unlike centralized version control systems, Git allows multiple developers to work on a project simultaneously without interfering with each other's work.​

### ❓ What is GitHub?
- GitHub is a cloud-based hosting service for Git repositories. 
- It provides a web interface to manage Git repositories, track issues, and collaborate with other developers.

## ➡️ Basic Difference between Git & Github
![Difference](https://andersenlab.org/dry-guide/2022-03-09/img/git_v_github.png)

## 2. Basic Git Commands

| **Command** |	**Description** |
|-------------|-----------------|
|  `git init` |	 Initialize a new Git repository |
| `git clone <url>` |	Clone an existing repository |
| `git add <file>`  | 	Stage changes for commit |
| `git commit -m "<msg>"` |	Commit staged changes with a message |
| `git status` |	Check the status of your working directory |
| `git log` |	View commit history |
| `git push`|	Push changes to a remote repository |
| `git pull`|	Fetch and merge changes from a remote repository |

## 3. Understanding Git Workflow
A typical Git workflow involves:​

1. **Cloning** a repository.

2. **Creating** a new branch for your feature or fix.

3. **Making** changes and **committing** them.

4. **Pushing** your branch to the remote repository.

5. **Creating** a Pull Request (PR) for review.

6. **Merging** the PR after approval.​

## 4. Branching and Merging

### ❓What is Branching?
- Branching allows you to work on different versions of a project simultaneously. 
- It's like creating a copy of your project to work on a new feature or fix without affecting the main codebase.

### ➕ Creating and Switching Branches
`git checkout -b new-feature`
- This command creates and switches to a new branch named new-feature.​

### Merging Branches
To merge changes from one branch into another:
```
git checkout main
git merge new-feature
```
- This merges the new-feature branch into the main branch.

![branch merging](https://wac-cdn.atlassian.com/dam/jcr:c6db91c1-1343-4d45-8c93-bdba910b9506/02%20Branch-1%20kopiera.png?cdnVersion=2655)






