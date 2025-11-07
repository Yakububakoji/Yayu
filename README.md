#yayu# GitHub Actions workflow to build a site (using Node) and publish to GitHub Pages.
# Adjust the build step for your framework (npm run build, hugo, etc.)
on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: |
          if [ -f package.json ]; then npm ci; fi

      - name: Build site
        run: |
          # Replace with your build command if different
          if [ -f package.json ]; then npm run build --if-present; fi

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
          # If your built files end up at a different path (e.g., ./public or ./), change publish_dir accordingly
