---
title: "Oneplus 8 VoLTE (Android 11)"
date: 2022-04-22T13:30:00+08:00
last_modified_at: 2023-08-20T01:30:00+08:00
categories:
  - Blog
tags:
  - Android
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

## References:
1. [Activate VoLTE/VoWiFi OOS 11(OB1&OB2)](https://forum.xda-developers.com/t/guide-activate-volte-vowifi-oos-11-ob1-ob2.4223967/)
2. [VoLTE in latest Android 11 OOS 11 Beta 1 for Oneplus 6](https://www.reddit.com/r/oneplus/comments/ogr7wr/volte_in_latest_android_11_oos_11_beta_1_for/)
3. [Bell, Telus, Koodo and Freedom Mobile Volte and Vowifi Calling Working *Update Android 11 Require Root*](https://forum.xda-developers.com/t/bell-telus-koodo-and-freedom-mobile-volte-and-vowifi-calling-working-update-android-11-require-root.4153631/)
4. [Enable VoLTE & VoWiFi on all devices NO ROOT.](https://forum.xda-developers.com/t/guide-enable-volte-vowifi-on-all-devices-no-root.4105817/)





[link]: https://forum.xda-developers.com/t/pdc-without-root-on-android-11.4200263/post-84103949