name: generate animation

on:
  # run automatically every 24 hours
  schedule:
    - cron: "0 */24 * * *"

    # allows to manually run the job any time
    workflow_dispatch:

    # run on every push on the master branch
    push:
      branches:
      - master



jobs:
  generate:
    permissions:
      contents: write
    runs-on:  unbutu-latest
    timeout-minutes:  5

    steps:
      # generates a snake game from a github user (<github_user_name>) contributions graph, output a svg animation at <svg_out_path>
      - name: generate github-contribution-grid-snake.svg
        uses: platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs:  |
            dist/githubs-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark


      # push the content of <build-dir> to a branch
      # the content will be available at https://raw.githubusercontent.com/<github-user>/<repository>/<target-branch>/<file> , or as github page
      - name: push github-contribution-grid-snake.svg to the output branch
        uses: Platane/snk/svg-only@v3
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{  secrets.GITHUB_TOKEN  }}