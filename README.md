# Check PR Approval on New Commits 

This Action checks if new commits have been pushed after a pull request (PR) approval and dismisses the approval if necessary or notifies the team. It helps maintain code review integrity by ensuring that approvals aren't stale when new commits are introduced.

## Features
- Automatically checks if commits are pushed after a PR approval.
- Notifies the team if a new commit is pushed and invalidates the approval.
- Can be customized to take different actions (like dismissing approvals) based on commit timestamps.

## Inputs

| Name           | Description                                                  | Required | Default |
|----------------|--------------------------------------------------------------|----------|---------|
| `github-token` | The GitHub token for authentication in API requests.          | `true`   |         |

## Outputs
This action currently doesn't return any outputs but performs in-place validation and optional notifications.

## Example Usage
To use this action in your workflows, you need to set it up as follows:

```yaml
name: Check PR Approval on New Commits

on:
  push:
    branches:
      - '**'

jobs:
  check-approval:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the repository
      - name: Checkout code
        uses: actions/checkout@v3

      # Step 2: Use the Check PR Approval Action
      - name: Check PR approval status
        uses: the-robots/check-pr-approval@latest
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}

      # Optional Step 3: Notify if approval is invalid
      - name: Notify team about invalid approval
        if: failure()
        run: |
          echo "New commit detected after approval. Notify the team!"
          # Add your notification logic here, such as posting a comment on the PR or sending a Slack message.
```

## Inputs Breakdown

github-token: This token is required to interact with GitHub's APIs. Use ${{ secrets.GITHUB_TOKEN }} in your workflow for GitHub's automatically generated token.

## Example Workflow

Here’s an example of how this action can be used in a workflow to automatically validate approvals on a push event:

```yaml
name: PR Approval Validator

on:
  push:
    branches:
      - '**'

jobs:
  check-pr-approval:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Check PR Approval
        uses: the-robots/check-pr-approval@latest
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}

      - name: Notify on Stale Approval
        if: failure()
        run: echo "The approval is stale, notify the team or take further action."
```

## How It Works:

- Trigger: This workflow listens for any new push events to any branch in the repository.
- Checkout: The repository is checked out to access the relevant files and commits.
- Check Approval: The custom action is used to compare the latest commit timestamp against the approval timestamp.
- Optional Notification: If the approval is found to be stale, the action can notify the team.

## License
This project is licensed under the MIT License. See LICENSE for details.

## Support
For any issues, feel free to open an issue in this repository. We’ll try help with any problems or questions you may have.


