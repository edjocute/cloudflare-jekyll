---
title: "Oneplus 8 VoLTE (Android 11)"
date: 2022-04-22T13:30:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Android
  - Oneplus
---

## With PDC + root:

1. Enable mode:
```bash
adb shell
su
setprop sys.usb.config diag,serial_cdev,rmnet,adb
```

2. Open PDC tool:
- Select Device.
- Right click currently `Active` profile &rarr; `Deactivate` &rarr; `Sub0`/`Sub1`.
- Right click new profile &rarr; `SetSelectedConfig` &rarr; `Sub0`/`Sub1`.
- Click Activate.

![PDC](/assets/images/android/pdc.jpg){: .align-center width="75%"}

3. Reboot: 
```
adb reboot
```

## Without root:

Modify Step 1 to:
```bash
adb reboot ftm
adb shell
setprop sys.usb.config diag,diag_mdm,qdss,qdss_mdm,serial_cdev,dpl,rmnet,adb
```



[link]: https://forum.xda-developers.com/t/pdc-without-root-on-android-11.4200263/post-84103949