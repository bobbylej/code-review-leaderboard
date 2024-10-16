# Code Review Leaderboard Workflow

This workflow is designed to track and summarize code reviews on pull requests within a GitHub repository. It generates a leaderboard based on the number of reviews (approvals and changes requested) by users.

## Workflow Triggers

The workflow is triggered by the following events:
- **Push**: When changes are pushed to the `.github/workflows/code-review-leaderboard.yml` file.
- **Pull Request**: When a pull request is opened.
- **Pull Request Review**: When a pull request review is submitted.

## Jobs

### 1. Code Review Leaderboard

- **Permissions**: Requires `write-all` permissions.
- **Runs on**: `ubuntu-latest`

#### Steps:
1. **Checkout**: Checks out the repository.
2. **Find PR associated with current commit**: Uses `jwalton/gh-find-current-pr@v1` to find the current pull request.
3. **Calculate Last Target Day**: Calculates the last target day based on specified variables.
4. **Count Reviews**: Counts the reviews for pull requests opened or merged since the last target day.
5. **Generate Comment**: Generates a comment summarizing the code review leaderboard.
6. **Post Review Summary Comment**: Posts the generated comment to the pull request.

## Variables

Ensure the following variables are set for this workflow:

- `CODE_REVIEW_LEADERBOARD_START_DAY`: The day of the week to start counting reviews (0 = Sunday, 6 = Saturday). Default is `5` (Friday).
- `CODE_REVIEW_LEADERBOARD_START_HOUR`: The hour of the day to start counting reviews (0-23). Default is `7` (7:00 AM UTC).
- `CODE_REVIEW_LEADERBOARD_START_MINUTE`: The minute of the hour to start counting reviews (0-59). Default is `0`.

## Usage

1. To use this workflow, ensure that the above variables are set in your repository. 
2. Copy the `code-review-leaderboard.yml` file to your `.github/workflows` directory.
3. The workflow will automatically run based on the specified triggers and update the pull request with a comment summarizing the code review activity.

# Code Review Leaderboard Winner Workflow

This additional workflow determines the winner of the code review leaderboard based on the highest number of reviews and sends a slack message with the winner's information.

## Workflow Triggers

The workflow is triggered by the following events:
- **Push**: When changes are pushed to the `.github/workflows/code-review-leaderboard-winner.yml` file.
- **Scheduled Events**: Runs at a specified interval to determine the current leaderboard winner.

## Jobs

### 1. Code Review Leaderboard Winner

- **Permissions**: Requires `read-all` permissions.
- **Runs on**: `ubuntu-latest`

#### Steps:
1. **Checkout**: Checks out the repository.
3. **Calculate Last Target Day**: Calculates the last target day based on specified variables.
4. **Count Reviews**: Counts the reviews for pull requests opened or merged since the last target day.
3. **Get Winner**: Determines the user with the highest score based on reviews: approvals, and changes requested.
4. **Announce Winner**: Send slack message with winner information.

## Variables

Ensure the following variables are set for this workflow:

- `CODE_REVIEW_LEADERBOARD_START_DAY`: The day of the week to start counting reviews (0 = Sunday, 6 = Saturday). Default is `5` (Friday).
- `CODE_REVIEW_LEADERBOARD_START_HOUR`: The hour of the day to start counting reviews (0-23). Default is `7` (7:00 AM UTC).
- `CODE_REVIEW_LEADERBOARD_START_MINUTE`: The minute of the hour to start counting reviews (0-59). Default is `0`.
- `MAIN_SLACK_CHANNEL_ID`: The ID of the Slack channel where notifications should be sent.

## Secrets

- `SLACK_NOTIFICATIONS_BOT_TOKEN`: A Slack bot token used to send notifications to the specified Slack channel.

## Usage

1. To use this workflow, ensure that the above variables and secrets are set in your repository. 
2. Copy the `code-review-leaderboard-winner.yml` file to your `.github/workflows` directory.
3. Set the cron schedule for the workflow to run at the desired interval. By default it runs every Friday at 7:00 AM UTC. (Cron cannot be controlled by GitHub variables, so whenever you want to change it you have to do it in the workflow code :()
```
on:
  schedule:
    - cron: '0 7 * * 5' # Runs every Friday at 7 AM UTC
```
4. The workflow will automatically run based on the specified triggers.
