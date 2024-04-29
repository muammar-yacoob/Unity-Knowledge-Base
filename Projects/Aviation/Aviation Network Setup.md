## Preface
This is a brief guide to testing an XR enabled Unity project over a local network setup

## Setup
### Pre-requisites:
- [SparkCore.Networking.Miror](/SparkCore/SparkCore.Networking.Miror.md)
- [XR](/Unity/Unity%20XR.md)
- [MultiPlay](/Third%20Party%20Tools%20%26%20Plugins/MultiPlay.md)

### 1. Server Build
##### Unity Editor Settings
1. in the App Scene, Select the `AppSceneBoot` and in the inspector, check `Override Server`
![Override Server](/_res/Aviation/OverrideServer.jpg)

2. From `File > Build Settings > Dedicated Server` Make a `Windows` server build with the `Development Build` option turned off to ignore expensive debug calls 
** Note: it is recommended that you keep the project target platform as `Android` and build your `Dedicated Server` on a [MultiPlay](/Third%20Party%20Tools%20%26%20Plugins/MultiPlay.md) clone

![Server Build](/_res/Aviation/ServerBuild.jpg)

3. Go to the server build directory
- `Aviation_Data/StreamingAssets` and edit the `config.ini` 
- set `IS_Server = true` and leave the rest of the file unchanged
``` ini
[AviationNetwork]
IS_SERVER = true
```
4. Now go back to the server build excutable file `Aviation.exe` right click it and `Run as administrator` and  **Make sure to allow public and private network**

5. In case you missed step 4, you will need to setup [Windows Firewall Setup](/Networking/Windows%20Firewall%20Setup.md) manually, otherwise; proceed to the next section

### 2. Unity Editor (Teacher Simulation)
1. in the App Scene, Select the `AppSceneBoot` and in the inspector, uncheck `Override Server`
2. In unity, goto: `Spark > Scene Boot Configuration` and select `Local Session` from `Live Sessions` and click `Save`
![Local Session](/_res/Aviation/LocalSessionEditor.jpg)
3. Turn on your paired Oculus headset with AirLink on and hit Play on Unity Editor to join as a teacher

### 3. Unity Editor Clone (Student Simulation)
1. Make sure you have [MultiPlay](/Third%20Party%20Tools%20%26%20Plugins/MultiPlay.md) installed in your project
2. Create and Launch a clone without the `Link Library` Option
3. Now in your clone, go to the App Scene, Select the `AppSceneBoot` and in the inspector, uncheck `Override Server`
4. Hit Play in unity to join as a student
** Note: `GlobalDontDestroyOnLoad.cs` detect if Unity is running on a clone and stop the `XR Service`
```c#
XRGeneralSettings.Instance.Manager.StopSubsystems();  
XRGeneralSettings.Instance.Manager.DeinitializeLoader();
```

#Aviation #XR #Networking #Mirror
