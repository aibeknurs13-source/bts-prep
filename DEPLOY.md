# Деплой GitHub Pages

Сейчас Pages собирается напрямую из ветки `main` (Source: *Deploy from a branch*),
поэтому каждый `git push` в `main` сразу публикует сайт.

Если захотите собирать через GitHub Actions — создайте файл
`.github/workflows/pages.yml` с таким содержимым:

```yaml
name: Deploy static site to GitHub Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "."
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

После этого в `Settings → Pages → Build and deployment` выберите Source: **GitHub Actions**.
