# **Getting Started with Git**

As with the rest of the onboarding pack, this is not intended to *teach* you Git. Rather it is supposed to serve as a helping hand.

A solid option is to use [GitHub Desktop](https://github.com/apps/desktop).

However, many engineers prefer to use the terminal. If you're one of those, here are some useful actions.

## **Important Actions to Understand in Git**

* **Clone**: Copy an existing Git repository to your local machine.

* **Branch**: Create separate lines of development.
* **Add**: Stage changes to be committed.
* **Commit**: Save changes to the local repository.
* **Push**: Upload local repository content to a remote repository.
* **Pull**: Fetch and merge changes from a remote repository to your local repository.
* **Merge**: Combine changes from different branches.


## **Examples**

1. **Clone a Repository**
To copy an existing Git repository to your local machine:

```git clone https://github.com/your-username/repo-name.git```

2. **Create a New Branch**
To create a new branch for your changes:

```git checkout -b your-new-branch-name```

3. **Add Changes**

```git add . ```


4. **Commit Changes**

To save changes to the local repository with a clear, concise message:

```git commit -m "your commit message"```

5. **Push Changes**
To upload your local repository content to a remote repository:

```git push origin your-new-branch-name```

6. **Pull Changes**

To pull and merge changes from a remote repository to your local repository:

```git pull origin main```

7. Merge Branches
To combine changes from different branches:

The existing branch:
```git checkout main```

Merge with your new branch:

```git merge your-new-branch-name```

