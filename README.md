# PapyonApps's Github Actions for Expo Managed Projects

The repository contains a set of Github Actions that can be used to automate the build and deployment of Expo Managed Projects.

## Usage

You need to add the following secrets to your repository:

- `EXPO_TOKEN`: The token used to authenticate with Expo. You can get it by this link: https://expo.io/settings/access-tokens

- `EXPO_APPLE_APP_SPECIFIC_PASSWORD`: The password used to authenticate with Apple Developer Portal for submit to App Store. You can get it by this link: https://appleid.apple.com/account/manage


### Preview QR for Pull Requests

This action will build your project and upload the generated QR code as a comment on the pull request.

```yaml
## preview.yml

name: Preview QR
on:
  pull_request:
    branches:
      - dev

jobs:
  call-workflow:
    name: Call Preview Workflow
    uses: papyon-apps/gh-actions/.github/workflows/preview.yml@main
    secrets:
      EXPO_TOKEN: ${{ secrets.EXPO_TOKEN }}
```


### Build and Deploy



This action will build your project, increment the build number and deploy it to the App Store (to Test Flight) and Google Play Store (to Internal Testing).

```yaml
### deploy.yml
name: Release App

on:
    push:
      branches:
        - master
        - main

jobs:
  call-workflow:
    name: Call Release Workflow
    uses: papyon-apps/gh-actions/.github/workflows/release.yml@main
    secrets:
      EXPO_TOKEN: ${{ secrets.EXPO_TOKEN }}
      EXPO_APPLE_APP_SPECIFIC_PASSWORD: ${{ secrets.EXPO_APPLE_APP_SPECIFIC_PASSWORD }}

```

### Build IOS development client for testing (Optional)

Sometimes we need new development client builds for testing. This action will trigger a new development client build when a new commit included `[need build]` in the commit message.

```yaml
### build-ios.yml

name: IOS Development Build
on:
  pull_request:
    branches:
      - dev


# `[need build]` is a keyword that will trigger the build
jobs:
  call-workflow:
    name: Call IOS Build Workflow
    uses: papyon-apps/gh-actions/.github/workflows/build-ios.yml@main
    secrets:
      EXPO_TOKEN: ${{ secrets.EXPO_TOKEN }}

```