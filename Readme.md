# Action Repository

This repository is set up to trigger GitHub webhooks on push, pull request, and merge events. It sends these events to the webhook-repo for processing and display.

## Purpose

This is a test repository designed to generate GitHub webhook events that will be captured by the webhook endpoint in the webhook-repo.

## Setup

1. Create this repository on GitHub

2. Configure the webhook:
   - Go to Settings > Webhooks > Add webhook
   - Payload URL: `https://your-webhook-endpoint.com/webhook` (your webhook-repo URL)
   - Content type: `application/json`
   - Events: Select "Let me select individual events"
     - ✓ Pushes
     - ✓ Pull requests
   - Active: ✓
   - Click "Add webhook"

## Testing the Integration

### Test Push Events

```bash
# Clone this repository
git clone <your-action-repo-url>
cd action-repo

# Make a change
echo "# Test Change" >> test.txt

# Commit and push
git add .
git commit -m "Test push event"
git push origin main
```

### Test Pull Request Events

```bash
# Create a new branch
git checkout -b feature-branch

# Make changes
echo "# Feature" >> feature.txt
git add .
git commit -m "Add feature"

# Push the branch
git push origin feature-branch

# Then create a pull request on GitHub from feature-branch to main
```

### Test Merge Events

1. Create a pull request (as shown above)
2. Review and merge the pull request on GitHub
3. The merge event will be captured by the webhook

## Webhook Event Flow

```
GitHub Action (Push/PR/Merge)
    ↓
GitHub Webhook
    ↓
webhook-repo endpoint (/webhook)
    ↓
MongoDB Storage
    ↓
Web UI Display
```

## Verifying Events

After performing actions on this repository:

1. Go to Settings > Webhooks
2. Click on your webhook
3. Scroll to "Recent Deliveries"
4. You can see each webhook delivery and its response

You should also see the events displayed in the webhook-repo web UI at `https://your-webhook-endpoint.com/`

## Sample Content

This repository includes sample files to help you test:
- `README.md` - This file
- `sample.txt` - A sample text file for testing commits

Feel free to modify these files to generate webhook events.

## Branches

- `main` - Main branch
- Create feature branches for testing pull requests

## Troubleshooting

**Webhook not firing:**
- Check that the webhook is active in repository settings
- Verify the payload URL is correct and accessible
- Check that the correct events are selected

**Events not showing in UI:**
- Verify your webhook-repo is running
- Check the webhook delivery details in GitHub for error messages
- Ensure MongoDB is accessible

## Related Repository

- **webhook-repo**: The Flask application that receives and displays these events
