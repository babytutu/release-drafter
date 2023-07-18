# Release Drafter

通过设置[Release Drafter](https://github.com/release-drafter/release-drafter)实现推送到指定分支后自动生成release草稿


## 配置workflow

新建yml文件，.github/workflows/release-drafter.yml

```yml
name: Release Drafter

on:
  push:
    # branches to consider in the event; optional, defaults to all
    branches:
      - master

permissions:
  contents: read

jobs:
  update_release_draft:
    permissions:
      # write permission is required to create a github release
      contents: write
      # write permission is required for autolabeler
      # otherwise, read permission is required at least
      pull-requests: write
    runs-on: ubuntu-latest
    steps:
      # (Optional) GitHub Enterprise requires GHE_HOST variable set
      #- name: Set GHE_HOST
      #  run: |
      #    echo "GHE_HOST=${GITHUB_SERVER_URL##https:\/\/}" >> $GITHUB_ENV

      # Drafts your next Release notes as Pull Requests are merged into "master"
      - uses: release-drafter/release-drafter@v5
        # (Optional) specify config name to use, relative to .github/. Default: release-drafter.yml
        # with:
        #   config-name: my-config.yml
        #   disable-autolabeler: true
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```


## 配置release模版

新建yml文件，.github/release-drafter.yml

```yml
categories:
  - title: '🚀 Features'
    label: 'feat'
  - title: '🐛 Bug Fixes'
    labels:
      - 'fix'
      - 'bug'
template: |
  ## What's Changed

  $CHANGES
```

## 使用方式

新建分支，推送变更后，执行`Pull requests`, 标记labels来分组