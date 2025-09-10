# 💭 CodePipeline Invalid Approval Token

### Problem
A pipeline edecution is paused at a Manual Approval stage for a long time (e.g., months). When trying to approve or reject via AWS CodePipeline Console, you get:

```
InvalidApprovalTokenException
Approval token 'xxxxx' either does not exist or is not the correct token for the action or X stage.
```

#### But why?
* Every manual approval generates a unique token tied to that execution.
* Once used or after long inactivity, the token becomes invalid.
* Trying to reuse it causes `InvalidApprovalTokenException`

### Solution

You cannot reject again with an old token. Instead, stop the stale execution.

1. Log in to the AWS Management Console
2. Go to the **CodePipline** → **Pipelines** → **`<Your-Pipeline>`** → **Execution history**
3. Select the old execution (the commit stuck in approval).
3. Click **Stop execution**.
4. Choose **Stop and Abandon**.
    * This halts the execution immediately.
    * No rollback is performed.
    * Existing resources in the `X` stage remain unchanged.

> [!TIP]
> * The stale execution is marked **Stopped**.
> * Newer executions (with later commits) are unblocked and can progress.
> * When the newer executions reaches `X` Approval, it generates a fresh approval token that can be used normally.