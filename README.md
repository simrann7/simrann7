name: 🚀 Update GitHub Profile

on:
schedule:
- cron: "17 2 * * *"
workflow_dispatch:
push:
branches:
- main

permissions:
contents: write

jobs:
build-profile:
runs-on: ubuntu-latest

```
steps:
  - name: 📥 Checkout
    uses: actions/checkout@v6

  - name: 📁 Create profile directory
    run: mkdir -p profile

  # -------------------------------------------------------
  # GITHUB STATS
  # -------------------------------------------------------
  - name: 📊 Generate GitHub Stats
    uses: stats-organization/github-readme-stats-action@v2
    with:
      card: stats
      options: username=simrann7&show_icons=true&include_all_commits=true&count_private=true&rank_icon=github&hide_border=true&theme=transparent
      path: profile/stats.svg
      token: ${{ secrets.GITHUB_TOKEN }}

  # -------------------------------------------------------
  # TOP LANGUAGES
  # -------------------------------------------------------
  - name: 💻 Generate Top Languages
    uses: stats-organization/github-readme-stats-action@v2
    with:
      card: top-langs
      options: username=simrann7&layout=compact&langs_count=8&hide_border=true&theme=transparent
      path: profile/top-langs.svg
      token: ${{ secrets.GITHUB_TOKEN }}

  # -------------------------------------------------------
  # CONTRIBUTION STREAK
  # -------------------------------------------------------
  - name: 🔥 Generate Contribution Streak
    uses: zients/github-readme-streak-stats@v2
    with:
      options: user=simrann7&theme=transparent&hide_border=true&disable_animations=true&card_width=500&card_height=195
      path: profile/streak.svg
      token: ${{ secrets.GITHUB_TOKEN }}

  # -------------------------------------------------------
  # TROPHIES
  # -------------------------------------------------------
  - name: 🏆 Generate GitHub Trophies
    uses: ryo-ma/github-profile-trophy@master
    with:
      username: simrann7
      output_path: profile/trophy.svg
      token: ${{ secrets.GITHUB_TOKEN }}
      theme: onedark

  # -------------------------------------------------------
  # CONTRIBUTION SNAKE
  # -------------------------------------------------------
  - name: 🐍 Generate Contribution Snake
    uses: Platane/snk@v3
    with:
      github_user_name: simrann7
      outputs: |
        profile/github-snake.svg?palette=github-light&color_snake=%2358a6ff
        profile/github-snake-dark.svg?palette=github-dark&color_snake=%2358a6ff

  # -------------------------------------------------------
  # COMMIT GENERATED FILES
  # -------------------------------------------------------
  - name: 💾 Commit generated profile assets
    run: |
      git config user.name "github-actions[bot]"
      git config user.email "41898282+github-actions[bot]@users.noreply.github.com"

      git add profile/

      if git diff --cached --quiet; then
        echo "No changes to commit."
        exit 0
      fi

      git commit -m "chore: update GitHub profile analytics"
      git push
```
