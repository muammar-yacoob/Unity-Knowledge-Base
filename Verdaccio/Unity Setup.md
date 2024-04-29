# Setup a private Unity Package Manager with Unity



## Get your access token

Navigate to the URL of your registry and **Login**: https://upm.spark.net/

Hit the settings icon and then `npm`.

Find your auth token from this line:

`npm config set //upm.spark.net/:_authToken "<this is your auth token>"`


## Access the Registry in Unity

Reference: https://forum.unity.com/threads/npm-registry-authentication.836308/

Create or edit:

`C:/Users/<me>/.upmconfig.toml`

```
[npmAuth."https://upm.spark.net"]

token = "<your auth token>"

email = "<your email>"

alwaysAuth = true
```

### Add registry to Unity

In your project `manifest.json` add the following:

```javascript
"scopedRegistries": [
    {
      "name": "package.openupm.com",
      "url": "https://package.openupm.com",
      "scopes": [
        "com.cysharp.unitask",
        "com.openupm",
        "jp.hadashikick.vcontainer"
      ]
    },
    {
      "name": "upm.spark.net",
      "url": "https://upm.spark.net/",
      "scopes": [
        "com.spark"
      ]
    }
  ]
```

Then install packages:

```javascript
"dependencies": {
    "com.spark.sparkcore": "0.10.1",
```

