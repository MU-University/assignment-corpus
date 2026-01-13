# Assignment Corpus - JPlag Plagiarism Detection

This repository contains a GitHub Actions workflow that automatically runs JPlag plagiarism detection on student submissions and creates publicly accessible download links.

## Features

- **Automated JPlag Analysis**: Runs JPlag on staged student submissions
- **Public Download Links**: Creates GitHub Releases with direct download URLs
- **No CORS Issues**: Download links work from any service without authentication or CORS errors
- **Persistent Storage**: Results are stored as GitHub Releases with downloadable ZIP files

## How It Works

1. The workflow is triggered manually via `workflow_dispatch`
2. It clones the student repository and stages the submission
3. Runs JPlag analysis when there are 2+ submissions to compare
4. Creates a GitHub Release with the results as a downloadable ZIP file
5. Provides a direct download URL that can be accessed from any service

## Accessing JPlag Results

### Direct Download URL Format

```
https://github.com/MU-University/assignment-corpus/releases/download/<TAG_NAME>/jplag-results-<OWNER>-<ASSIGNMENT>.zip
```

### Example Usage

After a workflow run completes, you can find the download URL in:
- The workflow run logs (last step)
- The [Releases page](https://github.com/MU-University/assignment-corpus/releases)

### Using from External Services

Since the download URLs are public GitHub Release assets, they can be accessed from any service without authentication:

```bash
# Download via curl
curl -L -O "https://github.com/MU-University/assignment-corpus/releases/download/<TAG_NAME>/jplag-results-<OWNER>-<ASSIGNMENT>.zip"

# Download via wget
wget "https://github.com/MU-University/assignment-corpus/releases/download/<TAG_NAME>/jplag-results-<OWNER>-<ASSIGNMENT>.zip"
```

```javascript
// Fetch from a web application (no CORS issues)
fetch('https://github.com/MU-University/assignment-corpus/releases/download/<TAG_NAME>/jplag-results-<OWNER>-<ASSIGNMENT>.zip')
  .then(response => response.blob())
  .then(blob => {
    // Process the downloaded file
  });
```

## Manual Workflow Trigger

To trigger the workflow:

1. Go to [Actions](https://github.com/MU-University/assignment-corpus/actions)
2. Select "Submit Assignment (Corpus)"
3. Click "Run workflow"
4. Enter:
   - **owner**: GitHub organization or user (e.g., `MU-University`)
   - **repo**: Student repository name (e.g., `student2-corpus`)
   - **ref**: Branch or tag (default: `master`)

## Benefits Over GitHub Actions Artifacts

### GitHub Actions Artifacts (Previous Approach)
- ❌ Require authentication to download
- ❌ Expire after 90 days
- ❌ Cause CORS errors when accessed from web applications
- ❌ Need GitHub API tokens for programmatic access

### GitHub Releases (New Approach)
- ✅ No authentication required
- ✅ Permanent storage (until manually deleted)
- ✅ No CORS issues - direct CDN links
- ✅ Simple HTTP/HTTPS download from any service
- ✅ Versioned with tags for easy tracking

## Requirements

- Repository must have `contents: write` permission (already configured)
- Workflow uses `GITHUB_TOKEN` automatically provided by GitHub Actions
