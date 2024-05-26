# 💭 Merging `x` branch to an `y` branch

`git merge` combines changes from two branches. Its primamry purpose is to integrate changes made in one branch (*source branch*) into another (*target branch*) and share those changes with other developers.

The `master` or `main` branch in Git is a repository's default and primary branch. It usually represents the latest stable version of the project's code.

1. Switch to `master` branch

    ```bash
    dev@dev:~/your-project$ git switch master
    ```

    <br />

    > **ℹ️ NOTE**
    >
    > Ensure you are on the branch you want to merge into.

2. Merge the `y` branch into `master` branch
    
    This will merge the `[branch-name]` to the current branch.

    ```bash
    dev@dev:~/your-project$ git merge [branch-name]
    ```

    <br />

    > **ℹ️ NOTE**
    >
    > Remember to replace `[branch-name]` with your branch name (e.g. `develop`).
    >
    > Example: `git merge develop`

3. Push the local changes to the remote repository

    ```bash
    dev@dev:~/your-project$ git push -u origin master
    ```

## 📋 Related Articles
- [Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- [How to Merge a Git Branch into Master](https://phoenixnap.com/kb/git-merge-branch-into-master)