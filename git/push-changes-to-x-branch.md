# 💭 Pushing Changes to an `x` Branch

1. Ensure and check in that you are in the right branch

    ```bash
    dev@dev:~/your-project$ git branch --show-current
    ```

    <br />

    > 🔔 **REMEMBER**
    >
    > This will show you the current branch you are in.

2. Stage all the changes in the current directory and its subdirectories

    ```bash
    dev@dev:~/your-project$ git add .
    ```

3. Stage modifications and deletions to already tracked files

    ```bash
    dev@dev:~/your-project$ git add -u
    ```

4. Create a new commit containing the current contents of the index and the given log message describing the changes

    ```bash
    dev@dev:~/your-project$ git commit -m "YOUR_COMMIT_MESSAGE"
    ```

    <br />

    > **ℹ️ NOTE**
    >
    > Remember to replace `YOUR_COMMIT_MESSAGE` with your commit message.

5. Send the committed changes to the current branch

    ```bash
    dev@dev:~/your-project$ git push
    ```