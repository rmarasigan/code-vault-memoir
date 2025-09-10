# 💭 CodePipeline Invalid Approval Token

### Problem
A pipeline execution is paused at a Manual Approval stage for a long time (e.g., months). When trying to approve or reject via AWS CodePipeline Console, you get:

```
InvalidApprovalTokenException
Approval token 'xxxxx' either does not exist or is not the correct token for the action or X stage.
```

#### But why?
* Every manual approval generates a unique token tied to that execution.
* Once used or after long inactivity, the token becomes invalid.
* Trying to reuse it causes `InvalidApprovalTokenException`.

Historically, the timeout for manual approval actions was fixed at 7 days and not configurable. If no action was taken within this period, the pipeline would fail, and the approval token would become invalid, leading to this exception if someone later tried to use it.

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

---

> [!NOTE]
> When stopping a pipeline execution, you have two options: "Stop and wait" or "Stop and abandon".
> * **Stop and wait**: Ensures that all in-progress actions are completed before the execution stops.
> * **Stop and abandon**: Halts the execution immediately, marking in-progress actions as abandoned.
>
> During either process, the pipeline execution enters a "Stopping" state, and once complete, transitions to a "Stopped" state. The next execution, particularly the most recent one, may automatically proceed into the stage once the current execution is stopped and the stage unlocks.
>
> When a specific AWS CodePipeline execution is **stopped and abandoned**, the next execution behaves as follows:
> * The next execution proceeds normally, provided it is the most recent one in the queue and stage transitions are enabled.
> * CodePipeline has a built-in "supersede" behavior: if multiple executions are queued at a stage (especially one with manual approval or a blocking action), enabling the transition allows only the latest (newest) execution to proceed, while older pending executions are automatically discarded or skipped.
> * Abandoning an execution immediately releases any stage locks, allowing the next execution to move forward as soon as conditions (like manual approval) are met.
> * If the next execution is waiting for manual approval, it will remain in a pending state until an approver acts or the approval timeout occurs.


## 📋 Related Articles
* [AWS Code pipeline Manual Approval Timeout](https://stackoverflow.com/questions/63763829/aws-code-pipeline-manual-approval-timeout)
* [Stop a pipeline execution in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-stop.html)
* [CodePipeline Approval Stage Timeout Configuration](https://repost.aws/questions/QU3K43oLPQQxWh-AsJAeoCEg/codepipeline-approval-stage-timeout-configuration)
* [Approve or reject an approval action in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals-approve-or-reject.html)
* [codepipeline-actions: support timeout for manual approval action](https://github.com/aws/aws-cdk/issues/33473)