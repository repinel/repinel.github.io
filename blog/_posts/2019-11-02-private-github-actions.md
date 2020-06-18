---
layout:      post
title:       Using GitHub Actions from private repositories
description: |-
  GitHub Actions from private repositories cannot be directly used by other
  repositories, even if they belong to the same organization. Let's explorer
  a workaround.
date:        2019-11-02 13:03
categories:  github actions workflow private
---

**Updated on 06/18/2020:** `actions/checkout@v2` can now handle private repositories. [See the updated solution](#updated-on-06-18-2020).

[GitHub Actions][github-actions] are awesome. The ability of build workflows
nicely coupled to source code and backed by cloud computing is truly
awesome. No wonder the feature will be moving to general availability on
November 13.

I was surprised to noticed this week that you cannot use a GitHub action defined
in a private repository like you would normally do with a public action.

```yaml
name: My workflow
on: push
jobs:
  do-something:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: my-organization/awesome-action@master
      - name: Last step
        run: ...
```

The workflow above will raise a `404` error when trying to use
`my-organization/awesome-action@master` if `my-organization/awesome-action` is a
private repository, even if the workflow's repository is part of the same
organization (`my-organization`). Okay, let's try checking out the
`awesome-action` and point the step to its local path in the container.

```yaml
name: My workflow
on: push
jobs:
  do-something:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: actions/checkout@v1
        with:
          repository: my-organization/awesome-action
          ref: refs/heads/master
          # GitHub's personal access token with access to `my-organization/awesome-action`
          token: ${{ secrets.GIT_TOKEN }}
          path: ./.github/actions/awesome-action
      - uses: ./.github/actions/awesome-action
      - name: Last step
        run: ...
```

The `actions/checkout@v1` is able to checkout `my-organization/awesome-action`,
but the path provided won't be defined in the next step that tries to use the
`awesome-action`. Someone had reported a similar [issue][actions-checkout-issue]
but no one from the GitHub team has reached out. How about a more manual approach?

```yaml
name: My workflow
on: push
jobs:
  do-something:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: Checkout my-organization/awesome-action
        run: git clone --single-branch --no-tags --depth 1 -b master https://${{ secrets.GIT_USER }}:${{ secrets.GIT_TOKEN }}@github.com/my-organization/awesome-action ./.github/actions/awesome-action
      - uses: ./.github/actions/awesome-action
      - name: Last step
        run: ...
```

Success! The private repository `my-organization/awesome-action` is correctly
cloned to the provided path and the next step is able to use the
`awesome-action`. The `git` command uses `secrets.GIT_USER` and
`secrets.GIT_TOKEN` to be able to access the repository. They were definied in
the workflow's repository settings. `secrets.GIT_TOKEN` is the GitHub's personal
access token with access to `my-organization/awesome-action` and
`secrets.GIT_USER` its owner.

## Updated on 06/18/2020
You can use `actions/checkout@v2` to checkout private repositories as first attempted above. The cleaner solution would be:
```yaml
name: My workflow
on: push
jobs:
  do-something:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Checkout my-organization/awesome-action
        uses: actions/checkout@v2
        with:
          repository: my-organization/awesome-action
          ref: refs/heads/master
          # GitHub's personal access token with access to `my-organization/awesome-action`
          token: ${{ secrets.GIT_TOKEN }}
          persist-credentials: false
          path: ./.github/actions/awesome-action
      - uses: ./.github/actions/awesome-action
      - name: Last step
        run: ...
```

[github-actions]: https://github.com/features/actions
[actions-checkout-issue]: https://github.com/actions/checkout/issues/57
