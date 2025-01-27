# workflows and events

In this section, you learned about the various events that can be used to trigger workflows in GitHub Actions. These events are diverse, with many being repository-related, such as push events, pull requests, or issue-related actions. Additionally, there are more general events, like scheduled workflows or workflows triggered manually. Many events also support activity types, enabling you to narrow down the specific scenarios that should trigger a workflow. For instance, you can configure a workflow to run only when a pull request is opened, edited, or closed, depending on your requirements.

When dealing with pull requests, it is important to note that workflows will not automatically trigger for pull requests from forked repositories. In such cases, manual approval is required for the first pull request from an unknown contributor. Subsequent workflow executions for pull requests from the same contributor will then run automatically.

Once a workflow is running, it can be canceled automatically if a job or step fails, or it can be canceled manually if you anticipate that it will not succeed or for other reasons. If you want to prevent a workflow from running before it is triggered, you can include special instructions in your commit messages to skip the workflow.

Event filters can also be applied to event definitions, offering a way to control when workflows are executed. These filters, similar to activity types, allow you to set conditions for running workflows, such as targeting specific branches or monitoring changes to certain files in push or pull request events. This level of control ensures that workflows run only when necessary, optimizing your CI/CD processes.
