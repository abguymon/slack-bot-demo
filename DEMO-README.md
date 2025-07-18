# Production Deployment Demo

This directory contains example files that demonstrate how the production deployment workflow works.

## Overview

When a Jira ticket moves to "Approved" status, the deploy bot:
1. Creates a PR in the prod K8s repo to update the image tag
2. Creates a devops Jira ticket for the production deployment

## Files Explained

### `docker-compose.yml`
- **Purpose**: Shows how a service would be configured with configurable image tags
- **Key Feature**: Uses `${IMAGE_TAG}` environment variable to demonstrate tag updates
- **Demo**: Shows the structure of a prod K8s repository

### `k8s-manifests/deployments/demo-service.yaml`
- **Purpose**: Example K8s deployment manifest that would be in the prod K8s repo
- **Key Feature**: Shows exactly which lines would be updated by the PR
- **Demo**: Contains comments showing where image tags and versions would be updated

## How the Workflow Works

### 1. Jira Ticket Approval
```json
{
  "issue": {
    "key": "FOO-123",
    "fields": {
      "status": { "name": "Approved" },
      "imageTag": "v2.0.0",
      "repoUrl": "https://github.com/your-org/demo-service"
    }
  }
}
```

### 2. Slack Button Creation
The bot posts a "Deploy to Production" button with:
- Action ID: `deploy_prod_button`
- Value: JSON with ticket, org, repo, and image tag

### 3. PR Creation
When clicked, the bot creates a PR in the prod K8s repo:
- **Title**: "Update demo-service image tag to v2.0.0"
- **Changes**: Updates image tag from `1.2.3` to `v2.0.0`
- **Files Modified**: `k8s-manifests/deployments/demo-service.yaml`

### 4. DevOps Ticket Creation
The bot creates a Jira ticket in the DEVOPS project:
- **Summary**: "Deploy demo-service v2.0.0 to production"
- **Description**: Links to original ticket and PR

## Example PR Changes

**Before:**
```yaml
image: your-registry/demo-service:1.2.3
```

**After:**
```yaml
image: your-registry/demo-service:v2.0.0
```

## Testing the Demo

1. **Start the demo service**:
   ```bash
   docker-compose up -e IMAGE_TAG=v1.2.3
   ```

2. **Update to new version**:
   ```bash
   docker-compose up -e IMAGE_TAG=v2.0.0
   ```

3. **Check the service**:
   ```bash
   curl http://localhost:8080/version
   curl http://localhost:8080/info
   ```

## Environment Variables Needed

For the actual production deployment workflow, you'll need:

```bash
# GitHub (for PR creation)
GITHUB_TOKEN=ghp_your_token
PROD_K8S_ORG=your-org
PROD_K8S_REPO=prod-k8s-manifests

# Jira (for ticket creation)
JIRA_BASE_URL=https://your-domain.atlassian.net
JIRA_EMAIL=your-email@company.com
JIRA_API_TOKEN=your-jira-api-token
JIRA_DEVOPS_PROJECT_KEY=DEVOPS
```

## Next Steps

1. Set up the actual prod K8s repository
2. Configure the environment variables in your deploy bot
3. Test the workflow with real Jira tickets
4. Monitor the PR and ticket creation process 