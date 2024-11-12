# Uiua Doc Gen Action

GitHub Action to generate documentation of your Uiua library for deployment to GitHub Pages.

# Usage

1. In your repository settings, find "Pages" configuration in the sidebar.
2. Set the "Build and deployment" source to "GitHub Actions".
3. Create the file ".github/workflows/deploy-docs.yml" in the repository.
4. Set it's contents to:
    ```yaml
    name: Deploy to GitHub Pages

    on:
      # Trigger the workflow every time you push to the `main` branch
      # Using a different branch name? Replace `main` with your branch’s name
      push:
        branches: [ main ]
      # Allows you to run this workflow manually from the Actions tab on GitHub.
      workflow_dispatch:

    # Allow this job to clone the repo and create a page deployment
    permissions:
      contents: read
      pages: write
      id-token: write

    # Allow only one concurrent deployment
    concurrency:
      group: "pages"
      cancel-in-progress: true

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout your repository using git
            uses: actions/checkout@v4

          - name: Setup Pages
            uses: actions/configure-pages@v4

          - name: Build the website
            uses: ekgame/uiua-doc-gen-action@main

          - name: Upload artifact
            uses: actions/upload-pages-artifact@v3
            with:
              path: ./doc-site

      deploy:
        needs: build
        runs-on: ubuntu-latest
        environment:
          name: github-pages
          url: ${{ steps.deployment.outputs.page_url }}
        steps:
          - name: Deploy to GitHub Pages
            id: deployment
            uses: actions/deploy-pages@v4
    ```
5. Shortly after pushing the file to GitHub, the action should run automatically.
6. After a couple of minutes, the website should be available at `https://<your-github-yusername>.github.io/<your-repository-name>/`.