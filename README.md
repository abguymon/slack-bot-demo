# Demo Repository for Deploy Bot

This is a demo repository to test the deploy bot functionality. It contains a GitHub Actions workflow that can be triggered remotely to simulate deployments.

## Setup Instructions

### 1. Create the Repository
1. Create a new repository on GitHub
2. Copy the `.github/workflows/deploy.yml` file to your repository
3. Push the changes

### 2. Configure Environment Variables
In your deploy bot's `.env` file, set these variables:

```env
GH_REPO=your-username/your-repo-name
GH_WORKFLOW_FILE=deploy.yml
GH_PAT=your-github-personal-access-token
```

### 3. Generate GitHub Personal Access Token
1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Generate a new token with these permissions:
   - `repo` (Full control of private repositories)
   - `workflow` (Update GitHub Action workflows)

### 4. Test the Workflow
The workflow can be triggered manually from GitHub Actions tab or via the deploy bot.

## Workflow Details

The `deploy.yml` workflow:
- Accepts `ticket` and `imageTag` as inputs
- Simulates a deployment process
- Creates a deployment summary
- Runs on the branch specified in the trigger

## Testing with Deploy Bot

1. Create a Jira issue with:
   - `status.name` = "Staged"
   - `fields.repoUrl` = "https://github.com/your-username/your-repo-name"
   - `fields.imageTag` = "v1.0.0"
   - `fields.workflowBranch` = "main" (optional, defaults to "main")

2. Send a webhook to your deploy bot's `/jira/webhook` endpoint

3. The bot will post a Slack message with a deploy button

4. Clicking the button will trigger this GitHub workflow

## Manual Testing

You can also test the workflow manually:
1. Go to Actions tab in your GitHub repository
2. Click "Deploy to Staging"
3. Fill in the ticket number and image tag
4. Click "Run workflow" 