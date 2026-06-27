# WHERE THIS GOES:
#   In your "audu0132/audu0132" profile repo, create the file:
#     .github/workflows/snake.yml
#   and paste this content in. Commit & push to "main".
#
# WHAT IT DOES:
#   Runs once a day (and on every push) to regenerate an animated SVG
#   of a snake "eating" your contribution graph, and publishes it to
#   a branch called "output" in this same repo.
#
# AFTER FIRST RUN:
#   Go to your repo's Actions tab, confirm "generate animated snake"
#   ran successfully, then the image in your README's snake section
#   will start rendering automatically (it points at the "output" branch).

name: generate animated snake

on:
  schedule:
    - cron: "0 0 * * *"   # once a day at midnight UTC
  push:
    branches:
      - main
  workflow_dispatch: {}    # lets you trigger it manually from the Actions tab

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: generate snake animation
        uses: Platane/snk@v3
        id: snake-gif
        with:
          github_user_name: audu0132
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: push generated files to the "output" branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
