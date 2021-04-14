# Mike approves
> Whenever Mike approves your PR it a special moment

![](mike-approves.gif)

## Usage
Add `mike-approves.yml` into `.github/workflows` and make your PR special.
```yaml
name: Mike approves
on: pull_request_review
jobs:
  mike-approves:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v3
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const reviews = await github.pulls.listReviews({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: ${{ github.event.pull_request.number }}
            });

            let reviewed = false;

            if (Array.isArray(reviews.data)) {
              reviewed = reviews.data.some((c) => (c.user.login === "migstopheles" && c.state === "APPROVED"));
            }

            if (reviewed) {
              await github.pulls.createReview({
                pull_number: ${{ github.event.pull_request.number }},
                owner: context.repo.owner,
                repo: context.repo.repo,
                event: 'COMMENT',
                body: '![](https://raw.githubusercontent.com/matejker/mike-approves/master/mike-approves.gif)'
              })
            }
```