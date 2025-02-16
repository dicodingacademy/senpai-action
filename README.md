# SenpAI (先輩) - Senior Programmer AI

![GitHub Action](https://img.shields.io/badge/GitHub%20Action-AI%20Code%20Review-blue)
![Gemini](https://img.shields.io/badge/Powered%20By-Google%20Gemini-orange)

SenpAI is an AI-powered GitHub Action that automatically reviews pull requests using Google's Gemini AI model. It provides detailed, line-by-line feedback on code quality, security, and performance.

---

## Features

- **AI-Powered Code Reviews**: Uses Google Gemini for intelligent code analysis
- **Customizable**: Set review language, tone, and exclusions
- **Line-by-Line Feedback**: Comments directly on problematic code
- **Trigger Support**: Run via `/review` command or automatically
- **File Exclusions**: Skip files using glob patterns
---

## Usage
### 1. Get Your Gemini API Key:

Sign up for an API key at [Google AI Studio](https://makersuite.google.com/app/apikey) if you don’t have one.

### 2. Add the API Key to GitHub Secrets:
- Click on Settings > Secrets and variables > Actions.
- Click New repository secret.
- Name it GEMINI_API_KEY and paste your API key in the value field.
- Click Add secret.

### 3. Add to Your Workflow

Create `.github/workflows/review.yml`:

```yaml
name: SenpAI Code Review
on:
  pull_request:
    types: [opened, synchronize, reopened]
  issue_comment:
    types: [created]

jobs:
  code-review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
      
      - name: Run SenpAI Review
        uses: dicodingacademy/senpai-action@latest
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
          GEMINI_MODEL: 'gemini-1.5-flash-002'
          EXCLUDE_FILES: '*.md, *.json, *.lock'
          TRIGGER_COMMAND: '/review'
          LANGUAGE_REVIEW: 'english'
          TONE_RESPONSE: 'professional'
