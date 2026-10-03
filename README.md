name: Update GitHub Profile Metrics

on:
  # RUN AUTOMATICALLY WHEN THIS WORKFLOW IS CHANGED
  push:
    branches:
      - main
    paths:
      - ".github/workflows/metrics.yml"

  # ALSO ALLOW MANUAL RUNS
  workflow_dispatch:

  # UPDATE EVERY NIGHT
  schedule:
    - cron: "15 3 * * *"

permissions:
  contents: write

jobs:
  metrics:
    runs-on: ubuntu-latest

    steps:

      # ============================================================
      # 1. GITHUB OVERVIEW
      # ============================================================

      - name: Generate GitHub Overview
        uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          user: OmarMed21

          # IMPORTANT:
          # Write directly into repository root.
          filename: github-overview.svg

          base: header, activity, community, repositories
          base_indepth: yes

          repositories: 200
          repositories_batch: 25

          repositories_affiliations: >
            owner,
            collaborator,
            organization_member

          config_timezone: Europe/Berlin

          plugins_errors_fatal: yes


      # ============================================================
      # 2. FULL-YEAR CONTRIBUTION CALENDAR
      # ============================================================

      - name: Generate Contribution Calendar
        if: ${{ success() || failure() }}

        uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          user: OmarMed21

          # IMPORTANT:
          # Also directly in repository root.
          filename: github-calendar.svg

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

          plugins_errors_fatal: yes
