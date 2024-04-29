# ADB Over Wi-Fi

### Setup
- For convienince, add `adb.exe` location to your environment variables path. You can find it from `Unity Editor > Preferences > External Tools > Android SDK Tools Installed with Unity (recommended)` and click on `Copy Path` and paste it in windows: `Environment Variables > System Variables > New`

![Android SDK](/_res/ADB/UnityAndroidSDKLocation.jpg)

![Environment Variable](/_res/ADB/SDKEnvVars.jpg)

### Connection Over Wi-Fi
- Connect Oculus with usb cable to PC
- Shell: `adb tcpip 5555` to restart connection in TCP mode on Port 5555
- Shell: `adb shell ip route` and copy the device IP address after `link src`
- Shell: `adb connect device.ip.address:port`. You can now unplug your usb cable

### Installation and Debugging
- Install: `adb install -r my-android-app.apk` To install your app.
- Launch: `adb shell monkey -p "com.package.name" -c android.intent.category.LAUNCHER 1`
- Debug: `adb logcat -e unity` To connect the debugger

>Notes 📝
>`adb devices -l` should now list the IP addresses instead of the devices names 
>  For the installation, you need to be in the same directory otherwise you will need to provide the full path to the apk file
>  - In case you have more than one device connected, you can target the installation by IP Address `adb -s 192.168.1.216:5555 my-android-app.apk`


Here is an example of the process in `MS Dos` but you can use `Bash` or `Windows Power Shell`  just make sure you launch your shell terminal with sufficient privileges
Getting device socket

``` console
D:\Spark>adb shell ip route
192.168.1.0/24 dev wlan0 proto kernel scope link src 192.168.1.216
```

Connect Over WIFI: `adb connect device.ip.address:port`

``` console
D:\Spark>adb connect 192.168.1.216:5555
connected to 192.168.1.216:5555
```

List all connected devices:  `adb devices -l`

``` console
D:\Spark>adb devices -l
List of devices attached
192.168.1.216:5555     device product:hollywood model:Quest_2 device:hollywood transport_id:7
```

Install apk: Go to your build directory and shell command `adb -s device.ip.address:port install my-android-app.apk`

``` console
D:\Spark\Resilience\Build>adb install -r resilience.apk
Performing Streamed Install
Success
```


Launching apk: `adb shell monkey -p "com.package.name" -c android.intent.category.LAUNCHER 1`
``` console
adb shell monkey -p "net.spark.resilience" -c android.intent.category.LAUNCHER 1
```

Debug a unity build: `adb logcat -e unity`. You can stop debugging by pressing [Ctrl + Break]

``` console
D:\Spark\Resilience\Build>adb logcat -e unity
--------- beginning of system
--------- beginning of tracking
--------- beginning of main
```

