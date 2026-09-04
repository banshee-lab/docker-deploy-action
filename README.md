# Docker Build, Push & Notify Action

A reusable GitHub Composite Action that streamlines the deployment pipeline by building a Docker image, publishing it to any container registry (like GHCR or Docker Hub), triggering a deployment webhook, and sending a Telegram notification upon success.

## Features

- 🚀 **Universal Registry Support:** Works with GitHub Container Registry (`ghcr.io`), Docker Hub, or any other OCI-compliant registry.
- 🏷️ **Smart Auto-Tagging:** Automatically generates a default image tag (`registry/repository:latest`) if none is specified, while allowing custom overrides.
- 🪝 **Webhook Integration:** Automatically triggers a deployment webhook only if the build and push steps succeed.
- 📱 **Telegram Notifications:** Sends a success message to your designated Telegram chat with details about the deployment.

---

## Usage

To use this action in your workflows, reference your repository and the desired version tag or branch (e.g., `@main`).

### Example 1: Basic Usage (Auto-tagging with GHCR)

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build, Push and Notify
        uses: banshee-lab/docker-deploy-action@main
        with:
          username: username
          password: ${{ secrets.GH_TOKEN }}
          webhook_url: ${{ secrets.WEBHOOK_URL }}
          telegram_token: ${{ secrets.TELEGRAM_TOKEN }}
          telegram_to: ${{ secrets.TELEGRAM_TO }}
          commit_message: ${{ github.event.head_commit.message }}