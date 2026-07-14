name: Generate Snake & Pacman Animations

on:
  schedule:
    - cron: "0 */6 * * *"   # runs every 6 hours
  workflow_dispatch: {}
  push:
    branches:
      - main

jobs:
  generate-snake:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    steps:
      - name: Generate Snake SVG
        uses: Platane/snk@v3
        id: snake-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg

      - name: Push snake.svg to snake-output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: snake-output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  generate-pacman:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    steps:
      - name: Generate Pacman SVG
        uses: Platane/snk@v3
        id: pacman-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/pacman-contribution-graph.svg?palette=github-light&color_snake=orange&color_dots=purple
            dist/pacman-contribution-graph-dark.svg?palette=github-dark&color_snake=orange&color_dots=purple

      - name: Push pacman svgs to pacman-output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: pacman-output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
