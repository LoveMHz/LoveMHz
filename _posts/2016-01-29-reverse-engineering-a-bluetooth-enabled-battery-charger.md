---
layout: post
status: publish
published: true
title: Reverse Engineering a Bluetooth Enabled Battery Charger
permalink: /article/reverse-engineering-a-bluetooth-enabled-battery-charger/
author: Dustin
date: '2016-01-29 21:45:18 -0500'
date_gmt: '2016-01-30 02:45:18 -0500'
categories:
- Article
tags: []
img: reverse-engineering-a-bluetooth-enabled-battery-charger/header.png
thumb: generic.png
---
### Overview

Through the never ending stream of _connected_ devices on the market we
have yet another. Today, I look at and tear into the workings of Efest's latest
product, the 
_[Efest LUC BLU6 OLED Intelligent Charger](http://www.efestpower.com/index.php?ac=article&at=read&did=408)._

The goal of this project is to identify, reverse engineer, and evaluate the
safety of this device. Through this process I'll also be writing a simple
interface library in Go as a working exercise in my pursuit to become more
efficient in this new programming language.

<!--more-->

* * *

### What You Will Need

*   Basic Java knowledge
*   Bluetooth LE compatible computer
*   Android Development Environment

* * *

### Analysis

Analysis starts off with looking at companion app. We will be downloading the
Android version of the app due the fact that it's generally easier to
disassemble.

![Google Play](/assets/img/blog/reverse-engineering-a-bluetooth-enabled-battery-charger/google-play.png)

After looking at the device on a mobile device, or via running it inside of
Android x86 and Virtualbox with Bluetooth passthrough, we can see that it's very
simple app that displays information about each charging slot. (Current voltage,
charging current, status, and charging progress chart). Also it has the option
to change each charging slots' charging current.

Now let's peek inside the app. All Android applications are packaged into an APK
file. APK files are simply ZIP archives and can be easily extracted. The easiest
way I've found to download the application's APK is to first find the app on the
Google Play website, retrive the app id, and use a third party website to
download the APK. For example, this app can be found at
https://play.google.com/store/apps/details?id=cn.efest&hl=en. Note the id
parameter in the url. Now that we have the app id we can use a site such as
[apkpure.com](https://apkpure.com/) to download the APK. Once downloaded,
extract the APK as a zip.

![Files](/assets/img/blog/reverse-engineering-a-bluetooth-enabled-battery-charger/files.png)

Once extracted the files should look like the screenshot above. Now the first
thing I do is check to see if it's bundled with any native libs, normally
located in a /libs folder if any exist. Luckily in this case there isn't any.
So at this point we can carry on and disassemble the Java code. The easiest and
quickest method I've found for doing this is a site called 
[http://www.decompileandroid.com/](http://decompileandroid.com)

Great! Now we have the app and the decompiled Java code so let's start looking
around.

_MainActivity.java_

```java
package cn.efest;
import android.bluetooth.BluetoothAdapter;
import android.bluetooth.BluetoothAdapter.LeScanCallback;
...
public class DeviceScanActivity extends ActionBarActivity {
...
   private void scanLeDevice(boolean enable) {
...
      adapter.startLeScan(this.mLeScanCallback);
...
   }
}
```

After looking at the _MainActivity.java_ we can see that it's using the Android
Bluetooth API to do a Bluetooth LE, _low energy_, scan. Knowing this will come
in handy when we start playing around with the connection. Now let's figure out
how data is handled.

_MainActivity.java_
```java
import com.iwit.bluetoothcommunication.util.encodeUtil;
...
public class MainActivity extends ActionBarActivity {
...
    private Pattern mChannelPattern;

    public MainActivity() {
...
        this.mChannelPattern = Pattern.compile("CH(\\d):(\\d),([0-9.]+)V,([0-9]+)mA,([0-9]+)per,(\\d)(\\d)", 2);
...
    }

    private void handleData(byte[] buf) {
        this.notifyString += StringUtil.hexStr2Str(encodeUtil.decodeMessage(buf).toUpperCase());

        /* RE Note: Is the device turning off? */
        if (this.notifyString.contains("AT+OK+SCROFF\r\n")) {
            this.closingOLED = false;
            invalidateOptionsMenu();
            this.notifyString = BuildConfig.FLAVOR;

        /* RE Note: Is this an update? */
        } else if (this.notifyString.contains("\r\n")) {
            String[] arr = this.notifyString.split("\r\n");
            if (arr.length >= REQUEST_ENABLE_BT) {
                parseData(arr[0]);
...
            }
        }
    }

    private void parseData(String data) {
        if (data != null) {
            Matcher m = this.mChannelPattern.matcher(data);
            List<SlotStatus> list = new ArrayList(6);

            while (m.find()) {
                SlotStatus cs = new SlotStatus();
                cs.setName("Slot" + m.group(REQUEST_ENABLE_BT));
                cs.setOrder(Integer.parseInt(m.group(REQUEST_ENABLE_BT)));
                cs.setStatus(Integer.parseInt(m.group(2)));
                cs.setDianYe(m.group(3));
                cs.setDianLiu(Integer.parseInt(m.group(4)));
                cs.setPercent(Integer.parseInt(m.group(5)));
                list.add(cs);
            }
...
        }
    }
}
```

Even better! Looks like data is handled either of two ways through
_handleData()_, it either notifies the app that the device is turning off or
that it has an update which is then passed to _parseData()_ and updates the UI.
Now it does looks like it's calling an internal Java class function
_encodeUtil_. During my research I was unable to find any information on this
encoding scheme, but in the end we will just port this function to Go.

_encodeUtil.java_
```java
package com.iwit.bluetoothcommunication.util;

...
public class encodeUtil {
    public static String decodeMessage(byte[] reciverBytes) {
        int i;
        String decodeData = BuildConfig.FLAVOR;

        int tou = reciverBytes[0];
        reciverBytes[1] = (byte) ((reciverBytes[1] ^ tou) - 1);

        int Onum = tou ^ reciverBytes[2];
        reciverBytes[2] = (byte) Onum;

        for (i = 3; i < reciverBytes.length; i++) {
            reciverBytes[i] = (byte) (reciverBytes[i] ^ Onum);
        }

        for (i = 3; i < reciverBytes.length; i++) {
            int num = reciverBytes[i];
            if (num < 0) {
                num += AccessibilityNodeInfoCompat.ACTION_NEXT_AT_MOVEMENT_GRANULARITY; // ACTION_NEXT_AT_MOVEMENT_GRANULARITY = 256
                reciverBytes[i] = (byte) num;
            }

            String mm = Integer.toHexString(num);
            if (mm.length() == 1) {
                mm = "0" + mm;
            }
            decodeData = new StringBuilder(String.valueOf(decodeData)).append(mm).toString();
        }

        return decodeData;
    }
..
}
```

We can greatly simplify this function in Go while also rewriting the function to
output the string instead of the hex equivalent as does the original function.

_efest6bay.go_
```go
func decodeMessage(reciverBytes []byte) string {
    var decodeData string

    var oNum byte = reciverBytes[0] ^ reciverBytes[2]
    for i := 3; i < len(reciverBytes); i++ {
        decodeData += string(reciverBytes[i] ^ oNum)
    }

    return decodeData
}
```

Now that we have the code ported to Go we can test decoding messages with output
from gattool. Jared Wolff has written a well written article on using gatttool
located
[here](http://www.jaredwolff.com/blog/get-started-with-bluetooth-low-energy/).

(The charger constantly sends updates without having to request data) With the
use of gattool and our decodeMessage() function we should be able to decode
something like this.

```
CH1:3,4.20V,0mA,100per,31,CH2:3,4.20V,0mA,100per,21,CH3:3,4.20V,0mA,100per,21,CH4:3,4.20V,0mA,100per,21,CH5:3,4.20V,0mA,100per,21,CH6:3,4.20V,0mA,100per,31
```

Fantastic, now let's look at the encode function and do the same.

_encodeUtil.java_
```java                 
public static byte[] encodeMessage(byte[] bytes) {
    byte[] encodeByte = new byte[(bytes.length + 3)];

    encodeByte[0] = (byte) 84;
    encodeByte[1] = (byte) bytes.length;
    encodeByte[1] = (byte) (encodeByte[0] ^ (encodeByte[1] + 1));

    int num = (int) (Math.random() * 10.0d);
    encodeByte[2] = (byte) (encodeByte[0] ^ num);

    for (int i = 0; i < bytes.length; i++) {
        encodeByte[i + 3] = (byte) (bytes[i] ^ num);
    }

    for (byte b : encodeByte) {

    }

    return encodeByte;
}
```

_efest6bay.go_

```go
func encodeMessage(bytes []byte) []byte {
    encodeByte := make([]byte, len(bytes) + 3)
    encodeByte[0] = 84

    encodeByte[1] = encodeByte[0] ^ byte(len(bytes) + 1)
    num := byte(rand.Int())

    encodeByte[2] = encodeByte[0] ^ num
    for i := 0; i < len(bytes); i++ {

        encodeByte[i + 3] = bytes[i] ^ num

    }

    return encodeByte
}
```

And there we go! Now we can decode and encode messages to the device. Now
there's two functions left that I haven't coded in Go yet, the setCurrent() and
queryChart(). For these implementations feel free to check the project on my
[GitHub](https://github.com/LoveMHz/Efest6bay)!

```java
private void setCurrent(int order, int value) {
    SampleGattAttributes.sendMessage(getAppContext().getBluetoothService().getBluetoothGatt(), StringUtil.str2HexStr("AT+CRT" + order + BuildConfig.FLAVOR + value + "\r\n"));
...
}

private void queryChart(int channel) {
    SampleGattAttributes.sendMessage(getAppContext().getBluetoothService().getBluetoothGatt(), StringUtil.str2HexStr("AT+CH" + channel + "+chart\r\n"));
    this.notifyString = BuildConfig.FLAVOR;
}
```
* * *

That pretty much concludes the bases of reverse engineering this charger. Over
the new couple of weeks I will be uploading my Go interface library to
[GitHub](https://github.com/LoveMHz/Efest6bay). If anything interesting comes
out of fuzzing commands to the device I'll make a new post with all the details!
