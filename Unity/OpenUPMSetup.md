## Preface
Since unity maintains it's own manifest.json file, using scope registeries, such as OpenUPM discussed in this article, provides users with the conviencence of having all relative package dependencies installed hassle free.
## Preparing Package
1. **File setup:** Inside a Unity project, create a new directory for your package within Unity's Packages directory, such as `com.sparkgames.multiplay`. This approach allows you to see your package as an imported project asset, while retaining the ability to modify it.

2. **Prepare the Package:** Within the `sparkgames.multiplay` directory, add a `package.json` file.
    ```json
    {
        "name": "com.sparkgames.multiplay",
        "version": "1.0.0",
        "displayName": "MultiPlay",
        "description": "Launches multiple unity editors simultanously for multiplayer testing.",
        "unity": "2020.1",
        	"keywords": ["Multiplayer","Photon","Mirror","PUN", "Editor","Multiple"]
        "author": {
            "name": "Spark Games",
            "url": "https://spark-games.co.uk"
            },

        "dependencies" : {
            "com.cysharp.unitask": "2.3.3",
            "jp.hadashikick.vcontainer": "1.13.2"
        }
    }
    ```

3. **Initialize Git:**. Add a remote repository, commit your changes, tag your commit with a semantically meaningful tag (e.g. `v1.0.0`), and push the commit and tag to the remote repository.
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
3. **GitHub Release:** Create a new release on GitHub and upload your .unitypackage file. 
    > **Note:** this file is temporary and will be replaced in a subsequent step.


4. **OpenUPM Registration:** Go to https://openupm.com/packages/com.sparkgames.multiplay/ Once you've tagged a release in your repository, any warnings related to the absence of a release should disappear.

5. **Package Installer:** Run the following API call to download a file and use it as replacement of the release package you uploaded in step 3 above. Rename this file to "Release-Latest" as it will always be the built on request.

    http://package-installer.glitch.me/v1/installer/OpenUPM/com.sparkgames.multiplay?registry=https://package.openupm.com

## Installation

Using UnityPackage: Drag and drop the Release-Latest.unitypackage file into your Unity project window.

Using OpenUPM: Open the terminal in your IDE and execute the following commands to install the OpenUPM CLI (if not already installed), search for your package, and add it to your project:
``` bash
npm install -g openupm-cli
openupm search net.born.ai
openupm add net.born.ai
```