name: Generate Profile Assets

on:
  schedule:
    - cron: "0 0 * * *"   # runs once a day
  workflow_dispatch:        # lets you trigger it manually
  push:
    branches:
      - main

jobs:
  generate:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      # ---------- Snake animation ----------
      - name: Generate snake animation
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: dist-snake/snake.svg

      - name: Push snake to snake-output branch
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist-snake
          publish_branch: snake-output
          force_orphan: true

      # ---------- GitHub stats card ----------
      - name: Generate stats card
        uses: stats-organization/github-readme-stats-action@v2
        with:
          card: stats
          options: username=${{ github.repository_owner }}&show_icons=true&theme=dracula
          path: dist-stats/stats.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Push stats to stats-output branch
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist-stats
          publish_branch: stats-output
          force_orphan: true

      # ---------- Top languages card ----------
      - name: Generate languages card
        uses: stats-organization/github-readme-stats-action@v2
        with:
          card: top-langs
          options: username=${{ github.repository_owner }}&layout=compact&theme=dracula
          path: dist-langs/languages.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Push languages to languages-output branch
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist-langs
          publish_branch: languages-output
          force_orphan: true

      # ---------- Pac-Man contribution graph ----------
      - name: Generate pacman contribution graph
        uses: abozanona/pacman-contribution-graph@main
        with:
          github_user_name: ${{ github.repository_owner }}

      - name: Push pacman graph to pacman-output branch
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
          publish_branch: pacman-output
          force_orphan: true
