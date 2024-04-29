# Package Publishing and Consumption Guide #

This guide will walk you through the steps to publish and consume Unity packages for Spark-games using GitHub Packages.

<br>

## 1. Setup `package.json`
First, create a `package.json` file in your Unity package root directory `Assets/Package`.
<br>Here is a sample of how the `package.json` file should look:

``` json
{
  "name": "@spark-games/sparkcore",
  "version": "0.10.1",
  "displayName": "SparkCore",
  "description": "Core library for SparkCore framework.",
  "unity": "2021.3",
  "bugs": {"url": "https://github.com/spark-games/sparkcore/issues"},
  "homepage": "https://github.com/spark-games/sparkcore#readme",
  "keywords": ["Spark", "Core", "Framework"],
  "author": "spark-games",
  "license": "ISC",
  "main": "Runtime/SparkCore.asmdef",
  "repository": {
    "type": "git",
    "url": "ssh://git@github.com:spark-games/sparkcore.git",
    "directory": "com.spark-games.sparkcore"
  },
  "publishConfig": {"registry": "https://npm.pkg.github.com/@spark-games"}
}
```


> Note: the name in `"name": "@spark-games/sparkcore"` should match the package name in the package .asmdf file

<br>

## 2. GitHub Action for Publishing
Next, create a GitHub Actions workflow to publish your package whenever there's a push to the main or npm-package branches. In your `.github/workflows` directory, create a file named `publish-package.yml` with the following contents:

``` yaml
name: Publish Package 📦
on: 
  workflow_dispatch:
  push:
    branches:
      - main
      - npm-package
      
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v1
      with:
        node-version: '12.x'
        registry-url: 'https://npm.pkg.github.com'
        scope: '@spark-games'
    - run: cd Assets/Package && npm ci
    - name: Setup .npmrc file to publish to GitHub Packages
      run: echo "//npm.pkg.github.com/:_authToken=${{ secrets.GH_PACKAGES_TOKEN }}" > Assets/Package/.npmrc
    - run: cd Assets/Package && npm publish
```
<br>

## 3. GitHub Token
In the above workflow, replace `GH_PACKAGES_TOKEN` with your GitHub token that has the necessary permissions `read:packages and write:packages`.

> Note: Set this up in your GitHub repository under `Settings > Secrets`.

<br>

## 4. Package Consumption
To consume this package in a Unity project, you need to point npm to the correct registry for your scope. In the root of your Unity project, create a `.npmrc` file with the following line:

```less
@spark-games:registry=https://npm.pkg.github.com/
```

This guide should help you to set up package publishing for Spark-games. Happy coding! 🚀