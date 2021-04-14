# Mike approves
> When Mike approves your PR it a special moment

![](mike-approves.gif)


```yaml
name: Mike approves
on: pull_request
jobs:
  mike-approves:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v3
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const comments = await github.pulls.listReviews({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: ${{ github.event.pull_request.number }}
            });
            if (comments.some((c) => (c.user.login === "jamie--stewart" && c.state === "APPROVED"))) {
              await github.pulls.createReview({
                pull_number: ${{ github.event.pull_request.number }},
                owner: context.repo.owner,
                repo: context.repo.repo,
                event: 'COMMENT',
                body: '![](https://raw.githubusercontent.com/matejker/mike-approves/master/mike-approves.gif)'
              })
            }

```