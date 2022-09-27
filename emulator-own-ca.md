- use android sdk without Google Play Services

- download pem from http://mitm.it (accessible from client connected to proxy)
- prepare certificate http://wiki.cacert.org/FAQ/ImportRootCert#Android_Phones_.26_Tablets
- `~/Android/Sdk/tools  ./emulator @Pixel -writable-system`
```
adb root
adb remount
adb shell "mount -o rw,remount /"
adb push c8750f0d.0 /system/etc/security/cacerts/
adb shell "chmod 644 /system/etc/security/cacerts/c8750f0d.0"
```

- Install frida-server

```
adb root
adb push frida-server-15.2.2-android-x86_64 /data/local/tmp/frida-server
adb shell chmod +x /data/local/tmp/frida-server
adb shell "/data/local/tmp/frida-server &"
```