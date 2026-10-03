name: Update GitHub Profile Metrics

on:
  workflow_dispatch:
  schedule:
    - cron: "15 3 * * *"

permissions:
  contents: write

jobs:
  metrics:
    runs-on: ubuntu-latest

    steps:

      # ============================================================
      # GITHUB OVERVIEW
      # ============================================================

      - name: Generate GitHub Overview
        uses: lowlighter/metrics@latest
        with:
          user: OmarMed21

          # Reads your public + private GitHub data
          token: ${{ secrets.METRICS_TOKEN }}

          # GitHub Actions token writes the SVG into this repo
          committer_token: ${{ secrets.GITHUB_TOKEN }}

          # THIS IS THE REAL PATH USED BY YOUR README
          filename: assets/github-overview.svg

          output_action: commit
          committer_branch: main
          committer_message: "chore: update GitHub overview [skip ci]"

          template: classic

          base: header, activity, community, repositories
          base_indepth: yes

          repositories: 200
          repositories_batch: 25

          repositories_affiliations: >
            owner,
            collaborator,
            organization_member

          config_timezone: Europe/Berlin

          retries: 3
          retries_delay: 30
          retries_output_action: 3
          retries_delay_output_action: 30


      # ============================================================
      # CONTRIBUTION CALENDAR
      # ============================================================

      - name: Generate Contribution Calendar
        if: ${{ success() || failure() }}

        uses: lowlighter/metrics@latest
        with:
          user: OmarMed21

          token: ${{ secrets.METRICS_TOKEN }}
          committer_token: ${{ secrets.GITHUB_TOKEN }}

          # THIS IS THE REAL PATH USED BY YOUR README
          filename: assets/github-calendar.svg

          output_action: commit
          committer_branch: main
          committer_message: "chore: update contribution calendar [skip ci]"

          base: ""

          plugin_isocalendar: yes
          plugin_isocalendar_duration: full-year

          repositories: 200
          repositories_batch: 25

          repositories_affiliations: >
            owner,
            collaborator,
            organization_member

          config_timezone: Europe/Berlin

          retries: 3
          retries_delay: 30
          retries_output_action: 3
          retries_delay_output_action: 30
