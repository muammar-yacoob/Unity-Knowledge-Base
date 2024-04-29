# How to Publish a Unity Package on GitHub using Package-Installer.Glitch.Me

## Overview

This guide outlines the steps for publishing a Unity package on GitHub and making it easy for users to install using the [Package-Installer.Glitch.Me](http://package-installer.glitch.me/) service.

## Steps

### 1. Create or Update Your Unity Package

- Make sure your Unity package is properly configured and contains a `package.json` file.

### 2. Upload Package to GitHub

- Initialize a GitHub repository.
- Commit your Unity package to the repository.
- Push the repository to GitHub.

### 3. Release Your Package on GitHub

- Go to the "Releases" section of your GitHub repository.
- Click on "Draft a new release."
- **Tag Version**: Enter a new tag based on the semantic versioning (e.g., `v1.0.0`). GitHub will create a Git tag for this version.
  - Make sure this tag corresponds with the version number in your `package.json`.
- **Release Title**: Optionally give your release a title.
- **Release Description**: Provide a detailed description of the changes, features, and fixes introduced in this version.
- **Attach Files**: Optionally, you can attach the Unity package files to the release.
- Click on "Publish release" to finalize the release.

With the tag in place, this specific release is now easy to identify and download.

### 4. Register on OpenUPM (Optional)

- If you want your package to be available via OpenUPM, submit it to OpenUPM.

### 5. Generate Package-Installer URL

- Use the Package-Installer service to generate a URL for your package.
- Example URL: http://package-installer.glitch.me/v1/installer/OpenUPM/com.sparkgames.sketchfabbrowser?registry=https://package.openupm.com


### 6. Update README.md on GitHub

- Add the generated Package-Installer URL to your GitHub `README.md` to make it easy for users to install the package.

> ***Warning**: you may need to restart unity if the package looks empty after installation*

