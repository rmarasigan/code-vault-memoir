# 💭 Delete 'Squash & Merged' Branch Locally

If a Pull Request has been merged (*Squash and Merge*) and the branch has been deleted in GitHub, you can still update your local repository.

1. Fetch the latest changes (e.g. `develop/api-v1`).

    ```bash
    dev@dev:~/your-project$ git fetch --prune
    ```

    This command fetches the latest changes from the remote and removes any local tracking references to the branch (since it was deleted on GitHub).
    You won't lose any of the merged changes, it just removes the reference to the deleted branch.

2. Switch to the main/base branch (the one you merged into, e.g. `main`).

    ```bash
    dev@dev:~/your-project$ git checkout main
    ```

3. Pull the latest changes to ensure your main/base branch is up-to-date.

    ```bash
    dev@dev:~/your-project$ git pull origin main
    ```

4. Delete your local branch (e.g. `develop/api-v1`).

    ```bash
    dev@dev:~/your-project$ git branch -d develop/api-v1
    ```

    If Git complains that it's not fully merged, use the following command:

    ```bash
    dev@dev:~/your-project$ git branch -D develop/api-v1
    ```

<br />

> [!TIP]
>
> The **Squash and Merge** combines multiple commits from a pull request into a single commit. This removes the individual commit history, leaving you only a single commit with a combined message. If your team/organization prefers a clean, simple and readable Git history, **Squash and Merge** is better.

## 📋 Related Articles
* [Squashing your merge commits](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/about-merge-methods-on-github#squashing-your-merge-commits)